---
title: Erreurs de Service de contrôle d’intégrité
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 31a38eacea3af3c0a288d61a77a24b4fa45a1932
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843370"
---
# <a name="health-service-faults"></a>Erreurs de Service de contrôle d’intégrité
> S’applique à Windows Server 2016

## <a name="what-are-faults"></a>Quelles sont les erreurs

Le Service d’intégrité surveille en permanence votre cluster d’espaces de stockage Direct pour détecter les problèmes et générer des « erreurs ». Une nouvelle applet de commande affiche toutes les erreurs actuelles, ce qui vous permet de facilement vérifier l’intégrité de votre déploiement sans examiner chaque entité ou une fonctionnalité à son tour. Les erreurs sont conçues pour être précises, faciles à comprendre et exploitables.  

Chaque erreur contient cinq champs importants :  

-   Sévérité
-   Description du problème
-   Étapes suivantes recommandées pour traiter le problème
-   Informations d’identification de l’entité défaillante
-   Son emplacement physique (si applicable)

Par exemple, voici une erreur type :  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > L’emplacement physique est dérivé de la configuration de votre domaine d’erreur. Pour plus d’informations sur les domaines d’erreur, consultez [domaines d’erreur dans Windows Server 2016](fault-domains.md). Si vous ne fournissez pas ces informations, le champ d’emplacement s’avère moins utile, par exemple, il peut afficher uniquement le numéro d’emplacement.  

## <a name="root-cause-analysis"></a>Analyse de la cause racine

Le Service de contrôle d’intégrité peut évaluer la causalité potentielle parmi défaillante entités pour identifier et combiner des erreurs qui sont des conséquences du même problème sous-jacent. La reconnaissance d’un effet tel en chaîne permet d’écourter les rapports. Par exemple, si un serveur est arrêté, il est probable que tous les lecteurs au sein du serveur sera également sans connectivité. Par conséquent, qu’une erreur est déclenchée pour la cause première-dans ce cas, le serveur.  

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Pour afficher toutes les erreurs actuelles dans PowerShell, exécutez cette applet de commande :

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Cette commande renvoie toutes les erreurs qui affectent le cluster d’espaces de stockage Direct global. En règle générale, ces erreurs sont liées au matériel ou de configuration. S’il n’y a aucune erreur, cette applet de commande ne renvoie rien.  

>[!NOTE]
> Dans un environnement hors production et à vos propres risques, vous pouvez expérimenter cette fonctionnalité en déclenchant des erreurs vous-même, par exemple, en supprimant un seul disque physique ou en arrêtant un nœud. Une fois que l’erreur apparaît, réinsérez le disque physique ou le redémarrage que du nœud et l’erreur disparaitre à nouveau.

Vous pouvez également afficher les erreurs qui affectent uniquement des volumes ou des partages de fichiers avec les applets de commande suivantes :  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Cela renvoie toutes les erreurs qui affectent uniquement le spécifique volume ou partage de fichiers. En règle générale, ces erreurs sont liées à la planification de capacité, la résilience des données ou des fonctionnalités telles que la qualité de Service de stockage ou le réplica de stockage. 

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

### <a name="query-faults"></a>Erreurs de requête

Appeler **diagnostiquer** pour obtenir toutes les erreurs actuelles limitées à la cible **CimInstance**, qui est le cluster ou n’importe quel volume.

La liste complète des erreurs qui sont disponibles dans chaque étendue dans Windows Server 2016 est documentée ci-dessous.

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

### <a name="optional-myfault-class"></a>Facultatif : Classe de MyFault

Il peut être judicieux pour construire et conserver votre propre représentation sous forme d’erreurs. Par exemple, cela **MyFault** classe stocke plusieurs propriétés clés de défauts, y compris le **FaultId**, qui peut être utilisée ultérieurement pour associer la mise à jour ou supprimer des notifications ou dédupliquer dans le cas où les mêmes domaines d’erreur a été détecté plusieurs fois, pour une raison quelconque.

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

La liste complète des propriétés dans chaque erreur (**DiagnoseResult**) est décrite ci-dessous.

### <a name="fault-events"></a>Événements d’erreur

Lorsque des erreurs sont créés, supprimés ou mis à jour, le Service d’intégrité génère des événements WMI. Ceux-ci sont essentiels pour la synchronisation des état de votre application sans la fréquence d’interrogation et peuvent aider à des éléments tels que la détermination des cas d’envoyer des alertes par courrier électronique, par exemple. Pour vous abonner à ces événements, cet exemple de code utilise le modèle de Design observateur à nouveau.

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

