---
title: Problèmes connus liés au réplica de stockage
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 06/25/2019
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 681e07b85af603d11295bf1ca2a08f0eb7181725
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865251"
---
# <a name="known-issues-with-storage-replica"></a>Problèmes connus liés au réplica de stockage

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Cette rubrique décrit les problèmes connus liés au réplica de stockage dans Windows Server.

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>Après la suppression de la réplication, les disques sont hors connexion et vous ne pouvez pas configurer à nouveau la réplication

Dans Windows Server2016, il est possible que vous ne puissiez pas configurer la réplication sur un volume qui a été précédemment répliqué ou que vous trouviez des volumes non montables. Cela peut se produire quand une condition d’erreur empêche la suppression de la réplication ou que vous réinstallez le système d’exploitation sur un ordinateur qui répliquait auparavant des données.  

Pour résoudre le problème, vous devez effacer la partition de réplica de stockage cachée des disques et rétablir un état accessible en écriture à l’aide de l’applet de commande `Clear-SRMetadata`.  

-   Pour supprimer tous les emplacements de bases de données de partition de réplica de stockage orphelins et remonter toutes les partitions, utilisez le paramètre `-AllPartitions` comme suit:  

    ```PowerShell
    Clear-SRMetadata -AllPartitions  
    ```  

-   Pour supprimer toutes les données du journal de réplica de stockage orphelines, utilisez le paramètre `-AllLogs` comme suit :  

    ```PowerShell
    Clear-SRMetadata -AllLogs  
    ```  

-   Pour supprimer toutes les données de configuration de cluster de basculement orphelines, utilisez le paramètre `-AllConfiguration` comme suit :  

    ```PowerShell
    Clear-SRMetadata -AllConfiguration  
    ```  

-   Pour supprimer des métadonnées de groupe de réplication individuelles, utilisez le paramètre `-Name` et spécifiez un groupe de réplication comme suit :  

    ```PowerShell
    Clear-SRMetadata -Name RG01 -Logs -Partition  
    ```  

Vous devrez peut-être redémarrer le serveur après le nettoyage de la base de données de partition ; vous pouvez éviter de le faire temporairement avec `-NoRestart`, mais vous ne devez pas ignorer le redémarrage du serveur s’il est demandé par l’applet de commande. Cette applet de commande ne supprime pas les volumes de données ni les données contenues dans ces volumes.  

## <a name="during-initial-sync-see-event-log-4004-warnings"></a>Pendant la synchronisation initiale, examinez les avertissements4004 du journal des événements

Dans Windows Server 2016, lors de la configuration de la réplication, les serveurs source et de destination peuvent présenter chacun plusieurs avertissements 4004 du journal des événements **StorageReplica\Admin**, avec l’état « Ressources système insuffisantes pour mener à bien l’API », durant la synchronisation initiale. Vous êtes susceptible de rencontrer aussi des erreurs 5014. Ces erreurs indiquent que les serveurs ne disposent pas de suffisamment de mémoire (RAM) pour exécuter à la fois la synchronisation initiale et les charges de travail. Ajoutez de la RAM ou réduisez la quantité de RAM utilisée par les fonctionnalités et applications autres que le réplica de stockage.  

## <a name="when-using-guest-clusters-with-shared-vhdx-and-a-host-without-a-csv-virtual-machines-stop-responding-after-configuring-replication"></a>Quand vous utilisez des clusters invités avec VHDX partagé et un hôte sans un volume CSV, les machines virtuelles cessent de répondre après la configuration de la réplication

Dans Windows Server2016, lors de l’utilisation de clusters invités Hyper-V à des fins de test ou de démonstration du réplica de stockage, et de l’utilisation de VHDX partagé en tant que stockage de cluster invité, les machines virtuelles cessent de répondre après la configuration de la réplication. Si vous redémarrez l’hôte Hyper-V, les machines virtuelles répondent, mais la configuration de la réplication n’est pas terminée et aucune réplication ne se produit.  

