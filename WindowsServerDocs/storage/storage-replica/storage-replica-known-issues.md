---
title: Problèmes connus liés au réplica de stockage
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/22/2018
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 9be1a0ef25ce396fa319de99540348d0f8bc1372
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865190"
---
# <a name="known-issues-with-storage-replica"></a>Problèmes connus liés au réplica de stockage

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique traite des problèmes connus avec le réplica de stockage dans Windows Server.

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>Après la suppression de la réplication, les disques sont hors connexion et vous ne pouvez pas configurer à nouveau la réplication

Dans Windows Server2016, il est possible que vous ne puissiez pas configurer la réplication sur un volume qui a été précédemment répliqué ou que vous trouviez des volumes non montables. Cela peut se produire quand une condition d’erreur empêche la suppression de la réplication ou que vous réinstallez le système d’exploitation sur un ordinateur qui répliquait auparavant des données.  

Pour résoudre le problème, vous devez effacer la partition de réplica de stockage cachée des disques et rétablir un état accessible en écriture à l’aide de l’applet de commande `Clear-SRMetadata`.  

-   Pour supprimer tous les emplacements de bases de données de partition de réplica de stockage orphelins et remonter toutes les partitions, utilisez le paramètre `-AllPartitions` comme suit:  

    ```  
    Clear-SRMetadata -AllPartitions  
    ```  

-   Pour supprimer toutes les données du journal de réplica de stockage orphelines, utilisez le paramètre `-AllLogs` comme suit :  

    ```  
    Clear-SRMetadata -AllLogs  
    ```  

-   Pour supprimer toutes les données de configuration de cluster de basculement orphelines, utilisez le paramètre `-AllConfiguration` comme suit :  

    ```  
    Clear-SRMetadata -AllConfiguration  
    ```  

