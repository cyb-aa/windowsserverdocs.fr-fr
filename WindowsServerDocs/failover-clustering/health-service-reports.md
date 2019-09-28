---
title: Rapports de Service de contrôle d’intégrité
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e65db8834bd0b059dc7bbebbcaf9288fb46da225
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369680"
---
# <a name="health-service-reports"></a>Rapports de Service de contrôle d’intégrité
> S’applique à : Windows Server 2019, Windows Server 2016

## <a name="what-are-reports"></a>Présentation des rapports  

Le Service de contrôle d’intégrité réduit le travail nécessaire pour recevoir des informations de performances et de capacité en temps réel à partir de votre cluster espaces de stockage direct. Une nouvelle applet de commande fournit une liste organisée de métriques essentielles, qui sont collectées efficacement et agrégées dynamiquement entre les nœuds, avec une logique intégrée pour la détection de l’appartenance au cluster. Toutes les valeurs sont en temps réel et ponctuelles uniquement.  

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez cette applet de commande pour obtenir les métriques pour l’ensemble du cluster espaces de stockage direct :

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Le paramètre facultatif **Count** indique le nombre de jeux de valeurs à retourner, à un intervalle d’une seconde.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

Vous pouvez également obtenir des métriques pour un volume ou un serveur spécifique :  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

## <a name="usage-in-net-and-c"></a>Utilisation dans .NET etC#

### <a name="connect"></a>Connection

Pour pouvoir interroger le Service de contrôle d’intégrité, vous devez établir un **CimSession** avec le cluster. Pour ce faire, vous aurez besoin de certains éléments qui ne sont disponibles que dans le .NET complet, ce qui signifie que vous ne pouvez pas effectuer cette opération directement à partir d’une application Web ou mobile. Ces exemples de code utilisent C @ no__t-0, le choix le plus simple pour cette couche d’accès aux données.

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

Il est recommandé de construire la **SecureString** de mot de passe directement à partir de l’entrée d’utilisateur en temps réel, de sorte que leur mot de passe ne soit jamais stocké en texte en clair. Cela permet d’atténuer un grand nombre de problèmes de sécurité. Toutefois, dans la pratique, il est courant de les construire comme indiqué ci-dessus à des fins de prototypage.

### <a name="discover-objects"></a>Détecter des objets

Une fois le **CimSession** établi, vous pouvez interroger Windows Management Instrumentation (WMI) sur le cluster.

Avant de pouvoir récupérer des erreurs ou des métriques, vous devez récupérer les instances de plusieurs objets pertinents. Tout d’abord, **msft @ no__t-1StorageSubSystem** qui représente espaces de stockage direct sur le cluster. À l’aide de cela, vous pouvez récupérer chaque **msft @ no__t-1StorageNode** dans le cluster, et chaque **msft @ no__t-3Volume**, les volumes de données. Enfin, vous aurez besoin de **msft @ no__t-1StorageHealth**, le service de contrôle d’intégrité lui-même.

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

Il s’agit des mêmes objets que ceux que vous recevez dans PowerShell à l’aide d’applets de commande, telles que **« StorageSubSystem »** , **« obten-StorageNode**» et **« obtient-volume »** .

Vous pouvez accéder aux mêmes propriétés, documentées dans les classes de l' [API de gestion du stockage](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

Appelez **GetReport** pour commencer à diffuser en continu des exemples d’une liste organisée d’experts des métriques essentielles, qui sont collectées efficacement et agrégées dynamiquement entre les nœuds, avec une logique intégrée pour la détection de l’appartenance au cluster. Les exemples arrivent après chaque seconde. Toutes les valeurs sont en temps réel et ponctuelles uniquement.

Les métriques peuvent être diffusées en continu pour trois étendues : le cluster, n’importe quel nœud ou n’importe quel volume.

La liste complète des métriques disponibles à chaque étendue dans Windows Server 2016 est décrite ci-dessous.

### <a name="iobserveronnext"></a>IObserver. OnNext ()

Cet exemple de code utilise le [modèle de conception observateur](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx) pour implémenter un observateur dont la méthode **OnNext ()** est appelée quand chaque nouvel échantillon de métriques arrive. Sa méthode **OnCompleted ()** est appelée si/à la fin de la diffusion en continu. Par exemple, vous pouvez l’utiliser pour relancer la diffusion en continu, de sorte qu’il continue indéfiniment.

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

### <a name="begin-streaming"></a>Commencer la diffusion en continu

Une fois l’observateur défini, vous pouvez commencer la diffusion en continu.

Spécifiez le **CimInstance** cible pour lequel vous souhaitez étendre les métriques. Il peut s’agir du cluster, de n’importe quel nœud ou de n’importe quel volume.

Le paramètre Count est le nombre d’échantillons avant la fin de la diffusion en continu.

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

Il est inutile de préciser que ces métriques peuvent être visualisées, stockées dans une base de données ou utilisées de la façon que vous voyez.

### <a name="properties-of-reports"></a>Propriétés des rapports

Chaque exemple de métrique est un « rapport » qui contient de nombreux « enregistrements » correspondant à des mesures individuelles.

Pour le schéma complet, examinez les classes **msft @ no__t-1StorageHealthReport** et **msft @ no__t-3HealthRecord** dans *storagewmi. mof*.

Chaque mesure a seulement trois propriétés, par cette table.

| **Propriété** | **Exemple**       |
| -------------|-------------------|
| Nom         | IOLatencyAverage  |
| Value        | 0,00021           |
| Sections        | 3                 |

Units = {0, 1, 2, 3, 4}, où 0 = « octets », 1 = « BytesPerSecond », 2 = « CountPerSecond », 3 = « secondes » ou 4 = « pourcentage ».

## <a name="coverage"></a>Couverture

Voici les mesures disponibles pour chaque étendue dans Windows Server 2016.

### <a name="msft_storagesubsystem"></a>MSFT_StorageSubSystem

| **Nom**                        | **Sections** |
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


### <a name="msft_storagenode"></a>MSFT_StorageNode

| **Nom**            | **Sections** |
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

### <a name="msft_volume"></a>MSFT_Volume

| **Nom**            | **Sections** |
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

- [Service de contrôle d’intégrité dans Windows Server 2016](health-service-overview.md)