Ce comportement se produit quand vous utilisez **fltmc.exe attach svhdxflt** pour contourner la nécessité de l’hôte Hyper-V exécutant un volume CSV. L’utilisation de cette commande n’est pas prise en charge et est destinée uniquement à des fins de test et de démonstration.  

La cause du ralentissement est un problème d’interopérabilité lié à la conception entre la fonctionnalité de qualité de service de stockage dans Windows Server2016 et le filtre VHDX partagé relié manuellement. Pour résoudre ce problème, désactivez le pilote de filtre de qualité de service de stockage et redémarrez l’hôte Hyper-V:  

```  
SC config storqosflt start= disabled  
```  

## <a name="cannot-configure-replication-when-using-new-volume-and-differing-storage"></a>Impossible de configurer la réplication lors de l’utilisation de New-Volume et de stockage différent

Quand vous utilisez l’applet de commande `New-Volume` avec différents ensembles de stockage sur les serveurs source et de destination, tels que deux SAN différents ou deux boîtiers JBOD avec des disques différents, il est possible que vous ne puissiez pas par la suite configurer la réplication avec `New-SRPartnership`. L’erreur affichée peut indiquer:  

    Data partition sizes are different in those two groups  

Utilisez l’applet de commande `New-Partition**` pour créer des volumes et les mettre en forme au lieu de `New-Volume`, comme cette dernière peut arrondir la taille du volume sur différents groupes de stockage. Si vous avez déjà créé un volume NTFS, vous pouvez utiliser `Resize-Partition` pour agrandir ou réduire l’un des volumes pour qu’il corresponde à l’autre (cette opération ne peut pas être effectuée avec des volumes ReFS). Si vous utilisez **Diskmgmt** ou le **Gestionnaire de serveur**, aucun arrondi n’est effectué.  

## <a name="running-test-srtopology-fails-with-name-related-errors"></a>Échec de l’exécution de Test-SRTopology avec des erreurs liées aux noms

Quand vous tentez d’utiliser `Test-SRTopology`, vous recevez l’une des erreurs suivantes :  

**EXEMPLE D’ERREUR 1 :**

    WARNING: Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as  
    input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs  
    WARNING: System.Exception  
    WARNING:    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()  
    Test-SRTopology : Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP  
    address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable  
    inputs  
    At line:1 char:1  
    + Test-SRTopology -SourceComputerName sr-srv01 -SourceVolumeName d: -So ...  
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  
        + CategoryInfo          : InvalidArgument: (:) [Test-SRTopology], Exception  
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand  

**EXEMPLE D’ERREUR 2 :**

    WARNING: Invalid value entered for source computer name

**EXEMPLE D’ERREUR 3 :**

    The specified volume cannot be found G: cannot be found on computer SRCLUSTERNODE1

Cette applet de commande offre un signalement des erreurs limité dans Windows Server2016 et retourne le même résultat pour de nombreux problèmes courants. L’erreur peut apparaître pour les raisons suivantes :  

* Vous êtes connecté à l’ordinateur source comme utilisateur local, et non utilisateur de domaine.  
* L’ordinateur de destination n’est pas en cours d’exécution ou n’est pas accessible sur le réseau.  
* Vous avez spécifié un nom incorrect pour l’ordinateur de destination.  
* Vous avez spécifié une adresse IP pour le serveur de destination.  
* Le pare-feu de l’ordinateur de destination bloque l’accès aux appels PowerShell et/ou CIM.  
* L’ordinateur de destination n’exécute pas le service WMI.   
* Vous n’avez pas utilisé CREDSSP lors de l’exécution de l’applet de commande `Test-SRTopology` à distance à partir d’un ordinateur de gestion.
* Les volumes sources ou de destination spécifiés sont des disques locaux sur un nœud de cluster et non des disques en cluster.  

