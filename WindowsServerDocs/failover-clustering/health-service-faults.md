---
title: Erreurs de Service de contrôle d’intégrité
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 913a596a46720718a165295345cb02e3e2baa1de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827562"
---
# <a name="health-service-faults"></a>Erreurs de Service de contrôle d’intégrité
> S’applique à : Windows Server 2019, Windows Server 2016

## <a name="what-are-faults"></a>Que sont les erreurs ?

Le Service de contrôle d’intégrité surveille en permanence votre cluster espaces de stockage direct pour détecter les problèmes et générer des « erreurs ». Une nouvelle applet de commande affiche les erreurs en cours, ce qui vous permet de vérifier facilement l’intégrité de votre déploiement sans avoir à examiner chaque entité ou fonctionnalité. Les erreurs sont conçues pour être précises, faciles à comprendre et exploitables.  

Chaque erreur contient cinq champs importants :  

-   Severity
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

Le Service de contrôle d’intégrité peut évaluer la causalité potentielle entre les entités défaillantes pour identifier et combiner les erreurs qui sont des conséquences du même problème sous-jacent. La reconnaissance d’un effet tel en chaîne permet d’écourter les rapports. Par exemple, si un serveur est hors service, il est prévu que tous les lecteurs au sein du serveur ne soient pas connectés. Par conséquent, une seule erreur sera déclenchée pour la cause racine, dans ce cas, le serveur.  

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Pour afficher les erreurs en cours dans PowerShell, exécutez cette applet de commande :

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Cela renvoie toutes les erreurs qui affectent l’ensemble du cluster espaces de stockage direct. La plupart du temps, ces erreurs sont liées au matériel ou à la configuration. En l’absence d’erreurs, cette applet de commande ne retourne rien.  

>[!NOTE]
> Dans un environnement hors production et à vos propres risques, vous pouvez expérimenter cette fonctionnalité en déclenchant des erreurs vous-même, par exemple en supprimant un disque physique ou en arrêtant un nœud. Une fois que l’erreur s’est produite, réinsérez le disque physique ou redémarrez le nœud pour que l’erreur disparaisse à nouveau.

Vous pouvez également afficher les erreurs qui affectent uniquement des volumes ou des partages de fichiers spécifiques avec les applets de commande suivantes :  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Cela renvoie toutes les erreurs qui affectent uniquement le volume ou le partage de fichiers spécifique. La plupart du temps, ces erreurs sont liées à la planification de la capacité, à la résilience des données ou à des fonctionnalités telles que la qualité de service de stockage ou le réplica de stockage. 

## <a name="usage-in-net-and-c"></a>Utilisation dans .NET etC#

### <a name="connect"></a>Se connecter

Pour pouvoir interroger le Service de contrôle d’intégrité, vous devez établir un **CimSession** avec le cluster. Pour ce faire, vous aurez besoin de certains éléments qui ne sont disponibles que dans le .NET complet, ce qui signifie que vous ne pouvez pas effectuer cette opération directement à partir d’une application Web ou mobile. Ces exemples de code utilisent C\#, le choix le plus simple pour cette couche d’accès aux données.

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

### <a name="discover-objects"></a>Détecter les objets

Une fois le **CimSession** établi, vous pouvez interroger Windows Management Instrumentation (WMI) sur le cluster.

Avant de pouvoir récupérer des erreurs ou des métriques, vous devez récupérer les instances de plusieurs objets pertinents. Tout d’abord, **MSFT\_StorageSubSystem** qui représente espaces de stockage direct sur le cluster. À l’aide de cela, vous pouvez récupérer chaque **\_msft StorageNode** dans le cluster, et chaque **msft\_volume**, les volumes de données. Enfin, vous aurez besoin du **\_msft**, le service de contrôle d’intégrité lui-même.

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

### <a name="query-faults"></a>Erreurs de requête

Appelez le **diagnostic** pour récupérer les erreurs en cours dans l’étendue de la **CimInstance**cible, qui correspond au cluster ou à n’importe quel volume.