Ensuite, implémentez un observateur dont **OnNext()** méthode sera appelée chaque fois qu’un nouvel événement est généré.

Chaque événement contient **ChangeType** qui indique si une erreur est créée, supprimé ou mis à jour et les informations pertinentes **FaultId**.

En outre, ils contiennent toutes les propriétés de l’erreur elle-même.

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

### <a name="understand-fault-lifecycle"></a>Comprendre le cycle de vie d’erreur

Les erreurs ne sont pas destinées à être marqué comme « vue » ou résolue par l’utilisateur. Elles sont créées lorsque le Service d’intégrité observe un problème, et ils sont supprimés automatiquement et uniquement lorsque le Service de contrôle d’intégrité ne sont plus permet d’observer le problème. En règle générale, cela reflète que le problème a été résolu.

Toutefois, dans certains cas, erreurs peuvent être de nouveau détectés par le Service de contrôle d’intégrité (par exemple, après le basculement, ou en raison d’une connectivité intermittente, etc..). Pour cette raison, il peut judicieux pour conserver votre propre représentation sous forme de défauts, donc vous pouvez dédupliquer facilement. Cela est particulièrement important si vous envoyez des alertes par courrier électronique ou équivalent.

### <a name="properties-of-faults"></a>Propriétés d’erreurs

Ce tableau présente plusieurs propriétés clés de l’objet d’erreur. Pour le schéma complet, inspecter la **MSFT\_StorageDiagnoseResult** classe *storagewmi.mof*.

| **Propriété**              | **Exemple**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Raison                    | « Le volume est en cours d’exécution en dehors de l’espace disponible. »                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Monter en rack A06, RU 25, emplacement 11                                        |
| RecommendedActions        | {« Étendre le volume. », « Migrer les charges de travail à d’autres volumes. »}   |

**FaultId** Unique dans l’étendue d’un cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {« Informational », « Avertissement » et « Erreur »}, ou équivalents couleurs telles que bleu, jaune et rouge.

**FaultingObjectDescription** partie des informations pour le matériel, généralement vide pour les objets logiciels.

**FaultingObjectLocation** informations d’emplacement pour le matériel, généralement vide pour les objets de logiciels.

**RecommendedActions** liste des actions recommandées, qui sont indépendantes et dans aucun ordre particulier. Aujourd'hui, cette liste est souvent de longueur 1.

## <a name="properties-of-fault-events"></a>Propriétés d’événements d’erreur

Ce tableau présente plusieurs propriétés clés de l’événement d’erreur. Pour le schéma complet, inspecter la **MSFT\_StorageFaultEvent** classe *storagewmi.mof*.

Remarque la **ChangeType**, ce qui indique si une erreur est créée, supprimé ou mis à jour et le **FaultId**. Un événement contient également toutes les propriétés de l’erreur concerné.

| **Propriété**              | **Exemple**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Raison                    | « Le volume est en cours d’exécution en dehors de l’espace disponible. »                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Monter en rack A06, RU 25, emplacement 11                                        |
| RecommendedActions        | {« Étendre le volume. », « Migrer les charges de travail à d’autres volumes. »}   |

**ChangeType** ChangeType = { 0, 1, 2 } = { "Create", "Remove", "Update" }.

## <a name="coverage"></a>Couverture

Dans Windows Server 2016, le Service de contrôle d’intégrité fournit la couverture des erreurs suivantes :  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Gravité : Warning
* Cause : *« Le disque physique a échoué. »*
* RecommendedAction: *« Remplacer le disque physique. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Gravité : Warning
* Cause : *« Connectivité a été perdue sur le disque physique. »*
* RecommendedAction: *« Vérifiez que le disque physique est actif et connectés correctement. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Gravité : Warning
* Cause : *« Le disque physique est présentant une absence de réponse périodique ».*
* RecommendedAction: *« Remplacer le disque physique. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Gravité : Warning
* Cause : *« Une défaillance du disque physique est prédite se produise plus rapidement. »*
* RecommendedAction: *« Remplacer le disque physique. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Gravité : Warning
* Cause : *« Le disque physique est mis en quarantaine, car il n’est pas pris en charge par votre fournisseur de solutions. »*
* RecommendedAction: *« Remplacer le disque physique avec le matériel pris en charge. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Gravité : Warning
* Cause : *« Le disque physique est en quarantaine, car sa version du microprogramme n’est pas pris en charge par votre fournisseur de solutions. »*
* RecommendedAction: *« Mettre à jour le microprogramme sur le disque physique à la version cible. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Gravité : Warning
* Cause : *« Le disque physique a des métadonnées non reconnu ».*
* RecommendedAction: *« Ce disque peut contenir des données à partir d’un pool de stockage inconnu. Tout d’abord vous assurer de qu'aucune donnée utile sur ce disque, puis réinitialiser le disque. »*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType : Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Gravité : Warning
* Cause : *« Échec de la tentative pour mettre à jour du microprogramme sur le disque physique ».*
* RecommendedAction: *« Essayez d’utiliser un microprogramme différents binaire ».*