## <a name="configuring-new-storage-replica-partnership-returns-an-error---failed-to-provision-partition"></a>La configuration du nouveau partenariat de réplica de stockage retourne une erreur, « Impossible de mettre en service la partition »

Quand vous tentez de créer un partenariat de réplication avec `New-SRPartnership`, vous recevez l’erreur suivante:

    New-SRPartnership : Unable to create replication group test01, detailed reason: Failed to provision partition ed0dc93f-107c-4ab4-a785-afd687d3e734.
    At line: 1 char: 1
    + New-SRPartnership -SourceComputerName srv1 -SourceRGName test01
    + Categorylnfo : ObjectNotFound: (MSFT_WvrAdminTasks : root/ Microsoft/. . s) CNew-SRPartnership], CimException
    + FullyQua1ifiedErrorId : Windows System Error 1168 ,New-SRPartnership

Cela est dû à la sélection d’un volume de données qui se trouve sur la même partition que le lecteur système (autrement dit, le lecteur **C:** avec son dossier Windows), par exemple, sur un lecteur qui contient à la fois les volumes **C:** et **D:** créés à partir de la même partition. Cette opération n’est pas prise en charge dans le réplica de stockage; vous devez choisir un volume différent à répliquer.

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>Échec des tentatives d’augmentation de la taille d’un volume répliqué en raison d'une mise à jour manquante

Quand vous essayez d’augmenter la taille ou d’étendre un volume répliqué, vous recevez l’erreur suivante:

    PS C:\> Resize-Partition -DriveLetter d -Size 44GB
    Resize-Partition : The operation failed with return code 8
    At line:1 char:1
    + Resize-Partition -DriveLetter d -Size 44GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
    [Resize-Partition], CimException
    + FullyQualifiedErrorId : StorageWMI 8,Resize-Partition

Si vous utilisez le composant logiciel enfichable MMC Gestion des disques, vous recevez cette erreur:

    Element not found

Cela se produit même si vous activez correctement le redimensionnement du volume sur le serveur source à l’aide de `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE`. 

Ce problème a été résolu dans la mise à jour cumulative pour Windows 10, version 1607 (mise à jour anniversaire) et Windows Server 2016 : Le 9 décembre 2016 (KB3201845). 

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>Échec des tentatives d’augmentation de la taille d’un volume répliqué en raison d'une étape manquante

Si vous tentez de redimensionner un volume répliqué sur le serveur source sans avoir au préalable défini `-AllowResizeVolume $TRUE`, vous recevez les erreurs suivantes :

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed

    Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
    At line:1 char:1
    + Resize-Partition -DriveLetter I -Size 8GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
        + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

    Storage Replica Event log error 10307:

    Attempted to resize a partition that is protected by Storage Replica .

    DeviceName: \Device\Harddisk1\DR1
    PartitionNumber: 7
    PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

    Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.

    Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true

    Before you grow the source data partition, ensure that the destination data partition has enough space to grow to an equal size. Shrinking of data partition protected by Storage Replica is blocked.

Erreur du composant logiciel enfichable Gestion des disques : 

    An unexpected error has occurred 

Après le redimensionnement du volume, pensez à désactiver le redimensionnement avec `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE`. Ce paramètre empêche les administrateurs de tenter de redimensionner des volumes avant de s'être assurés de l'existence d'un espace disponible suffisant sur le volume de destination (généralement par ignorance de la présence du réplica de stockage). 

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>Échec de la tentative de déplacement d’une ressource de disque physique entre des sites sur un cluster étendu asynchrone

Quand vous tentez de déplacer un rôle lié à une ressource de disque physique, comme un serveur de fichiers pour une utilisation générale, afin de déplacer le stockage associé dans un cluster étendu asynchrone, vous recevez une erreur.

Si vous utilisez le composant logiciel enfichable Gestionnaire du cluster de basculement :

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group