La liste complète des erreurs disponibles à chaque étendue dans Windows Server 2016 est décrite ci-dessous.

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

### <a name="optional-myfault-class"></a>Facultatif : classe MyFault

Il peut être judicieux de construire et de conserver votre propre représentation des erreurs. Par exemple, cette classe **MyFault** stocke plusieurs propriétés clés des erreurs, y compris le **FaultId**, qui peut être utilisé ultérieurement pour associer des notifications de mise à jour ou de suppression, ou pour dédupliquer dans le cas où la même erreur est détectée plusieurs fois, pour une raison quelconque.

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

La liste complète des propriétés de chaque erreur (**DiagnoseResult**) est décrite ci-dessous.

### <a name="fault-events"></a>Événements d’erreur

Lorsque des erreurs sont créées, supprimées ou mises à jour, le Service de contrôle d’intégrité génère des événements WMI. Celles-ci sont essentielles pour assurer la synchronisation de l’état de votre application sans interrogation fréquente. elles peuvent vous aider à déterminer quand envoyer des alertes par courrier électronique, par exemple. Pour s’abonner à ces événements, cet exemple de code utilise à nouveau le modèle de conception observateur.

Tout d’abord, abonnez-vous à **MSFT\_événements StorageFaultEvent** .

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

Implémentez ensuite un observateur dont la méthode **OnNext ()** est appelée chaque fois qu’un nouvel événement est généré.

Chaque événement contient un **ChangeType** indiquant si une erreur est créée, supprimée ou mise à jour, ainsi que les **FaultId**pertinents.

En outre, ils contiennent toutes les propriétés de l’erreur proprement dite.

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

### <a name="understand-fault-lifecycle"></a>Comprendre le cycle de vie des erreurs

Les erreurs ne sont pas censées être marquées comme étant « consultées » ou résolues par l’utilisateur. Elles sont créées lorsque le Service de contrôle d’intégrité observe un problème et elles sont automatiquement supprimées et uniquement lorsque le Service de contrôle d’intégrité ne peut plus observer le problème. En général, cela indique que le problème a été résolu.

Toutefois, dans certains cas, les erreurs peuvent être redécouvertes par le Service de contrôle d’intégrité (par exemple après un basculement ou en raison d’une connectivité intermittente, etc.). Pour cette raison, il peut être judicieux de conserver votre propre représentation des erreurs, afin de pouvoir les dédupliquer facilement. Cela est particulièrement important si vous envoyez des alertes par courrier électronique ou des alertes équivalentes.

### <a name="properties-of-faults"></a>Propriétés des erreurs

Ce tableau présente plusieurs propriétés clés de l’objet d’erreur. Pour le schéma complet, inspectez la classe **MSFT\_StorageDiagnoseResult** dans *storagewmi. mof*.

| **Propriété**              | **Exemple**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft. Health. FaultType. volume. Capacity                      |
| Raison                    | « Espace disponible insuffisant sur le volume. »                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, emplacement 11                                        |
| RecommendedActions        | {« Développer le volume. », « migrer les charges de travail vers d’autres volumes ».}   |

**FaultId** Unique au sein de l’étendue d’un cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {« informatif », « avertissement » et « erreur »}, ou des couleurs équivalentes telles que le bleu, le jaune et le rouge.

**FaultingObjectDescription** Informations de référence sur le matériel, généralement vides pour les objets logiciels.

**FaultingObjectLocation** Informations d’emplacement pour le matériel, généralement vides pour les objets logiciels.

**RecommendedActions** Liste des actions recommandées, qui sont indépendantes et dans aucun ordre particulier. Aujourd’hui, cette liste est souvent de longueur 1.

## <a name="properties-of-fault-events"></a>Propriétés des événements d’erreur

Ce tableau présente plusieurs propriétés clés de l’événement d’erreur. Pour le schéma complet, inspectez la classe **MSFT\_StorageFaultEvent** dans *storagewmi. mof*.

