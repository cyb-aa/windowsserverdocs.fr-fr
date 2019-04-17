---
title: "Pannes de Service de contrôle d’intégrité"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: c9e1fb4568ee93739c49ccc1a13106b09161c5f3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-faults"></a>Pannes de Service de contrôle d’intégrité
> S’applique à Windows Server2016

## <a name="what-are-faults"></a>Quelles sont les défauts?

Le Service de contrôle d’intégrité surveille en permanence votre cluster d’espaces de stockage Direct pour détecter les problèmes et générer des «erreurs». Une nouvelle applet de commande affiche toutes les erreurs actuelles, qui vous permet de facilement vérifier l’intégrité de votre déploiement sans examiner chaque entité ou fonctionnalité. Erreurs sont conçues pour être précises, faciles à comprendre et exploitables.  

Chaque erreur contient cinq champs importants:  

-   Gravité
-   Description du problème
-   Étapes suivantes recommandées pour résoudre le problème
-   Informations d’identification de l’entité défaillante
-   Son emplacement physique (si applicable)

Par exemple, voici une erreur type:  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > L’emplacement physique est dérivé de la configuration de votre domaine d’erreur. Pour plus d’informations sur les domaines d’erreur, voir [domaines d’erreur dans Windows Server 2016](fault-domains.md). Si vous ne fournissez pas ces informations, le champ d’emplacement s’avère moins utile, par exemple, il peut afficher uniquement le numéro d’emplacement.  

## <a name="root-cause-analysis"></a>Analyse des causes premières

Le Service de contrôle d’intégrité peut évaluer la causalité potentielle parmi défaillant entités pour identifier et combiner des erreurs qui sont des conséquences du même problème sous-jacent. Reconnaissance d’effet, cela permet d’écourter les rapports. Par exemple, si un serveur est en panne, il est prévu que tous les lecteurs au sein du serveur sera également sans connectivité. Par conséquent, qu’une erreur est déclenchée pour la cause - dans ce cas, le serveur.  

## <a name="usage-in-powershell"></a>Utilisation de PowerShell

Pour afficher toutes les erreurs actuelles dans PowerShell, exécutez la commande suivante:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Cette propriété renvoie toutes les erreurs qui affectent le cluster d’espaces de stockage Direct global. Le plus souvent, ces erreurs sont liées au matériel ou de configuration. S’il n’y a aucune erreur, cette applet de commande ne renvoie rien.  

>[!NOTE]
> Dans un environnement de non-production et à vos propres risques, vous pouvez expérimenter cette fonctionnalité en déclenchant des erreurs vous-même, par exemple, en supprimant un seul disque physique ou en arrêtant un nœud. Une fois que l’erreur est apparue, réinsérez le disque physique ou à redémarrer que le nœud et l’erreur disparaitre à nouveau.

Vous pouvez également afficher les erreurs qui affectent uniquement des volumes ou des partages de fichiers avec les applets de commande suivantes:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Cette propriété renvoie toutes les erreurs qui affectent uniquement le spécifique volume ou partage de fichiers. Le plus souvent, ces erreurs sont liées à la planification de capacité, de résilience des données ou de fonctionnalités telles que la qualité de Service de stockage ou le réplica de stockage. 

## <a name="usage-in-net-and-c"></a>Utilisation de .NET et Visual c#

### <a name="connect"></a>Se connecter

Afin d’interroger le Service de contrôle d’intégrité, vous devez établir un **CimSession** avec le cluster. Pour ce faire, vous devez certaines choses qui sont uniquement disponibles dans .NET complet, ce qui signifie que vous ne pouvez pas facilement cela directement à partir d’un site Web ou application mobile. Ces exemples de code utiliseront c#, le choix plus simple pour cette couche d’accès aux données.

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

Il est recommandé que vous construisez le mot de passe **SecureString** directement à partir de l’entrée utilisateur dans en temps réel, par conséquent, leur mot de passe n’est jamais stocké dans la mémoire en texte clair. Cela permet d’atténuer une variété de problèmes de sécurité. Mais dans la pratique, il la construction ci-dessus est commune à des fins de prototypage.

### <a name="discover-objects"></a>Détecter des objets

Avec la **CimSession** établie, vous pouvez interroger WindowsManagementInstrumentation (WMI) sur le cluster.