Si vous utilisez l’applet de commande PowerShell Cluster :

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

Cela se produit en raison d’un comportement lié à la conception dans Windows Server 2016. Utilisez `Set-SRPartnership` pour déplacer ces disques de ressource de disque physique dans un cluster étendu asynchrone.  

Ce comportement a été modifié dans Windows Server, version 1709 pour autoriser les basculements manuels et automatisés avec la réplication asynchrone, en fonction des commentaires des clients.

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>Une tentative d’ajout de disques à un cluster asymétrique à deux nœuds retourne un message de type « Aucun disque approprié pour les disques de cluster trouvés ».

Quand vous tentez de configurer un cluster avec deux nœuds uniquement, avant d’ajouter la réplication de cluster étendu de réplica de stockage, vous tentez d’ajouter les disques du deuxième site aux disques disponibles. Vous recevez l’erreur suivante :

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests." 

Cela ne se produit pas si vous avez au moins trois nœuds dans le cluster. Ce problème se pose en raison d’une modification du code de conception dans Windows Server2016 pour les comportements de clustering de stockage asymétrique. 

Pour ajouter le stockage, vous pouvez exécuter la commande suivante sur le nœud dans le deuxième site :

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

Cela ne fonctionne pas avec le stockage local du nœud. Vous pouvez utiliser le réplica de stockage pour répliquer un cluster étendu entre deux nœuds au total, **chacun de ces nœuds utilisant son propre ensemble de stockage partagé.** 

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>Le limiteur de bande passante SMB ne parvient pas à limiter la bande passante de réplica de stockage

Lorsque vous spécifiez une limite de bande passante pour le réplica de stockage, la limite est ignorée et toute la bande passante est utilisée. Exemple :

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

Ce problème se produit en raison d’un problème d’interopérabilité entre le réplica de stockage et SMB. Ce problème a été résolu pour la première fois dans la mise à jour cumulative de juillet 2017 de Windows Server 2016 et dans Windows Server, version 1709.

## <a name="event-1241-warning-repeated-during-initial-sync"></a>Avertissement 1241 d’événement répété pendant la synchronisation initiale

Lorsqu’un partenariat de réplication est spécifié asynchrone, l’ordinateur source enregistre à plusieurs reprises un avertissement d'événement 1241 dans le canal d’administration du réplica de stockage. Exemple :

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 3:10:41 PM
    Event ID:      1241
    Task Category: (1)
    Level:         Warning
    Keywords:      (1)
    User:          SYSTEM
    Computer:      sr-srv05.corp.contoso.com
    Description:
    The Recovery Point Objective (RPO) of the asynchronous destination is unavailable.

    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {e20b6c68-1758-4ce4-bd3b-84a5b5ef2a87}
    LocalReplicaName: f:\
    LocalPartitionId: {27484a49-0f62-4515-8588-3755a292657f}
    ReplicaSetId: {1f6446b5-d5fd-4776-b29b-f235d97d8c63}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {7f18e5ea-53ca-4b50-989c-9ac6afb3cc81}
    TargetRPO: 30

    Guidance: This is typically due to one of the following reasons: 

La destination asynchrone est actuellement déconnectée. Le RPO peut devenir indisponible une fois la connexion rétablie.

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

Ce comportement est prévu pendant la synchronisation initiale et peut être ignoré en toute sécurité. Ce comportement peut être modifié dans une version ultérieure. Si vous voyez ce comportement lors de la réplication asynchrone en cours, examinez le partenariat pour déterminer pourquoi la réplication est retardée au-delà de votre RPO configuré (30 secondes par défaut).

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>Avertissement 4004 d’événement répété après le redémarrage d’un nœud répliqué