Notez le **ChangeType**, qui indique si une erreur est créée, supprimée ou mise à jour, et le **FaultId**. Un événement contient également toutes les propriétés de l’erreur affectée.

| **Propriété**              | **Exemple**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft. Health. FaultType. volume. Capacity                      |
| Raison                    | « Espace disponible insuffisant sur le volume. »                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, emplacement 11                                        |
| RecommendedActions        | {« Développer le volume. », « migrer les charges de travail vers d’autres volumes ».}   |

**ChangeType** ChangeType = {0, 1, 2} = {"créer", "supprimer", "mettre à jour"}.

## <a name="coverage"></a>Couverture

Dans Windows Server 2016, le Service de contrôle d’intégrité fournit la couverture d’erreur suivante :  

### <a name="physicaldisk-8"></a>**Disque physique (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. FailedMedia
* Gravité : avertissement
* Raison : *« le disque physique a échoué. »*
* RecommendedAction : *« remplacer le disque physique ».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. LostCommunication
* Gravité : avertissement
* Raison : *« la connectivité a été perdue sur le disque physique ».*
* RecommendedAction : *"Vérifiez que le disque physique fonctionne et est correctement connecté."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. ne répond pas
* Gravité : avertissement
* Raison : *« le disque physique présente une inactivité récurrente ».*
* RecommendedAction : *« remplacer le disque physique ».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. PredictiveFailure
* Gravité : avertissement
* Raison : *« une défaillance du disque physique est prévue pour s’exécuter bientôt ».*
* RecommendedAction : *« remplacer le disque physique ».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. UnsupportedHardware
* Gravité : avertissement
* Raison : *« le disque physique est mis en quarantaine parce qu’il n’est pas pris en charge par le fournisseur de votre solution. »*
* RecommendedAction : *"remplacer le disque physique par un matériel pris en charge."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. UnsupportedFirmware
* Gravité : avertissement
* Raison : *« le disque physique est en quarantaine, car sa version du microprogramme n’est pas prise en charge par le fournisseur de votre solution. »*
* RecommendedAction : *« mettre à jour le microprogramme sur le disque physique avec la version cible ».*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. UnrecognizedMetadata
* Gravité : avertissement
* Raison : *« le disque physique contient des métadonnées non reconnues ».*
* RecommendedAction : *«ce disque peut contenir des données d’un pool de stockage inconnu. Tout d’abord, assurez-vous qu’il n’existe aucune donnée utile sur ce disque, puis réinitialisez le disque.»*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType : Microsoft. Health. FaultType. PhysicalDisk. FailedFirmwareUpdate
* Gravité : avertissement
* Raison : *« échec de la tentative de mise à jour du microprogramme sur le disque physique ».*
* RecommendedAction : *« essayez d’utiliser un autre binaire de microprogramme ».*

### <a name="virtual-disk-2"></a>**Disque virtuel (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType : Microsoft. Health. FaultType. Virtualdisks et. NeedsRepair
* Gravité : informatif
* Raison : *«certaines données sur ce volume ne sont pas entièrement résilientes. Il reste accessible.»*
* RecommendedAction : *« restauration de la résilience des données ».*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType : Microsoft. Health. FaultType. Virtualdisks et. détaché
* Gravité : critique
* Raison : *«le volume est inaccessible. Certaines données peuvent être perdues.»*
* RecommendedAction : *Vérifiez la connectivité physique et/ou réseau de tous les périphériques de stockage. Vous devrez peut-être effectuer une restauration à partir d’une sauvegarde.»*

### <a name="pool-capacity-1"></a>**Capacité du pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType : Microsoft. Health. FaultType. StoragePool. InsufficientReserveCapacityFault
* Gravité : avertissement
* Raison : *«le pool de stockage n’a pas la capacité de réserve minimale recommandée. Cela peut limiter votre capacité à restaurer la résilience des données en cas de défaillances du disque.*
* RecommendedAction : *"Ajoutez de la capacité supplémentaire au pool de stockage ou libérez de la capacité. La réserve minimale recommandée varie en fonction du déploiement, mais elle est d’environ 2 unités de capacité.*

