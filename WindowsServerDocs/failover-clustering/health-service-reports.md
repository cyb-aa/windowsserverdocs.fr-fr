---
title: Rapports de Service d’intégrité
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821960"
---
# <a name="health-service-reports"></a>Rapports de Service d’intégrité
> S’applique à Windows Server 2016

## <a name="what-are-reports"></a>Que sont les rapports  

Le Service d’intégrité réduit le travail nécessaire pour obtenir des performances en direct et des informations sur la capacité de votre cluster d’espaces de stockage Direct. Une nouvelle applet de commande fournit une liste organisée des mesures essentielles qui sont collectées de manière efficace et agrégées de façon dynamique entre les nœuds, avec une logique intégrée pour détecter l’appartenance au cluster. Toutes les valeurs sont en temps réel et ponctuelles uniquement.  

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez cette applet de commande pour obtenir des mesures pour l’ensemble du cluster espaces de stockage Direct :

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Le paramètre facultatif **nombre** paramètre indique le nombre de jeux de valeurs à retourner, à intervalles d’une seconde.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

Vous pouvez également obtenir des métriques pour un volume spécifique ou serveur :  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

## <a name="usage-in-net-and-c"></a>L’utilisation de .NET etC#

### <a name="connect"></a>Connection

Pour interroger le Service de contrôle d’intégrité, vous devez établir un **CimSession** avec le cluster. Pour ce faire, vous devez les éléments qui sont uniquement disponibles dans .NET complet, ce qui signifie que vous ne pouvez pas facilement cela directement à partir d’une application web ou mobile. Ces exemples de code utilisera C\#, sont les plus simples choix pour cette couche d’accès aux données.

``` 
...
using System.Security;
using Microsoft.Management.Infrastructure;

public CimSession Connect(string Domain = "...", string Computer = "...", string Username = "...", string Password = "...")
{
    SecureString PasswordSecureString = new SecureString();
    foreach (char c in Password)
    {
        PasswordSecureString.AppendChar(c);
    }

    CimCredential Credentials = new CimCredential(
        PasswordAuthenticationMechanism.Default, Domain, Username, PasswordSecureString);
    WSManSessionOptions SessionOptions = new WSManSessionOptions();
    SessionOptions.AddDestinationCredentials(Credentials);
    Session = CimSession.Create(Computer, SessionOptions);
    return Session;
}
```

Le nom d’utilisateur fourni doit être un administrateur local de l’ordinateur cible.

Il est recommandé que vous construisez le mot de passe **SecureString** directement à partir de l’entrée d’utilisateur dans en temps réel, par conséquent, leur mot de passe n’est jamais stocké en mémoire en texte clair. Cela permet d’atténuer une variété de problèmes de sécurité. Mais dans la pratique, la construction de comme indiqué ci-dessus est courant à des fins de prototypage.

### <a name="discover-objects"></a>Détecter des objets

Avec le **CimSession** établie, vous pouvez interroger Windows Management Instrumentation (WMI) sur le cluster.

Avant de pouvoir obtenir des erreurs ou des mesures, vous devez obtenir des instances de plusieurs objets appropriés. Tout d’abord, le **MSFT\_StorageSubSystem** qui représente les espaces de stockage Direct sur le cluster. À l’utiliser, vous pouvez obtenir chaque **MSFT\_StorageNode** dans le cluster et chaque **MSFT\_Volume**, les volumes de données. Enfin, vous devez le **MSFT\_StorageHealth**, l’intégrité du Service lui-même, trop.

```
CimInstance Cluster;
List<CimInstance> Nodes;
List<CimInstance> Volumes;
CimInstance HealthService;

public void DiscoverObjects(CimSession Session)
{
    // Get MSFT_StorageSubSystem for Storage Spaces Direct
    Cluster = Session.QueryInstances(@"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageSubSystem")
        .First(Instance => (Instance.CimInstanceProperties["FriendlyName"].Value.ToString()).Contains("Cluster"));

    // Get MSFT_StorageNode for each cluster node
    Nodes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageNode", null, "StorageSubSystem", "StorageNode").ToList();

    // Get MSFT_Volumes for each data volume
    Volumes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToVolume", null, "StorageSubSystem", "Volume").ToList();

    // Get MSFT_StorageHealth itself
    HealthService = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageHealth", null, "StorageSubSystem", "StorageHealth").First();
}
```

Ces objets sont les mêmes dans PowerShell, vous obtenez à l’aide des applets de commande comme **Get-StorageSubSystem**, **Get-StorageNode**, et **Get-Volume**.