Avant de pouvoir obtenir des erreurs ou des mesures, vous devez obtenir des instances de plusieurs objets pertinents. Tout d’abord, la **MSFT\_StorageSubSystem** qui représente les espaces de stockage Direct sur le cluster. À l’aide qui, vous pouvez obtenir chaque **MSFT\_StorageNode** dans le cluster et chaque **MSFT\_Volume**, les volumes de données. Enfin, vous devez le **MSFT\_StorageHealth**, l’intégrité du Service lui-même, trop.

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

Vous pouvez accéder aux mêmes propriétés, documentées dans [Classes des API de gestion de stockage](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

### <a name="query-faults"></a>Erreurs de requête

Appeler **diagnostiquer** pour obtenir toutes les erreurs actuelles à la cible de l’étendue de **CimInstance**, qui est le cluster ou un volume.

La liste complète des défauts disponibles au niveau de chaque étendue dans Windows Server2016 est présentée ci-dessous.

```       
public void GetFaults(CimSession Session, CimInstance Target)
{
    // Set Parameters (None)
    CimMethodParametersCollection FaultsParams = new CimMethodParametersCollection();
    // Invoke API
    CimMethodResult Result = Session.InvokeMethod(Target, "Diagnose", FaultsParams);
    IEnumerable<CimInstance> DiagnoseResults = (IEnumerable<CimInstance>)Result.OutParameters["DiagnoseResults"].Value;
    // Unpack
    if (DiagnoseResults != null)
    {
        foreach (CimInstance DiagnoseResult in DiagnoseResults)
        {
            // TODO: Whatever you want!
        }
    }
}
```

### <a name="optional-myfault-class"></a>Facultatif: MyFault classe

Il peut s’avérer pertinent pour construire et conserver vos propres représentation sous forme de défauts. Par exemple, cela **MyFault** classe stocke plusieurs propriétés de clé de défauts, y compris le **FaultId**, ce qui peut servir ultérieurement pour associer la mise à jour ou supprimer des notifications ou pour dédupliquer dans le cas où l’erreur même est a détecté plusieurs fois, pour une raison quelconque.

```       
public class MyFault {
    public String FaultId { get; set; }
    public String Reason { get; set; }
    public String Severity { get; set; }
    public String Description { get; set; }
    public String Location { get; set; }

    // Constructor
    public MyFault(CimInstance DiagnoseResult)
    {
        CimKeyedCollection<CimProperty> Properties = DiagnoseResult.CimInstanceProperties;
        FaultId     = Properties["FaultId"                  ].Value.ToString();
        Reason      = Properties["Reason"                   ].Value.ToString();
        Severity    = Properties["PerceivedSeverity"        ].Value.ToString();
        Description = Properties["FaultingObjectDescription"].Value.ToString();
        Location    = Properties["FaultingObjectLocation"   ].Value.ToString();
    }
}
```

```
List<MyFault> Faults = new List<MyFault>;

foreach (CimInstance DiagnoseResult in DiagnoseResults)
{
    Faults.Add(new Fault(DiagnoseResult));
}
```

La liste complète des propriétés de chaque erreur (**DiagnoseResult**) est décrit ci-dessous.

### <a name="fault-events"></a>Événements de panne

Lorsque des erreurs sont créés, supprimés ou mis à jour, le Service de contrôle d’intégrité génère des événements WMI. Ces sont essentiels à la synchronisation des état de votre application sans interrogation fréquente et peuvent aider à des éléments tels que de déterminer à quel moment envoyer des alertes par courrier électronique, par exemple. Pour vous abonner à ces événements, cet exemple de code utilise le modèle de conception observateur à nouveau.

Tout d’abord, vous abonner à **MSFT\_StorageFaultEvent** événements.

```      
public void ListenForFaultEvents()
{
    IObservable<CimSubscriptionResult> Events = Session.SubscribeAsync(
        @"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageFaultEvent");
    // Subscribe the Observer
    FaultsObserver<CimSubscriptionResult> Observer = new FaultsObserver<CimSubscriptionResult>(this);
    IDisposable Disposeable = Events.Subscribe(Observer);
}   
```

Ensuite, implémentez un observateur dont **OnNext()** méthode est appelée chaque fois qu’un nouvel événement est généré.

Chaque événement contient **ChangeType** indiquant si une erreur est en cours de création, supprimés ou mis à jour et les **FaultId**.

En outre, ils contiennent toutes les propriétés de l’erreur.

```
class FaultsObserver : IObserver
{
    public void OnNext(T Event)
    {
        // Cast
        CimSubscriptionResult SubscriptionResult = Event as CimSubscriptionResult;

        if (SubscriptionResult != null)
        {
            // Unpack            
            CimKeyedCollection<CimProperty> Properties = SubscriptionResult.Instance.CimInstanceProperties;
            String ChangeType = Properties["ChangeType"].Value.ToString();
            String FaultId = Properties["FaultId"].Value.ToString();

            // Create
            if (ChangeType == "0")
            {
                Fault MyNewFault = new MyFault(SubscriptionResult.Instance);
                // TODO: Whatever you want!
            }
            // Remove
            if (ChangeType == "1")
            {
                // TODO: Use FaultId to find and delete whatever representation you have...
            }
            // Update
            if (ChangeType == "2")
            {
                // TODO: Use FaultId to find and modify whatever representation you have...
            }
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Nothing
    }
}
```

### <a name="understand-fault-lifecycle"></a>Comprendre le cycle de vie des pannes

Erreurs ne servent pas être marquée comme «vue» ou résolue par l’utilisateur. Ils sont créés lorsque le Service de contrôle d’intégrité observe un problème, et ils sont automatiquement supprimés et uniquement lorsque le Service de contrôle d’intégrité peuvent observer n’est plus le problème. En règle générale, cela reflète que le problème a été résolu.

Toutefois, dans certains cas, défauts peuvent être redécouverts par le Service de contrôle d’intégrité (par exemple, après le basculement, ou en raison d’une connectivité intermittente, etc.). Pour cette raison, il peut judicieux de conserver votre propre représentation sous forme de défauts, afin que vous pouvez facilement dédupliquer. Cela est particulièrement important si vous envoyez des alertes par courrier électronique ou l’équivalent.

### <a name="properties-of-faults"></a>Propriétés de défauts

Ce tableau présente plusieurs propriétés de clé de l’objet d’erreur. Pour le schéma complet, inspecter le **MSFT\_StorageDiagnoseResult** classe dans *storagewmi.mof*.

| **Propriété**              | **Exemple**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Raison                    | «Le volume est en cours d’exécution manque d’espace disponible.»                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, emplacement 11                                        |
| RecommendedActions        | {«Développez le volume.», «Migrer les charges de travail à d’autres volumes».}   |

**FaultId** Unique au sein d’un cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {«Information», «Avertissement» et «Erreur»}, ou équivalents couleurs comme rouge, jaune et bleue.

**FaultingObjectDescription** partie des informations pour le matériel, généralement vide pour les objets de logiciel.

**FaultingObjectLocation** informations d’emplacement pour le matériel, généralement vide pour les objets de logiciel.

**RecommendedActions** liste des actions recommandées, qui sont indépendantes et dans aucun ordre particulier. Aujourd'hui, cette liste est souvent de longueur 1.

## <a name="properties-of-fault-events"></a>Propriétés des événements de panne

Ce tableau présente plusieurs propriétés de clé de l’événement d’erreur. Pour le schéma complet, inspecter le **MSFT\_StorageFaultEvent** classe dans *storagewmi.mof*.

Notez le **ChangeType**, ce qui indique si une erreur est créée, supprimé ou mis à jour et le **FaultId**. Un événement contient également toutes les propriétés de l’erreur concerné.

| **Propriété**              | **Exemple**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Raison                    | «Le volume est en cours d’exécution manque d’espace disponible.»                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, emplacement 11                                        |
| RecommendedActions        | {«Développez le volume.», «Migrer les charges de travail à d’autres volumes».}   |

**ChangeType** ChangeType = {0, 1, 2} = {«créer», «Remove», «Mise à jour»}.

## <a name="coverage"></a>Couverture

Dans Windows Server2016, le Service de contrôle d’intégrité fournit la couverture des erreurs suivantes:  

### **<a name="physicaldisk-8"></a>Disque physique (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Gravité: avertissement
* Raison: *«le disque physique a échoué.»*
* RecommendedAction: *«Remplacer le disque physique».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Gravité: avertissement
* Raison: *«Connectivité a été perdue sur le disque physique.»*
* RecommendedAction: *«Vérifiez que le disque physique est correctement connectés et fonctionnement».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Gravité: avertissement
* Raison: *«le disque physique est présentant un dysfonctionnement blocage récurrent.»*
* RecommendedAction: *«Remplacer le disque physique».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Gravité: avertissement
* Raison: *«une défaillance du disque physique est prévue pour se produire dès».*
* RecommendedAction: *«Remplacer le disque physique».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Gravité: avertissement
* Raison: *«le disque physique est mis en quarantaine, car il n’est pas pris en charge par votre fournisseur de solutions.»*
* RecommendedAction: *«Remplacer le disque physique avec le matériel pris en charge».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Gravité: avertissement
* Raison: *«le disque physique est en quarantaine, car sa version du microprogramme n’est pas pris en charge par votre fournisseur de solutions».*
* RecommendedAction: *«Mettre à jour le microprogramme sur le disque physique à la version cible.»*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Gravité: avertissement
* Raison: *«le disque physique a n’est pas reconnu métadonnées.»*
* RecommendedAction: *«ce disque peut contenir des données à partir d’un pool de stockage inconnu. Tout d’abord Vérifiez que n’est pas utile de données sur ce disque, puis le disque de réinitialisation.»*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Gravité: avertissement
* Raison: *«Échec de la tentative de mise à jour du microprogramme sur le disque physique.»*
* RecommendedAction: *«Essayez d’utiliser un microprogramme différents binaire».*

### **<a name="virtual-disk-2"></a>Disque virtuel (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Gravité: information
* Raison: *«certaines données sur ce volume ne sont pas complètement résilientes. Il reste accessible.»*
* RecommendedAction: *«Résilience de restauration des données».*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Gravité: critique
* Raison: *«le volume n’est pas accessible. Certaines données peuvent être perdues.»*
* RecommendedAction: *«vérification de la carte réseau physique et/ou la connectivité de tous les périphériques de stockage réseau. Vous devrez peut-être restaurer à partir de la sauvegarde.»*

### **<a name="pool-capacity-1"></a>Capacité du pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Gravité: avertissement
* Raison: *«le pool de stockage n’a pas la capacité de réserves recommandée. Cela peut limiter votre capacité à restaurer la résilience des données en cas de défaillances de lecteur.»*
* RecommendedAction: *«ajouter une capacité supplémentaire au pool de stockage ou libérer de la capacité. La valeur minimale recommandée réserve varie selon le déploiement, mais est la valeur des lecteurs environ 2 de la capacité.»*

### <a name="volume-capacity-2sup1sup"></a>**Capacité de volume (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravité: avertissement
* Raison: *«le volume est en cours d’exécution manque d’espace disponible.»*
* RecommendedAction: *«Étendre le volume ou migrer les charges de travail à d’autres volumes».*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravité: critique
* Raison: *«le volume est en cours d’exécution manque d’espace disponible.»*
* RecommendedAction: *«Étendre le volume ou migrer les charges de travail à d’autres volumes».*

### **<a name="server-3"></a>Serveur (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* Gravité: critique
* Raison: *«le serveur ne peut pas être atteint».*
* RecommendedAction: *«démarrer ou remplacer le serveur.»*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* Gravité: critique
* Raison: *«le serveur est isolé du cluster en raison de problèmes de connectivité».*
* RecommendedAction: *«Si l’isolation persiste, vérifiez les réseaux ou migrer les charges de travail vers d’autres nœuds.»*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Gravité: critique
* Raison: *«le serveur est mis en quarantaine par le cluster en raison d’échecs périodiques.»*
* RecommendedAction: *«Corriger le problème réseau ou remplacer le serveur.»*

### **<a name="cluster-1"></a>Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Gravité: critique
* Raison: *«le cluster est une défaillance du serveur hors de baisse».*
* RecommendedAction: *«vérifier la ressource témoin et redémarrer si nécessaire. Démarrer ou remplacez des serveurs ayant échouées.»*

### **<a name="network-adapterinterface-4"></a>Carte de réseau (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Gravité: avertissement
* Raison: *«l’interface réseau a été déconnecté.»*
* RecommendedAction: *«Reconnectez le câble réseau».*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Gravité: avertissement
* Raison: *«{serveur} a manquant cartes réseau connectées au réseau de cluster {réseau de cluster}.»*
* RecommendedAction: *«Connecter le serveur au réseau de cluster manquant».*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Gravité: avertissement
* Raison: *«l’interface réseau a eu une défaillance matérielle.»*
* RecommendedAction: *«Remplacer la carte d’interface réseau».*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Gravité: avertissement
* Raison: *«l’interface réseau {réseau} n’est pas activé et n’est pas utilisé.»*
* RecommendedAction: *«activer le l’interface réseau.»*

### **<a name="enclosure-6"></a>Boîtier (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Gravité: avertissement
* Raison: *«Communication a été perdue pour le boîtier de stockage.»*
* RecommendedAction: *«Démarrer ou remplacer le boîtier de stockage».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Gravité: avertissement
* Raison: *«le ventilateur de position la {} du boîtier de stockage a échoué.»*
* RecommendedAction: *«Remplacer le ventilateur sur le boîtier de stockage».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Gravité: avertissement
* Raison: *«le capteur actuel à la position la {} du boîtier de stockage a échoué.»*
* RecommendedAction: *«Remplacement d’un capteur dans le boîtier de stockage en cours».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Gravité: avertissement
* Raison: *«le capteur de tension à la position la {} du boîtier de stockage a échoué.»*
* RecommendedAction: *«Remplacement d’un capteur de tension dans le boîtier de stockage».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Gravité: avertissement
* Raison: *«le contrôleur d’e/s à position la {} du boîtier de stockage a échoué.»*
* RecommendedAction: *«Remplacement d’un contrôleur d’e/s dans le boîtier de stockage».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Gravité: avertissement
* Raison: *«le capteur de température à position la {} du boîtier de stockage a échoué.»*
* RecommendedAction: *«Remplacement d’un capteur de température du boîtier de stockage».*

### **<a name="firmware-rollout-3"></a>Déploiement du microprogramme (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Gravité: avertissement
* Raison: *«Ne peut pas progresser lors du déploiement du microprogramme.»*
* RecommendedAction: *«Vérifier tous les espaces de stockage sont sains, et qu’aucun domaine d’erreur n’est actuellement en mode maintenance».*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Gravité: avertissement
* Raison: *«déploiement du microprogramme a été annulée en raison d’informations sur la version du microprogramme illisibles ou inattendue après avoir appliqué une mise à jour du microprogramme.»*
* RecommendedAction: *«redémarrage microprogramme déployer une fois que le microprogramme a été résolu.»*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Gravité: avertissement
* Raison: *«déploiement du microprogramme a été annulée en raison d’un trop grand nombre de disques physiques Échec d’une tentative de mise à jour du microprogramme.»*
* RecommendedAction: *«redémarrage microprogramme déployer une fois que le microprogramme a été résolu.»*

### <a name="storage-qos-3sup2sup"></a>**Qualité de service de stockage (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Gravité: avertissement
* Raison: *«du débit de stockage est insuffisant pour satisfaire les réserves de ressources».*
* RecommendedAction: *«Reconfigurez les stratégies de QoS de stockage».*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Gravité: avertissement
* Raison: *«le Gestionnaire de stratégie de QoS de stockage a perdu la communication avec le volume.»*
* RecommendedAction: *«, redémarrez les nœuds {nœuds}»*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Gravité: avertissement
* Raison: *«un ou plusieurs consommateurs de stockage (généralement des Machines virtuelles) utilisent une stratégie inexistant avec l’id {id}.»*
* RecommendedAction: *«Recréer les stratégies de QoS de stockage manquants».*

<sup>1</sup> indique le volume a atteint un remplissage à 80 % (gravité mineure) ou à 90 % (gravité majeure).  
<sup>2</sup> indique que certains fichiers .vhd sur le volume n’ont pas atteint leurs e/s minimale pour sur 10 % (mineur), 30 % (majeur) ou 50 % (critique) de fenêtre de 24 heures.  

>[!NOTE]
> L’intégrité des composants de boîtier de stockage tels que les ventilateurs, alimentations et capteurs est dérivée de Services de boîtier SCSI (SES). Si votre fournisseur ne fournit pas ces informations, le Service de contrôle d’intégrité ne peut pas afficher.  

## <a name="see-also"></a>Voir aussi

- [Service de contrôle d’intégrité dans Windows Server 2016](health-service-overview.md)