### <a name="virtual-disk-2"></a>**Disque virtuel (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType : Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Gravité : Informationnel
* Cause : *« Certaines données sur ce volume ne seront pas complètement résilientes. Il reste accessible. »*
* RecommendedAction: *« Restauration de la résilience des données ».*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType : Microsoft.Health.FaultType.VirtualDisks.Detached
* Gravité : Critique
* Cause : *« Le volume n’est pas accessible. Certaines données peuvent être perdues. »*
* RecommendedAction: *« Vérifiez physique et/ou la connectivité de tous les périphériques de stockage réseau. Vous devrez peut-être restaurer à partir de la sauvegarde. »*

### <a name="pool-capacity-1"></a>**Capacité du pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType : Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Gravité : Warning
* Cause : *« Le pool de stockage n’a pas la capacité de réserves recommandée. Cela peut limiter votre capacité à restaurer la résilience des données en cas d’échec (s) de lecteur. »*
* RecommendedAction: *« Ajouter une capacité supplémentaire au pool de stockage ou libérez de capacité. La valeur minimale recommandée réserve varie selon le déploiement, mais il est l’équivalent d’environ 2 disques de capacité. »*

### <a name="volume-capacity-2sup1sup"></a>**Capacité du volume (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType : Microsoft.Health.FaultType.Volume.Capacity
* Gravité : Warning
* Cause : *« Le volume est en cours d’exécution en dehors de l’espace disponible. »*
* RecommendedAction: *« Étendre le volume ou migrer des charges de travail à d’autres volumes ».*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType : Microsoft.Health.FaultType.Volume.Capacity
* Gravité : Critique
* Cause : *« Le volume est en cours d’exécution en dehors de l’espace disponible. »*
* RecommendedAction: *« Étendre le volume ou migrer des charges de travail à d’autres volumes ».*

### <a name="server-3"></a>**Serveur (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType : Microsoft.Health.FaultType.Server.Down
* Gravité : Critique
* Cause : *« Le serveur ne peut pas être atteint ».*
* RecommendedAction: *« Démarrer ou remplacer le serveur. »*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType : Microsoft.Health.FaultType.Server.Isolated
* Gravité : Critique
* Cause : *« Le serveur est isolé à partir du cluster en raison de problèmes de connectivité ».*
* RecommendedAction: *« Si isolation persiste, vérifiez l’ou les réseaux ou migrer des charges de travail à d’autres nœuds. »*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType : Microsoft.Health.FaultType.Server.Quarantined
* Gravité : Critique
* Cause : *« Le serveur est mis en quarantaine par le cluster en raison d’échecs périodiques. »*
* RecommendedAction: *« Remplacer le serveur ou corriger le problème réseau ».*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType : Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Gravité : Critique
* Cause : *« Le cluster est une défaillance du serveur en dehors de la dégradation. »*
* RecommendedAction: *« Vérifiez la ressource témoin et redémarrez si nécessaire. Démarrer ou remplacer des serveurs ayant échoués. »*

### <a name="network-adapterinterface-4"></a>**Interface/de cartes réseau (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType : Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Gravité : Warning
* Cause : *« L’interface réseau est déconnectée. »*
* RecommendedAction: *« Reconnectez le câble réseau. »*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType : Microsoft.Health.FaultType.NetworkInterface.Missing
* Gravité : Warning
* Cause : *« Le serveur {serveur} possède manquant réseau ou les cartes réseau connectées au réseau de cluster {réseau de cluster} ».*
* RecommendedAction: *« Se connecter le serveur au réseau de cluster manquantes. »*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType : Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Gravité : Warning
* Cause : *« L’interface réseau a une défaillance matérielle ».*
* RecommendedAction: *« Remplacer la carte d’interface réseau. »*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType : Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Gravité : Warning
* Cause : *« L’interface réseau {interface de réseau} n’est pas activé et n’est pas utilisé. »*
* RecommendedAction: *« Activer l’interface réseau ».*