Vous pouvez accéder à tous les mêmes propriétés, décrites à l’adresse [Classes API de gestion du stockage](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

Appeler **GetReport** pour commencer la diffusion en continu des exemples d’une liste organisée d’expert des mesures essentielles qui sont collectées de manière efficace et agrégées de façon dynamique entre les nœuds, avec une logique intégrée pour détecter l’appartenance au cluster. Exemples arriveront à chaque seconde par la suite. Toutes les valeurs sont en temps réel et ponctuelles uniquement.

Métriques peuvent être diffusés en continu pour les trois portées : le cluster, n’importe quel nœud ou n’importe quel volume.

La liste complète des métriques disponibles dans chaque étendue dans Windows Server 2016 est documentée ci-dessous.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Cet exemple de code utilise le [modèle de Design Observateur](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx) pour implémenter une méthode Observer dont **OnNext()** méthode sera appelée l’arrivée de chaque nouvel exemple de mesures. Son **OnCompleted()** méthode sera appelée si/quand se termine de diffusion en continu. Par exemple, vous utiliseriez il pour relancer la diffusion en continu, afin qu’il continue indéfiniment.

```
class MetricsObserver<T> : IObserver<T>
{
    public void OnNext(T Result)
    {
        // Cast
        CimMethodStreamedResult StreamedResult = Result as CimMethodStreamedResult;

        if (StreamedResult != null)
        {
            // For illustration, you could store the metrics in this dictionary
            Dictionary<string, string> Metrics = new Dictionary<string, string>();

            // Unpack
            CimInstance Report = (CimInstance)StreamedResult.ItemValue;
            IEnumerable<CimInstance> Records = (IEnumerable<CimInstance>)Report.CimInstanceProperties["Records"].Value;
            foreach (CimInstance Record in Records)
            {
                /// Each Record has "Name", "Value", and "Units"
                Metrics.Add(
                    Record.CimInstanceProperties["Name"].Value.ToString(),
                    Record.CimInstanceProperties["Value"].Value.ToString()
                    );
            }

            // TODO: Whatever you want!
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Reinvoke BeginStreamingMetrics(), defined in the next section
    }
}
```

### <a name="begin-streaming"></a>Commencer le streaming

Avec l’observateur est défini, vous pouvez commencer la diffusion en continu.

Spécifiez la cible **CimInstance** à laquelle vous souhaitez que les mesures de portée. Il peut être le cluster, n’importe quel nœud ou n’importe quel volume.

Le paramètre de nombre est le nombre d’échantillons avant la fin de la diffusion en continu.

```
CimInstance Target = Cluster; // From among the objects discovered in DiscoverObjects()

public void BeginStreamingMetrics(CimSession Session, CimInstance HealthService, CimInstance Target)
{
    // Set Parameters
    CimMethodParametersCollection MetricsParams = new CimMethodParametersCollection();
    MetricsParams.Add(CimMethodParameter.Create("TargetObject", Target, CimType.Instance, CimFlags.In));
    MetricsParams.Add(CimMethodParameter.Create("Count", 999, CimType.UInt32, CimFlags.In));
    // Enable WMI Streaming
    CimOperationOptions Options = new CimOperationOptions();
    Options.EnableMethodResultStreaming = true;
    // Invoke API
    CimAsyncMultipleResults<CimMethodResultBase> InvokeHandler;
    InvokeHandler = Session.InvokeMethodAsync(
        HealthService.CimSystemProperties.Namespace, HealthService, "GetReport", MetricsParams, Options
        );
    // Subscribe the Observer
    MetricsObserver<CimMethodResultBase> Observer = new MetricsObserver<CimMethodResultBase>(this);
    IDisposable Disposeable = InvokeHandler.Subscribe(Observer);
}
```

Inutile de préciser que ces mesures peuvent être visualisés stockée dans une base de données, ou utilisés en fonction de vos besoins.

### <a name="properties-of-reports"></a>Propriétés des rapports

Chaque échantillon de mesures est un « rapport » qui contient de nombreux « enregistrements » correspondant à des mesures individuelles.

Pour le schéma complet, inspecter la **MSFT\_StorageHealthReport** et **MSFT\_HealthRecord** classes de *storagewmi.mof*.

Chaque mesure a uniquement trois propriétés, conformément à cette table.

| **Propriété** | **Exemple**       |
| -------------|-------------------|
| Nom         | IOLatencyAverage  |
| Value        | 0.00021           |
| Unités        | 3                 |

Unités = {0, 1, 2, 3, 4}, où 0 = « Octets », 1 = « BytesPerSecond », 2 = « CountPerSecond », 3 = « Secondes » ou 4 = « Pourcentage ».

## <a name="coverage"></a>Couverture

Voici les mesures disponibles pour chaque étendue dans Windows Server 2016.

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **Nom**                        | **Unités** |
|---------------------------------|-----------|
| CPUUsage                        | 4         |
| CapacityPhysicalPooledAvailable | 0         |
| CapacityPhysicalPooledTotal     | 0         |
| CapacityPhysicalTotal           | 0         |
| CapacityPhysicalUnpooled        | 0         |
| CapacityVolumesAvailable        | 0         |
| CapacityVolumesTotal            | 0         |
| IOLatencyAverage                | 3         |
| IOLatencyRead                   | 3         |
| IOLatencyWrite                  | 3         |
| IOPSRead                        | 2         |
| IOPSTotal                       | 2         |
| IOPSWrite                       | 2         |
| IOThroughputRead                | 1         |
| IOThroughputTotal               | 1         |
| IOThroughputWrite               | 1         |
| MemoryAvailable                 | 0         |
| MemoryTotal                     | 0         |


### <a name="msftstoragenode"></a>MSFT_StorageNode

| **Nom**            | **Unités** |
|---------------------|-----------|
| CPUUsage            | 4         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |
| MemoryAvailable     | 0         |
| MemoryTotal         | 0         |

### <a name="msftvolume"></a>MSFT_Volume

| **Nom**            | **Unités** |
|---------------------|-----------|
| CapacityAvailable   | 0         |
| CapacityTotal       | 0         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |

## <a name="see-also"></a>Voir aussi

- [Service d’intégrité de Windows Server 2016](health-service-overview.md)