### <a name="volume-capacity-2sup1sup"></a>**Capacité du volume (2)** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType : Microsoft. Health. FaultType. volume. Capacity
* Gravité : avertissement
* Raison : *« espace disponible insuffisant pour le volume. »*
* RecommendedAction : *développez le volume ou migrez les charges de travail vers d’autres volumes.*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType : Microsoft. Health. FaultType. volume. Capacity
* Gravité : critique
* Raison : *« espace disponible insuffisant pour le volume. »*
* RecommendedAction : *développez le volume ou migrez les charges de travail vers d’autres volumes.*

### <a name="server-3"></a>**Serveur (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType : Microsoft. Health. FaultType. Server. UpDown
* Gravité : critique
* Raison : *« Impossible d’accéder au serveur ».*
* RecommendedAction : *« Démarrer ou remplacer le serveur ».*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType : Microsoft. Health. FaultType. Server. isolated
* Gravité : critique
* Raison : *« le serveur est isolé du cluster en raison de problèmes de connectivité ».*
* RecommendedAction : *« si l’isolation persiste, vérifiez le ou les réseaux ou migrez les charges de travail vers d’autres nœuds ».*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType : Microsoft. Health. FaultType. Server. quarantined
* Gravité : critique
* Raison : *« le serveur est mis en quarantaine par le cluster en raison d’échecs récurrents ».*
* RecommendedAction : *« remplacer le serveur ou corriger le réseau ».*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType : Microsoft. Health. FaultType. ClusterQuorumWitness. erreur
* Gravité : critique
* Raison : *« le cluster n’est plus en panne. »*
* RecommendedAction : *«Vérifiez la ressource témoin, puis redémarrez si nécessaire. Démarrez ou remplacez les serveurs défaillants.»*

### <a name="network-adapterinterface-4"></a>**Carte réseau/interface (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType : Microsoft. Health. FaultType. NetworkAdapter. disconnected
* Gravité : avertissement
* Raison : *« l’interface réseau a été déconnectée ».*
* RecommendedAction : *« reconnectez le câble réseau. »*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType : Microsoft. Health. FaultType. l’interface réseau. Missing
* Gravité : avertissement
* Raison : *« le serveur {Server} n’a pas de carte réseau connectée au réseau de cluster {cluster Network} ».*
* RecommendedAction : *« Connectez le serveur au réseau du cluster manquant ».*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType : Microsoft. Health. FaultType. NetworkAdapter. Hardware
* Gravité : avertissement
* Raison : *« l’interface réseau a rencontré une défaillance matérielle ».*
* RecommendedAction : *« remplacer la carte d’interface réseau ».*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType : Microsoft. Health. FaultType. NetworkAdapter. Disabled
* Gravité : avertissement
* Raison : *« l’interface réseau {Network Interface} n’est pas activée et n’est pas utilisée. »*
* RecommendedAction : *« activer l’interface réseau ».*

### <a name="enclosure-6"></a>**Boîtier (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType : Microsoft. Health. FaultType. StorageEnclosure. LostCommunication
* Gravité : avertissement
* Raison : *« la communication a été perdue dans le boîtier de stockage. »*
* RecommendedAction : *« Démarrer ou remplacer le boîtier de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType : Microsoft. Health. FaultType. StorageEnclosure. FanError
* Gravité : avertissement
* Raison : *« le ventilateur à la position {position} du boîtier de stockage a échoué ».*
* RecommendedAction : *« remplacer le ventilateur dans le boîtier de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType : Microsoft. Health. FaultType. StorageEnclosure. CurrentSensorError
* Gravité : avertissement
* Raison : *« le capteur actuel à la position {position} du boîtier de stockage a échoué ».*
* RecommendedAction : *« remplacer un capteur actuel dans le boîtier de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType : Microsoft. Health. FaultType. StorageEnclosure. VoltageSensorError
* Gravité : avertissement
* Raison : *« le capteur de voltage à la position {position} du boîtier de stockage a échoué ».*
* RecommendedAction : *« remplacer un capteur de voltage dans le boîtier de stockage. »*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType : Microsoft. Health. FaultType. StorageEnclosure. IoControllerError
* Gravité : avertissement
* Raison : *« le contrôleur d’e/s à la position {position} du boîtier de stockage a échoué ».*
* RecommendedAction : *« remplacer un contrôleur d’e/s dans le boîtier de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType : Microsoft. Health. FaultType. StorageEnclosure. TemperatureSensorError
* Gravité : avertissement
* Raison : *« le capteur de température à la position {position} du boîtier de stockage a échoué ».*
* RecommendedAction : *« remplacer un capteur de température dans le boîtier de stockage ».*