Dans des circonstances rares et généralement impossibles à reproduire, redémarrer un serveur qui se trouve dans un partenariat conduit à l’échec de la réplication et le nœud redémarré consigne l’avertissement d'événement 4004 avec une erreur d'accès refusé.

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 11:43:25 AM
    Event ID:      4004
    Task Category: (7)
    Level:         Warning
    Keywords:      (256)
    User:          SYSTEM
    Computer:      server.contoso.com
    Description:
    Failed to establish a connection to a remote computer.

    RemoteComputerName: server
    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    ReplicaSetId: {00000000-0000-0000-0000-000000000000}
    RemoteShareName:{a386f747-bcae-40ac-9f4b-1942eb4498a0}.{00000000-0000-0000-0000-000000000000}
    Status: {Access Denied}
    A process has requested access to an object, but has not been granted those access rights.

    Guidance: Possible causes include network failures, share creation failures for the remote replication group, or firewall settings. Make sure SMB traffic is allowed and there are no connectivity issues between the local computer and the remote computer. You should expect this event when suspending replication or removing a replication partnership.

Notez le `Status: "{Access Denied}"` et le message `A process has requested access to an object, but has not been granted those access rights.` il s’agit d’un problème connu dans le réplica de stockage et a été résolu dans la mise à jour de qualité, le 12 septembre 2017, KB4038782 (version du système d’exploitation 14393,1715) https://support.microsoft.com/help/4038782/windows-10-update-kb4038782 

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>Erreur « Impossible de mettre la ressource "Disque de Cluster x" en ligne ». avec un cluster étendu

Si vous essayez de mettre en ligne un disque de cluster après un basculement réussi, alors que vous tentez de redéfinir comme principal le site source d’origine, vous recevez une erreur dans le Gestionnaire du cluster de basculement. Exemple :

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.

    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.

Si vous essayez de déplacer le disque ou le CSV manuellement, vous recevez une erreur supplémentaire. Exemple :

    Error
    The operation has failed.
    The action 'Move' did not complete.

    Error Code: 0x8007138d
    A cluster node is not available for this operation

Ce problème est dû au fait qu’un ou plusieurs disques non initialisés sont attachés à un ou plusieurs nœuds de cluster. Pour résoudre le problème, initialises l'ensemble du stockage attaché à l’aide de DiskMgmt.msc, DISKPART.EXE ou de l’applet de commande PowerShell Initialize-Disk.

Nous nous attelons à vous proposer une mise à jour qui résout définitivement ce problème. Si vous souhaitez nous aider et si vous avez un contrat de support Microsoft Premier, envoyez un message électronique à SRFEED@microsoft.com afin que nous puissions travailler avec vous sur la soumission d’une demande de rétroportage.

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>Erreur GPT lorsque vous tentez de créer un partenariat SR

Lorsque vous exécutez New-SRPartnership, vous obtenez un échec avec l’erreur :

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

Dans l’interface utilisateur graphique du Gestionnaire de cluster de basculement, aucune option ne permet de configurer la réplication pour le disque.

Lorsque vous exécutez Test-SRTopology, vous obtenez un échec avec : 

    WARNING: Object reference not set to an instance of an object.
    WARNING: System.NullReferenceException
    WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
       at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
       at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Object reference not set to an instance of an object.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