### <a name="enclosure-6"></a>**Boîtier (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType : Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Gravité : Warning
* Cause : *« Communication a été perdue dans le boîtier de stockage. »*
* RecommendedAction: *« Démarrer ou remplacez le boîtier de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType : Microsoft.Health.FaultType.StorageEnclosure.FanError
* Gravité : Warning
* Cause : *« Le ventilateur à la position {position} du boîtier de stockage a échoué. »*
* RecommendedAction: *« Remplacer le ventilateur dans le boîtier de stockage. »*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType : Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Gravité : Warning
* Cause : *« Le capteur de courant à la position {position} du boîtier de stockage a échoué. »*
* RecommendedAction: *« Remplacer un capteur de courant dans le boîtier de stockage. »*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType : Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Gravité : Warning
* Cause : *« Le capteur de voltage à la position {position} du boîtier de stockage a échoué. »*
* RecommendedAction: *« Remplacer un capteur de tension dans le boîtier de stockage. »*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType : Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Gravité : Warning
* Cause : *« Le contrôleur d’e/s à la position {position} du boîtier de stockage a échoué. »*
* RecommendedAction: *« Remplacer un contrôleur d’e/s dans le boîtier de stockage. »*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType : Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Gravité : Warning
* Cause : *« Le capteur de température à la position {position} du boîtier de stockage a échoué. »*
* RecommendedAction: *« Remplacer un capteur de température dans le boîtier de stockage. »*

### <a name="firmware-rollout-3"></a>**Déploiement du microprogramme (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType : Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Gravité : Warning
* Cause : *« Impossible actuellement de progresser lors de l’exécution du microprogramme de lancement ».*
* RecommendedAction: *« Vérifier tous les espaces de stockage sont intègres, et qu’aucun domaine d’erreur n’est actuellement en mode maintenance ».*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType : Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Gravité : Warning
* Cause : *« De lancement du microprogramme a été annulée en raison d’informations sur la version du microprogramme illisible ou inattendue après avoir appliqué une mise à jour du microprogramme. »*
* RecommendedAction: *« Redémarrage microprogramme déployons une fois que le problème du microprogramme a été résolu. »*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType : Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Gravité : Warning
* Cause : *« Déploiement de microprogramme a été annulée en raison du trop grand nombre de disques physiques Échec d’une tentative de mise à jour du microprogramme. »*
* RecommendedAction: *« Redémarrage microprogramme déployons une fois que le problème du microprogramme a été résolu. »*

### <a name="storage-qos-3sup2sup"></a>**QoS de stockage (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType : Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Gravité : Warning
* Cause : *« Débit de stockage est insuffisant pour satisfaire les réserves de ressources ».*
* RecommendedAction: *« Reconfigurez les stratégies de QoS de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType : Microsoft.Health.FaultType.StorQos.LostCommunication
* Gravité : Warning
* Cause : *« Le Gestionnaire de stratégie QoS de stockage a perdu la communication avec le volume. »*
* RecommendedAction: *« Veuillez redémarrer les nœuds {nœuds} »*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType : Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Gravité : Warning
* Cause : *« Un ou plusieurs consommateurs de stockage (généralement des Machines virtuelles) sont à l’aide une stratégie inexistante avec l’id {id}. »*
* RecommendedAction: *« Recréer toutes les stratégies QoS de stockage manquants. »*

<sup>1</sup> indique le volume a atteint un remplissage à 80 % (gravité mineure) ou à 90 % (gravité majeure).  
<sup>2</sup> indique que certains fichiers .vhd sur le volume n’ont pas été réunies pour leur nombre minimal d’IOPS sur 10 % (mineur), 30 % (majeur) ou 50 % (critique) de la fenêtre de 24 heures.  

>[!NOTE]
> L’intégrité des composants du boîtier de stockage tels que les ventilateurs, blocs d’alimentations et capteurs est dérivée des services de boîtier SCSI. Si votre fournisseur ne fournit pas ces informations, le service de contrôle d’intégrité ne peut pas les afficher.  

## <a name="see-also"></a>Voir aussi

- [Service d’intégrité de Windows Server 2016](health-service-overview.md)