### <a name="firmware-rollout-3"></a>**Déploiement du microprogramme (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType : Microsoft. Health. FaultType. FaultDomain. FailedMaintenanceMode
* Gravité : avertissement
* Raison : *« Impossible d’effectuer l’opération en cours lors du déploiement du microprogramme ».*
* RecommendedAction : *« vérifier que tous les espaces de stockage sont intègres et qu’aucun domaine d’erreur n’est actuellement en mode de maintenance ».*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType : Microsoft. Health. FaultType. FaultDomain. FirmwareVerifyVersionFaile
* Gravité : avertissement
* Raison : *« le déploiement du microprogramme a été annulé en raison d’informations de version du microprogramme illisibles ou inaccessibles après l’application d’une mise à jour du microprogramme ».*
* RecommendedAction : *« Redémarrez le déploiement du microprogramme une fois que le problème du microprogramme a été résolu ».*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType : Microsoft. Health. FaultType. FaultDomain. TooManyFailedUpdates
* Gravité : avertissement
* Raison : *« le déploiement du microprogramme a été annulé en raison d’un trop grand nombre de disques physiques qui n’ont pas pu être mis à jour. »*
* RecommendedAction : *« Redémarrez le déploiement du microprogramme une fois que le problème du microprogramme a été résolu ».*

### <a name="storage-qos-3sup2sup"></a>**Qualité de service de stockage (3)** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType : Microsoft. Health. FaultType. StorQos. InsufficientThroughput
* Gravité : avertissement
* Raison : *« le débit de stockage est insuffisant pour satisfaire les réserves ».*
* RecommendedAction : *« Reconfigurez les stratégies de qualité de service de stockage ».*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType : Microsoft. Health. FaultType. StorQos. LostCommunication
* Gravité : avertissement
* Raison : *« le gestionnaire de stratégie QoS de stockage a perdu la communication avec le volume ».*
* RecommendedAction : *"Veuillez redémarrer les nœuds {Nodes}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType : Microsoft. Health. FaultType. StorQos. MisconfiguredFlow
* Gravité : avertissement
* Raison : *« un ou plusieurs consommateurs de stockage (généralement des machines virtuelles) utilisent une stratégie inexistante avec l’ID {id} ».*
* RecommendedAction : *"recréer toutes les stratégies QoS de stockage manquantes."*

<sup>1</sup> indique que le volume a atteint 80% de l’intégralité (gravité mineure) ou 90% complet (gravité majeure).  
<sup>2</sup> indique qu’un ou plusieurs disques durs virtuels sur le volume n’ont pas atteint leur nombre d’e/s par seconde minimum pendant plus de 10% (mineur), 30% (majeur) ou 50% (critique) de la fenêtre roulante de 24 heures.  

>[!NOTE]
> L’intégrité des composants du boîtier de stockage tels que les ventilateurs, blocs d’alimentations et capteurs est dérivée des services de boîtier SCSI. Si votre fournisseur ne fournit pas ces informations, le service de contrôle d’intégrité ne peut pas les afficher.  

## <a name="see-also"></a>Voir aussi

- [Service de contrôle d’intégrité dans Windows Server 2016](health-service-overview.md)