Cela est dû au niveau fonctionnel du cluster, qui est toujours défini sur Windows Server 2012 R2 (c'est-à-dire FL 8). Le réplica de stockage est supposé renvoyer une erreur spécifique, mais retourne plutôt un mappage d’erreur incorrect.

Exécutez Get-Cluster | fl * sur chaque nœud.

Si ClusterFunctionalLevel = 9, il s'agit de la version de ClusterFunctionalLevel Windows 2016 nécessaire pour implémenter le réplica de stockage sur ce nœud.
Si ClusterFunctionalLevel n’est défini sur 9, ClusterFunctionalLevel doit être mis à jour afin d’implémenter le réplica de stockage sur ce nœud.

Pour résoudre le problème, augmentez le niveau fonctionnel du cluster en exécutant l’applet de commande PowerShell : [Mettre à jour-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>Partition inconnue de petite taille répertoriée dans DISKMGMT pour chaque volume répliqué

Lorsque vous exécutez le composant logiciel enfichable Gestion des disques (DISKMGMT.MSC), vous remarquez un ou plusieurs volumes répertoriés sans étiquette ou lettre de lecteur et dont la taille est de 1 Mo. Dans certains cas, il est possible de supprimer le volume inconnu ou le message suivant peut s’afficher :

    "An Unexpected Error has Occurred"  

Ce comportement est normal. Ceci n’est pas un volume, mais une partition. Le réplica de stockage crée une partition de 512 Ko en tant qu’emplacement de base de données pour les opérations de réplication (l’outil DiskMgmt.msc hérité arrondit la taille au mégaoctet le plus proche). Il est normal et souhaitable de disposer d’une telle partition pour chaque volume répliqué. Lorsqu’elle n’est plus utilisée, vous pouvez supprimer cette partition de 512 Ko. En revanche, il n’est pas possible de supprimer celles qui sont en cours d’utilisation. La taille de la partition ne change jamais. Si vous recréez la réplication, nous vous recommandons de conserver la partition car le réplica de stockage revendiquera les partitions inutilisées.

Pour afficher les détails, utilisez l’outil DISKPART ou l’applet de commande Get-Partition. Ces partitions auront un Type GPT `558d43c5-a1ac-43c0-aac8-d1472b2923d1`.

## <a name="a-storage-replica-node-hangs-when-creating-snapshots"></a>Un nœud de réplica de stockage se bloque lors de la création d’instantanés

Lors de la création d’un instantané VSS (par le biais de Backup, VSSADMIN, etc), un nœud de réplica de stockage se bloque et vous devez forcer un redémarrage du nœud à récupérer. Il n’y a pas d’erreur, juste un blocage matériel du serveur.

Ce problème se produit lorsque vous créez une capture instantanée VSS du volume du journal. La cause sous-jacente est un aspect de la conception hérité de VSS, et non du réplica de stockage. Le comportement résultant de l’instantané du volume du journal du réplica de stockage est un mécanisme Queing d’e/s VSS qui bloque le serveur.

Pour éviter ce comportement, n’effectuez pas de capture instantanée des volumes de journaux de réplication de stockage. Il n’est pas nécessaire d’effectuer un instantané des volumes de journaux de réplica de stockage, car ces journaux ne peuvent pas être restaurés. En outre, le volume du journal ne doit jamais contenir d’autres charges de travail, donc aucun instantané n’est nécessaire en général.

## <a name="high-io-latency-increase-when-using-storage-spaces-direct-with-storage-replica"></a>Augmentation de la latence d’e/s élevée lors de l’utilisation de espaces de stockage direct avec le réplica de stockage

Lorsque vous utilisez espaces de stockage direct avec un cache NVME ou SSD, vous constatez une augmentation de la latence supérieure à celle prévue lors de la configuration de la réplication du réplica de stockage entre les clusters espaces de stockage direct. La modification de la latence est proportionnellement plus élevée que lorsque vous utilisez NVME et SSD dans une configuration des performances et des capacités, et pas de niveau de disque dur ni de niveau de capacité.

Ce problème se produit en raison de limitations architecturales dans le mécanisme de journalisation du réplica de stockage combiné avec la latence extrêmement faible de NVME par rapport aux supports plus lents. Lors de l’utilisation du cache espaces de stockage direct, toutes les e/s des journaux des réplicas de stockage, ainsi que toutes les e/s de lecture/écriture les plus récentes des applications, sont exécutées dans le cache et jamais sur les niveaux de performance ou de capacité. Cela signifie que toutes les activités de réplica de stockage se produisent sur le même support de vitesse : cette configuration est prise https://aka.ms/srfaq en charge mais n’est pas recommandée (voir pour obtenir des recommandations pour les journaux). 

Lorsque vous utilisez espaces de stockage direct avec des disques durs, vous ne pouvez pas désactiver ou éviter le cache. En guise de solution de contournement, si vous utilisez uniquement SSD et NVME, vous pouvez configurer uniquement les niveaux de performances et de capacité. Si vous utilisez cette configuration et que vous placez les journaux SR sur le niveau de performance uniquement avec les volumes de données qu’ils utilisent uniquement sur le niveau de capacité, vous évitez le problème de latence élevée décrit ci-dessus. La même opération peut être effectuée avec une combinaison de SSD plus rapide et plus lente et aucun NVME.

Cette solution de contournement n’est bien sûr pas idéale et certains clients peuvent ne pas être en mesure de les utiliser. L’équipe de réplicas de stockage travaille sur les optimisations et un mécanisme de journalisation mis à jour à l’avenir pour réduire ces goulots d’étranglement artificiels. Ce journal v 1.1 est tout d’abord disponible dans Windows Server 2019 et ses performances améliorées sont décrites dans sur le [blog stockage](https://blogs.technet.microsoft.com/filecab/2018/12/13/chelsio-rdma-and-storage-replica-perf-on-windows-server-2019-are-💯/)sur le serveur.

## <a name="error-could-not-find-file-when-running-test-srtopology-between-two-clusters"></a>Erreur « Impossible de trouver le fichier » lors de l’exécution de test-SRTopology entre deux clusters

Lors de l’exécution de test-SRTopology entre deux clusters et leurs chemins d’accès CSV, l’opération échoue avec l’erreur suivante : 

    PS C:\Windows\system32> Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName NedClusterB -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 1 -ResultPath C:\Temp

    Validating data and log volumes...
    Measuring Storage Replica recovery and initial synchronization performance...
    WARNING: Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
    WARNING: System.IO.FileNotFoundException
    WARNING:    at System.IO.__Error.WinIOError(Int32 errorCode, String maybeFullPath)
    at System.IO.FileStream.Init(String path, FileMode mode, FileAccess access, Int32 rights, Boolean useRights, FileShare share, Int32 buff
    erSize, FileOptions options, SECURITY_ATTRIBUTES secAttrs, String msgPath, Boolean bFromProxy, Boolean useLongPath, Boolean checkHost)
    at System.IO.FileStream..ctor(String path, FileMode mode, FileAccess access, FileShare share, Int32 bufferSize, FileOptions options)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.GenerateWriteIOWorkload(String Path, UInt32 IoSizeInBytes, UInt32 Parallel
    IoCount, UInt32 Duration)
    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.<>c__DisplayClass75_0.<PerformRecoveryTest>b__0()
    at System.Threading.Tasks.Task.Execute()
    Test-SRTopology : Could not find file '\\NedNode1\C$\CLUSTERSTORAGE\VOLUME1TestSRTopologyRecoveryTest\SRRecoveryTestFile01.txt'.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName NedClusterA -SourceVolumeName  ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], FileNotFoundException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

Cela est dû à un défaut de code connu dans Windows Server 2016. Ce problème a été résolu pour la première fois dans Windows Server, version 1709 et les outils RSAT associés. Pour une résolution de niveau inférieur, contactez Support Microsoft et demandez une mise à jour rétroporter. Il n’existe aucune solution de contournement.

## <a name="error-specified-volume-could-not-be-found-when-running-test-srtopology-between-two-clusters"></a>Erreur « le volume spécifié est introuvable » lors de l’exécution de test-SRTopology entre deux clusters

Lors de l’exécution de test-SRTopology entre deux clusters et leurs chemins d’accès CSV, l’opération échoue avec l’erreur suivante :

    PS C:\> Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName RRN44-14-13 -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 30 -ResultPath c:\report

    Test-SRTopology : The specified volume C:\ClusterStorage\Volume1 cannot be found on computer RRN44-14-09. If this is a cluster node, the volume must be part of a role or CSV; volumes in Available Storage are not accessible
    At line:1 char:1
    + Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], Exception
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

Lorsque vous spécifiez le fichier CSV du nœud source comme volume source, vous devez sélectionner le nœud qui possède le volume partagé de cluster. Vous pouvez soit déplacer le volume partagé de cluster vers le nœud spécifié, soit modifier le nom `-SourceComputerName`de nœud que vous avez spécifié dans. Cette erreur a reçu un message amélioré dans Windows Server 2019.

## <a name="unable-to-access-the-data-drive-in-storage-replica-after-unexpected-reboot-when-bitlocker-is-enabled"></a>Impossible d’accéder au lecteur de données dans le réplica de stockage après un redémarrage inattendu lorsque BitLocker est activé

Si BitLocker est activé sur les deux lecteurs (lecteur de journal et lecteur de données) et sur les deux lecteurs de réplica de stockage, si le serveur principal redémarre, vous ne pouvez pas accéder au lecteur principal même après avoir déverrouillé le lecteur de journal de BitLocker.

Il s’agit d’un comportement attendu. Pour récupérer les données ou accéder au lecteur, vous devez d’abord déverrouiller le lecteur de journal, puis ouvrir diskmgmt. msc pour localiser le lecteur de données. Réactivez le lecteur de données en mode hors connexion et en ligne. Recherchez l’icône BitLocker sur le lecteur et déverrouillez le lecteur.

## <a name="issue-unlocking-the-data-drive-on-secondary-server-after-breaking-the-storage-replica-partnership"></a>Problème de déverrouillage du lecteur de données sur le serveur secondaire après avoir endommagé le partenariat de réplica de stockage

Après la désactivation du partenariat SR et la suppression du réplica de stockage, il est prévu que vous ne puissiez pas déverrouiller le lecteur de données du serveur secondaire avec son mot de passe ou sa clé respectifs. 

Vous devez utiliser la clé ou le mot de passe du lecteur de données du serveur principal pour déverrouiller le lecteur de données du serveur secondaire.

## <a name="test-failover-doesnt-mount-when-using-asynchronous-replication"></a>Le test de basculement n’est pas monté quand vous utilisez la réplication asynchrone

Lors de l’exécution de Mount-SRDestination pour mettre en ligne un volume de destination dans le cadre de la fonctionnalité de test de basculement, l’opération échoue avec l’erreur suivante :

    Mount-SRDestination: Unable to mount SR group <TEST>, detailed reason: The group or resource is not in the correct state to perform the supported operation.
    At line:1 char:1
    + Mount-SRDestination -ComputerName SRV1 -Name TEST -TemporaryP . . .
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (MSFT WvrAdminTasks : root/Microsoft/...(MSFT WvrAdminTasks : root/Microsoft/. T_WvrAdminTasks) (Mount-SRDestination], CimException
        + FullyQua1ifiedErrorId : Windows System Error 5823, Mount-SRDestination.  

Si vous utilisez un type de partenariat synchrone, le test de basculement fonctionne normalement.

Cela est dû à un défaut de code connu dans Windows Server, version 1709. Pour résoudre ce problème, installez la [mise à jour du 18 octobre 2018](https://support.microsoft.com/help/4462932/windows-10-update-kb4462932). Ce problème n’est pas présent dans Windows Server 2019 et Windows Server, version 1809 et ultérieures.

## <a name="see-also"></a>Voir aussi

- [Réplica de stockage](storage-replica-overview.md)  
- [Réplication de cluster étendu à l’aide d’un stockage partagé](stretch-cluster-replication-using-shared-storage.md)  
- [Réplication de stockage de serveur à serveur](server-to-server-storage-replication.md)  
- [Réplication de stockage de cluster à cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de stockage : Forum Aux Questions](storage-replica-frequently-asked-questions.md)  
- [Espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)  