-   Pour supprimer des métadonnées de groupe de réplication individuelles, utilisez le paramètre `-Name` et spécifiez un groupe de réplication comme suit :  

    ```  
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

Ce problème a été résolu dans la mise à jour Cumulative pour Windows 10, version 1607 (mise à jour anniversaire) et Windows Server 2016 : Décembre 9 2016 (KB3201845). 

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>Échec des tentatives d’augmentation de la taille d’un volume répliqué en raison d'une étape manquante
Si vous tentez de redimensionner un volume répliqué sur le serveur source sans avoir au préalable défini `-AllowResizeVolume $TRUE`, vous recevez les erreurs suivantes :

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed

ID d’activité : {87aebbd6-4f47-4621-8aa4-5328dfa6c3be} ligne : 1 char : 1
    + Resize-Partition - lettre_lecteur I-taille de 8 Go
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

Disk Management Snap-in Error: 

    An unexpected error has occurred 

After resizing the volume, remember to disable resizing with `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE`. This parameter prevents admins from attempting to resize volumes prior to ensuring that there is sufficient space on the destination volume, typically because they were unaware of Storage Replica's presence. 

## Attempting to move a PDR resource between sites on an asynchronous stretch cluster fails
When attempting to move a physical disk resource-attached role - such as a file server for general use - in order to move the associated storage in an asynchronous stretch cluster, you receive an error.

If using the Failover Cluster Manager snap-in:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group
    
If using the Cluster powershell cmdlet:

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

This occurs due to a by-design behavior in Windows Server 2016. Use `Set-SRPartnership` to move these PDR disks in an asynchronous stretched cluster.  

This behavior has been changed in Windows Server, version 1709 to allow manual and automated failovers with asynchronous replication, based on customer feedback.

## Attempting to add disks to a two-node asymmetric cluster returns "No disks suitable for cluster disks found" 
When attempting to provision a cluster with only two nodes, prior to adding Storage Replica stretch replication, you attempt to add the disks in the second site to the Available Disks. You receive the following error:

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests." 

This does not occur if you have at least three nodes in the cluster. This issue occurs because of a by-design code change in Windows Server 2016 for asymmetric storage clustering behaviors. 

To add the storage, you can run the following command on the node in the second site:

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

This will not work with node local storage. You can use Storage Replica to replicate a stretch cluster between two total nodes, **each one using its own set of shared storage.** 

## The SMB Bandwidth limiter fails to throttle Storage Replica bandwidth
When specifying a bandwidth limit to Storage Replica, the limit is ignored and full bandwidth used. For example:

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

This issue occurs because of an interoperability issue between Storage Replica and SMB. This issue was first fixed in the July 2017 Cumulative Update of Windows Server 2016 and in Windows Server, version 1709.

## Event 1241 warning repeated during initial sync
When specifying a replication partnership is asynchronous, the source computer repeatedly logs warning event 1241 in the Storage Replica Admin channel. For example:

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

The asynchronous destination is currently disconnected. The RPO may become available after the connection is restored.

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

This is expected behavior during initial sync and can safely be ignored. This behavior may change in a later release. If you see this behavior during ongoing asynchronous replication, investigate the partnership to determine why replication is delayed beyond your configured RPO (30 seconds, by default).

## Event 4004 warning repeated after rebooting a replicated node
Under rare and usually unreproducable circumstances, rebooting a server that is in a partnership leads to replication failing and the rebooted node logging warning event 4004 with an access denied error.

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

Note the `Status: "{Access Denied}"` and the message `A process has requested access to an object, but has not been granted those access rights.` This is a known issue within Storage Replica and was fixed in Quality Update September 12, 2017—KB4038782 (OS Build 14393.1715) https://support.microsoft.com/en-us/help/4038782/windows-10-update-kb4038782 

## Error "Failed to bring the resource 'Cluster Disk x' online." with a stretch cluster
When attempting to bring a cluster disk online after a successful failover, where you are attempting to make the original source site primary again, you receive an error in Failover Cluster Manager. For example:

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.
    
    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.
    
If you attempt to move the disk or CSV manually, you receive an additional error. For example:

    Error
    The operation has failed.
    The action 'Move' did not complete.
    
    Error Code: 0x8007138d
    A cluster node is not available for this operation

This issue is caused by one or more uninitialzed disks being attached to one or more cluster nodes. To resolve the issue, initialize all attached storage using DiskMgmt.msc, DISKPART.EXE, or the Initialize-Disk PowerShell cmdlet.

We are working on providing an update that permanently resolves this issue. If you are interested in assisting us and you have a Microsoft Premier Support agreement, please email SRFEED@microsoft.com so that we can work with you on filing a backport request.

## GPT error when attempting to create a new SR partnership

When running New-SRPartnership, it fails with error: 

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

In the Failover Cluster Manager GUI, there is no option to setup Replication for the disk.

When running Test-SRTopology, it fails with: 

    WARNING: Object reference not set to an instance of an object.
    WARNING: System.NullReferenceException
    WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
       at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
       at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Object reference not set to an instance of an object.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

This is caused by the cluster functional level still being set to Windows Server 2012 R2 (i.e. FL 8). Storage Replica is supposed to return a specific error here but instead returns an incorrect error mapping.

Run Get-Cluster | fl * on each node.

If ClusterFunctionalLevel = 9, that is the Windows 2016 ClusterFunctionalLevel version needed to implement Storage Replica on this node.
If ClusterFunctionalLevel is not 9, the ClusterFunctionalLevel will need to be updated in order to implement Storage Replica on this node.

To resolve the issue, raise the cluster functional level by running the PowerShell cmdlet: Update-ClusterFunctionalLevel
https://technet.microsoft.com/itpro/powershell/windows/failoverclusters/update-clusterfunctionallevel

## Small unknown partition listed in DISKMGMT for each replicated volume

When running the Disk Management snap-in (DISKMGMT.MSC), you notice one or more volumes listed with no label or drive letter and 1MB in size. You may be able to delete the unknown volume or you may receive:

    "An Unexpected Error has Occurred"  

This behavior is by design. This not a volume, but a partition. Storage Replica creates a 512KB partition as a database slot for replication operations (the legacy DiskMgmt.msc tool rounds to the nearest MB). Having a partition like this for each replicated volume is normal and desirable. When no longer in use, you are free to delete this 512KB partition; in-use ones cannot be deleted. The partition will never grow or shrink. If you are recreating replication we recommend leaving the partition as Storage Replica will claim unused ones.

To view details, use the DISKPART tool or Get-Partition cmdlet. These partitions will have a GPT Type of `558d43c5-a1ac-43c0-aac8-d1472b2923d1`. 

## A Storage Replica node hangs when creating snapshots
When creating a VSS snapshot (through backup, VSSADMIN, etc) a Storage Replica node hangs, and you must force a restart of the node to recover. There is no error, just a hard hang of the server.

This issue occurs when you create a VSS snapshot of the log volume. The underlying cause is a legacy design aspect of VSS, not Storage Replica. The resulting behavior when you snapshot the Storage Replica log volume is a VSS I/O queing mechanism deadlocks the server.

To prevent this behavior, do not snapshot Storage Replica log volumes. There is no need to snapshot Storage Replica log volumes, as these logs cannot be restored. Furthermore, the log volume should never contain any other workloads, so no snapshot is needed in general.

## High IO latency increase when using Storage Spaces Direct with Storage Replica  
When using Storage Spaces Direct with an NVME or SSD cache, you see a greater than expected increase in latency when configuring Storage Replica replication between Storage Spaces Direct clusters. The change in latency is proportionally much higher than you see when using NVME and SSD in a performance + capacity configuration and no HDD tier nor capacity tier.

This issue occurs due to architectural limitations within Storage Replica's log mechanism combined with the extremely low latency of NVME when compared to slower media. When using the Storage Spaces Direct cache, all I/O of Storage Replica logs, along with all recent read/write IO of applications, will occur in the cache and never on the performance or capacity tiers. This means that all Storage Replica activity happens on the same speed media - this configuration issupported but not recommended (see https://aka.ms/srfaq for log recommendations). 

When using Storage Spaces Direct with HDDs, you cannot disable or avoid the cache. As a workaround, if using just SSD and NVME, you can configure just performance and capacity tiers. If using that configuration, and by placing the SR logs on the performance tier only with the data volumes they service being on the capacity tier only, you will avoid the high latency issue described above. The same could be done with a mix of faster and slower SSDs and no NVME.

This workaround is of course not ideal and some customers may not be able to make use of it. The Storage Replica team is working on optimizations and an updated log mechanism for the future to reduce these artificial bottlenecks. This v1.1 log first became available in Windows Server 2019 and its improved performance is described in on the [Server Storage Blog](https://blogs.technet.microsoft.com/filecab/2018/12/13/chelsio-rdma-and-storage-replica-perf-on-windows-server-2019-are-💯/).

## Error "Could not find file" when running Test-SRTopology between two clusters

When running Test-SRTopology between two clusters and their CSV paths, it fails with error: 

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

This is caused by a known code defect in Windows Server 2016. This issue was first fixed in Windows Server, version 1709 and the associated RSAT tools. For a downlevel resolution, please contact Microsoft Support and request a backport update. There is no workaround.

## Error "specified volume could not be found" when running Test-SRTopology between two clusters

When running Test-SRTopology between two clusters and their CSV paths, it fails with error: 

    PS C:\> Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ClusterStorage\Volume1 -SourceLogVolumeName L: -DestinationComputerName RRN44-14-13 -DestinationVolumeName C:\ClusterStorage\Volume1 -DestinationLogVolumeName L: -DurationInMinutes 30 -ResultPath c:\report

    Test-SRTopology : The specified volume C:\ClusterStorage\Volume1 cannot be found on computer RRN44-14-09. If this is a cluster node, the volume must be part of a role or CSV; volumes in Available Storage are not accessible
    At line:1 char:1
    + Test-SRTopology -SourceComputerName RRN44-14-09 -SourceVolumeName C:\ ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : ObjectNotFound: (:) [Test-SRTopology], Exception
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand

When specifying the source node CSV as the source volume, you must select the node that owns the CSV. You can either move the CSV to the specified node or change the node name you specified in `-SourceComputerName`. This error received an improved message in Windows Server 2019. 

## Unable to access the data drive in Storage Replica after unexpected reboot when BitLocker is enabled

If BitLocker is enabled on both drives (Log Drive and Data Drive) and in both Storage replica drives, if the Primary Server reboots then you are unable to access the Primary Drive even after unlocking the Log Drive from BitLocker.

This is an expected behavior. To recover the data or access the drive, you need to unlock the log drive first and then open Diskmgmt.msc to locate the data drive. Turn the data drive offline and online again. Locate the BitLocker icon on the drive and unlock the drive.

## Issue unlocking the Data drive on secondary server after breaking the Storage Replica partnership

After Disabling the SR Partnership and removing the Storage Replica, it is expected if you are unable to unlock the Secondary Server’s Data drive with its respective password or key. 

You need to use Key or Password of Primary Server’s Data drive to unlock the Secondary Server’s data drive.

## Test Failover doesn't mount when using asynchronous replication

When running Mount-SRDestination to bring a destination volume online as part of the Test Failover feature, it fails with error:

    Mount-SRDestination: Unable to mount SR group <TEST>, detailed reason: The group or resource is not in the correct state to perform the supported operation.
    At line:1 char:1
    + Mount-SRDestination -ComputerName SRV1 -Name TEST -TemporaryP . . .
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (MSFT WvrAdminTasks : root/Microsoft/...(MSFT WvrAdminTasks : root/Microsoft/. T_WvrAdminTasks) (Mount-SRDestination], CimException
        + FullyQua1ifiedErrorId : Windows System Error 5823, Mount-SRDestination.  

If using a synchronous partnership type, test failover works normally.    

This is caused by a known code defect in Windows Server, version 1709. To resolve this issue, install the [October 18, 2018 update](https://support.microsoft.com/help/4462932/windows-10-update-kb4462932). This issue isn't present in Windows Server 2019 and Windows Server, version 1809 and newer.

## See also

- [Storage Replica](storage-replica-overview.md)  
- [Stretch Cluster Replication Using Shared Storage](stretch-cluster-replication-using-shared-storage.md)  
- [Server to Server Storage Replication](server-to-server-storage-replication.md)  
- [Cluster to Cluster Storage Replication](cluster-to-cluster-storage-replication.md)  
- [Storage Replica: Frequently Asked Questions](storage-replica-frequently-asked-questions.md)  
- [Storage Spaces Direct](../storage-spaces/storage-spaces-direct-overview.md)  
