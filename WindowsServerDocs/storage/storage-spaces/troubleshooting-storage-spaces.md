---
title: Résolution des problèmes de espaces de stockage direct
description: Découvrez comment résoudre les problèmes liés à votre déploiement espaces de stockage direct.
keywords: Espaces de stockage
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 30fdda5ada01510027100efce1e95f310f69c6a1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865098"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Résoudre les espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Utilisez les informations suivantes pour résoudre les problèmes liés à votre déploiement espaces de stockage direct.

En général, commencez par les étapes suivantes :

1. Confirmez que la marque/le modèle de disque SSD est certifié pour Windows Server 2016 et Windows Server 2019 à l’aide du catalogue Windows Server. Vérifiez auprès du fournisseur que les disques sont pris en charge pour espaces de stockage direct.
2. Inspectez le stockage de tous les lecteurs défectueux. Utilisez le logiciel de gestion du stockage pour vérifier l’état des lecteurs. Si l’un des lecteurs est défectueux, travaillez avec votre fournisseur. 
3. Mettez à jour le stockage et le microprogramme si nécessaire.
   Vérifiez que les dernières mises à jour Windows sont installées sur tous les nœuds. Vous pouvez obtenir les dernières mises à jour pour Windows Server 2016 à partir de l' [historique des mises à jour Windows 10 et Windows server 2016](https://aka.ms/update2016) et pour windows server 2019 à partir de [l’historique des mises à jour Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Mettez à jour les pilotes et le microprogramme de la carte réseau.
5. Exécutez la validation de cluster et passez en revue la section espace de stockage direct, vérifiez que les lecteurs qui seront utilisés pour le cache sont signalés correctement et qu’il n’y a pas d’erreur.

Si vous rencontrez toujours des problèmes, passez en revue les scénarios ci-dessous.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Les ressources de disque virtuel ne sont pas dans un état de redondance
Les nœuds d’un espaces de stockage direct système redémarrent de manière inattendue en raison d’un incident ou d’une panne d’alimentation. Ensuite, un ou plusieurs disques virtuels peuvent ne pas être mis en ligne et la description « informations de redondance insuffisantes » s’affiche.

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Size| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| OK|  Healthy| True|  10 To|  Nœud-01. Conto...|
|Disk3         |Mirror                 |OK                          |Healthy       |True            |10 To | Nœud-01. Conto...|
|Disk2         |Mirror                 |Aucune redondance               |Unhealthy     |True            |10 To | Nœud-01. Conto...|
|Disk1         |Mirror                 |{Aucune redondance, InService}  |Unhealthy     |True            |10 To | Nœud-01. Conto...| 

En outre, après avoir essayé de mettre le disque virtuel en ligne, les informations suivantes sont consignées dans le journal du cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

L' **état opérationnel sans redondance** peut se produire en cas d’échec d’un disque ou si le système ne parvient pas à accéder aux données sur le disque virtuel. Ce problème peut se produire si un redémarrage se produit sur un nœud lors de la maintenance sur les nœuds.

Pour résoudre ce problème, procédez comme suit :

1. Supprimez les disques virtuels affectés du volume partagé de cluster. Cela les place dans le groupe « stockage disponible » du cluster et s’affichent sous la forme d’un ResourceType de « disque physique ».

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Sur le nœud qui possède le groupe de stockage disponible, exécutez la commande suivante sur chaque disque qui n’est pas dans un état de redondance. Pour identifier le nœud sur lequel se trouve le groupe « stockage disponible », vous pouvez exécuter la commande suivante.

   ```powershell
   Get-ClusterGroup
   ```
3. Définissez l’action de récupération du disque, puis démarrez le ou les disques.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Une réparation doit démarrer automatiquement. Attendez la fin de la réparation. Il est possible qu’il passe à l’état suspendu et redémarre. Pour surveiller la progression : 
    - Exécutez la fonctionnalité **obtenir-StorageJob** pour surveiller l’état de la réparation et savoir quand elle est terminée.
    - Exécutez la méthode **obtenir-VirtualDisk** et vérifiez que l’espace retourne un HealthStatus sain.
5. Une fois la réparation terminée et les disques virtuels sains, modifiez les paramètres du disque virtuel.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Mettre les disques hors connexion, puis de nouveau en ligne pour que les DiskRecoveryAction prennent effet :

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Rajoutez les disques virtuels affectés au volume partagé de cluster.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** est un commutateur de remplacement qui permet d’attacher le volume d’espace en mode lecture-écriture sans aucune vérification. La propriété vous permet d’effectuer des diagnostics en raison de la raison pour laquelle un volume ne sera pas mis en ligne. Elle est très similaire au mode de maintenance, mais vous pouvez l’appeler sur une ressource en état d’échec. Elle vous permet également d’accéder aux données, ce qui peut être utile dans des situations telles que « aucune redondance », où vous pouvez accéder à toutes les données que vous pouvez et copier. La propriété DiskRecoveryAction a été ajoutée au 22 février, 2018, Update, KB 4077525.


## <a name="detached-status-in-a-cluster"></a>État détaché dans un cluster 

Quand vous exécutez l’applet de commande **VirtualDisk** , le OperationalStatus pour un ou plusieurs disques virtuels espaces de stockage direct est détaché. Toutefois, le HealthStatus signalé par l’applet de commande **obtenir-PhysicalDisk** indique que tous les disques physiques sont dans un état sain.

Voici un exemple de sortie de l’applet de commande **VirtualDisk** .

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Size|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 OK|                  Healthy|       True|            10 To|  Nœud-01. Conto...|
|Disk3|         Mirror|                 OK|                  Healthy|       True|            10 To|  Nœud-01. Conto...|
|Disk2|         Mirror|                 Détaché|            Inconnu|       True|            10 To|  Nœud-01. Conto...|
|Disk1|         Mirror|                 Détaché|            Inconnu|       True|            10 To|  Nœud-01. Conto...| 


En outre, les événements suivants peuvent être consignés sur les nœuds :

```
Log Name: Microsoft-Windows-StorageSpaces-Driver/Operational
Source: Microsoft-Windows-StorageSpaces-Driver 
Event ID: 311 
Level: Error
User: SYSTEM 
Computer: Node#.contoso.local 
Description: Virtual disk {GUID} requires a data integrity scan.  

Data on the disk is out-of-sync and a data integrity scan is required. 

To start the scan, run the following command:   
Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask                

Once you have resolved the condition listed above, you can online the disk by using the following commands in PowerShell:   

Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsReadOnly $false 
Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsOffline  $false
------------------------------------------------------------

Log Name: System
Source: Microsoft-Windows-ReFS 
Event ID: 134
Level: Error 
User: SYSTEM
Computer: Node#.contoso.local 
Description: The file system was unable to write metadata to the media backing volume <VolumeId>. A write failed with status "A device which does not exist was specified." ReFS will take the volume offline. It may be mounted again automatically.
------------------------------------------------------------
Log Name: Microsoft-Windows-ReFS/Operational
Source: Microsoft-Windows-ReFS 
Event ID: 5 
Level: Error 
User: SYSTEM 
Computer: Node#.contoso.local 
Description: ReFS failed to mount the volume. 
Context: 0xffffbb89f53f4180 
Error: A device which does not exist was specified.
Volume GUID:{00000000-0000-0000-0000-000000000000} 
DeviceName: 
Volume Name:
``` 

L' **état opérationnel détaché** peut se produire si le journal DRT (Dirty Region Tracking) est saturé. Les espaces de stockage utilisent le suivi des régions modifiées (DRT) pour les espaces en miroir pour s’assurer qu’en cas de panne de courant, toutes les mises à jour en cours des métadonnées sont journalisées pour s’assurer que l’espace de stockage peut rétablir ou annuler les opérations pour ramener l’espace de stockage dans un État flexible. et un état cohérent lorsque l’alimentation est restaurée et que le système est rétabli. Si le journal DRT est plein, le disque virtuel ne peut pas être mis en ligne tant que les métadonnées DRT ne sont pas synchronisées et vidées. Ce processus nécessite l’exécution d’une analyse complète, qui peut prendre plusieurs heures.

Pour résoudre ce problème, procédez comme suit :
1. Supprimez les disques virtuels affectés du volume partagé de cluster.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Exécutez les commandes suivantes sur chaque disque qui n’est pas en ligne. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Exécutez la commande suivante sur chaque nœud dans lequel le volume détaché est en ligne. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Cette tâche doit être lancée sur tous les nœuds sur lesquels le volume détaché est en ligne. Une réparation doit démarrer automatiquement. Attendez la fin de la réparation. Il est possible qu’il passe à l’état suspendu et redémarre. Pour surveiller la progression : 
   - Exécutez la fonctionnalité **obtenir-StorageJob** pour surveiller l’état de la réparation et savoir quand elle est terminée.
   - Exécutez la méthode **obtenir-VirtualDisk** et vérifiez que l’espace retourne un HealthStatus sain.
     - L’option « analyse de l’intégrité des données pour la récupération après incident » est une tâche qui ne s’affiche pas en tant que tâche de stockage et il n’y a aucun indicateur de progression. Si la tâche est affichée comme étant en cours d’exécution, elle est en cours d’exécution. Une fois l’opération terminée, elle s’affiche comme terminée.

       En outre, vous pouvez afficher l’état d’une tâche de planification en cours d’exécution à l’aide de l’applet de commande suivante : 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Dès que l’analyse de l’intégrité des données pour la récupération sur incident est terminée, la réparation se termine et les disques virtuels sont sains, et les paramètres de disque virtuel sont restaurés. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```

5. Mettre les disques hors connexion, puis de nouveau en ligne pour que les DiskRecoveryAction prennent effet :

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 

6. Rajoutez les disques virtuels affectés au volume partagé de cluster.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   La **valeur 7 de DiskRunChkdsk** est utilisée pour attacher le volume d’espace et la partition en mode lecture seule. Cela permet aux espaces de se découvrir et de s’auto-réparer en déclenchant une réparation. La réparation s’exécutera automatiquement une fois montée. Elle vous permet également d’accéder aux données, ce qui peut être utile pour accéder à toutes les données que vous pouvez copier. Pour certaines conditions d’erreur, par exemple, un journal DRT complet, vous devez exécuter l’analyse de l’intégrité des données pour la tâche planifiée de récupération après incident.

**L’analyse de l’intégrité des données pour la tâche de récupération après incident** est utilisée pour synchroniser et effacer un journal DRT (Dirty Region Tracking) complet. La réalisation de cette tâche peut prendre plusieurs heures. L’option « analyse de l’intégrité des données pour la récupération après incident » est une tâche qui ne s’affiche pas en tant que tâche de stockage et il n’y a aucun indicateur de progression. Si la tâche est affichée comme étant en cours d’exécution, elle est en cours d’exécution. Une fois l’opération terminée, elle s’affiche comme terminée. Si vous annulez la tâche ou que vous redémarrez un nœud pendant que cette tâche est en cours d’exécution, la tâche devra recommencer depuis le début.

Pour plus d’informations, consultez [résolution des problèmes de espaces de stockage direct intégrité et des États opérationnels](storage-spaces-states.md).

## <a name="event-5120-with-status_io_timeout-c00000b5"></a>Événement 5120 avec STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **Pour Windows Server 2016 :** Pour réduire le risque de rencontrer ces problèmes lors de l’application de la mise à jour avec le correctif, il est recommandé d’utiliser la procédure du mode de maintenance du stockage ci-dessous pour installer la [mise à jour cumulative 18 octobre 2018 pour Windows Server 2016](https://support.microsoft.com/help/4462928) ou une version ultérieure Lorsque les nœuds ont installé une mise à jour cumulative de Windows Server 2016 publiée du [2018 8 mai](https://support.microsoft.com/help/4103723) au [9 octobre 2018](https://support.microsoft.com/help/KB4462917).

Vous pouvez obtenir l’événement 5120 avec STATUS_IO_TIMEOUT c00000b5 après avoir redémarré un nœud sur Windows Server 2016 avec la mise à jour cumulative qui a été publiée du [8 mai à 2018 kb 4103723](https://support.microsoft.com/help/4103723) au [9 octobre, 2018 Ko 4462917](https://support.microsoft.com/help/4462917) installé.

Lorsque vous redémarrez le nœud, l’événement 5120 est consigné dans le journal des événements système et comprend l’un des codes d’erreur suivants :

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Lorsqu’un événement 5120 est enregistré, un vidage instantané est généré pour collecter des informations de débogage susceptibles de provoquer des symptômes supplémentaires ou d’avoir un effet sur les performances. La génération de l’image mémoire dynamique crée une brève pause pour permettre la création d’un instantané de la mémoire pour l’écriture du fichier de vidage. Les systèmes disposant d’une grande quantité de mémoire et sont soumis à un stress peuvent entraîner la suppression de l’appartenance au cluster des nœuds et entraîner l’enregistrement de l’événement 1135 suivant.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Une modification a été introduite dans le 8 mai 2018 à Windows Server 2016, qui était une mise à jour cumulative pour ajouter des handles résilients SMB pour le espaces de stockage direct les sessions réseau SMB intra-cluster. Cela a été fait pour améliorer la résilience aux défaillances réseau temporaires et améliorer la façon dont RoCE gère l’encombrement du réseau. Ces améliorations ont également augmenté par inadvertance les délais d’attente lorsque les connexions SMB essaient de se reconnecter et attendent d’expirer lorsqu’un nœud est redémarré. Ces problèmes peuvent affecter un système qui est soumis à un stress. Pendant les temps d’arrêt non planifiés, les pauses d’e/s d’un maximum de 60 secondes ont également été observées lorsque le système attend des connexions à expiration. Pour résoudre ce problème, installez la [mise à jour cumulative 18 octobre 2018 pour Windows Server 2016](https://support.microsoft.com/help/4462928) ou une version ultérieure.

*Remarque* Cette mise à jour aligne les délais d’expiration du CSV avec les délais d’expiration de la connexion SMB pour résoudre ce problème. Elle n’implémente pas les modifications pour désactiver la génération de vidages dynamiques mentionnée dans la section solution de contournement.

### <a name="shutdown-process-flow"></a>Déroulement du processus d’arrêt :

1. Exécutez l’applet de commande VirtualDisk et assurez-vous que la valeur HealthStatus est saine.
2. Drainez le nœud en exécutant l’applet de commande suivante :

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Placez les disques sur ce nœud en mode maintenance du stockage en exécutant l’applet de commande suivante : 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Exécutez l’applet de commande **obten-PhysicalDisk** et assurez-vous que la valeur OperationalStatus est en mode maintenance.
5. Exécutez l’applet de commande **Restart-Computer** pour redémarrer le nœud.
6. Après le redémarrage du nœud, supprimez les disques de ce nœud du mode de maintenance du stockage en exécutant l’applet de commande suivante :

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Reprenez le nœud en exécutant l’applet de commande suivante :

   ```powershell
   Resume-ClusterNode
   ```
8. Vérifiez l’état des travaux de resynchronisation en exécutant l’applet de commande suivante :

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>Désactivation des vidages instantanés
Pour atténuer l’effet de la génération de vidages instantanés sur les systèmes qui ont beaucoup de mémoire et qui sont en trop sollicités, vous pouvez également souhaiter désactiver la génération de vidages dynamiques. Trois options sont fournies ci-dessous.

>[!Caution]
>Cette procédure peut empêcher la collecte d’informations de diagnostic que Support Microsoft devrez peut-être examiner ce problème. Un agent de support peut avoir à vous demander de réactiver la génération de vidages instantanés en fonction de scénarios de dépannage spécifiques.

Il existe deux méthodes pour désactiver les dumps dynamiques, comme décrit ci-dessous.

#### <a name="method-1-recommended-in-this-scenario"></a>Méthode 1 (recommandé dans ce scénario)
Pour désactiver complètement tous les vidages, y compris les dumps dynamiques à l’ensemble du système, procédez comme suit :

1. Créez la clé de Registre suivante : HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Sous la nouvelle clé **ForceDumpsDisabled** , créez une propriété REG_DWORD en tant que GuardedHost, puis définissez sa valeur sur 0x10000000.
3. Appliquez la nouvelle clé de Registre à chaque nœud de cluster.

>[!NOTE]
>Vous devez redémarrer l’ordinateur pour que la modification nParamètre prenne effet.

Une fois cette clé de Registre définie, la création de vidages dynamiques échoue et génère une erreur « STATUS_NOT_SUPPORTED ».

#### <a name="method-2"></a>Méthode 2
Par défaut, Rapport d’erreurs Windows n’autorise qu’un seul LiveDump par type de rapport par période de 7 jours et seulement 1 LiveDump par ordinateur pour 5 jours. Vous pouvez modifier cela en définissant les clés de Registre suivantes pour qu’une seule LiveDump sur l’ordinateur soit toujours autorisée.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Remarque* Vous devez redémarrer l’ordinateur pour que la modification soit prise en compte.

### <a name="method-3"></a>Méthode 3
Pour désactiver la génération de clusters de vidages dynamiques (par exemple, quand un événement 5120 est enregistré), exécutez l’applet de commande suivante :

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Cette applet de commande a un effet immédiat sur tous les nœuds de cluster sans redémarrage de l’ordinateur.

## <a name="slow-io-performance"></a>Ralentissement des performances d’e/s

Si vous constatez des performances d’e/s lentes, vérifiez si le cache est activé dans votre configuration de espaces de stockage direct. 

Il existe deux façons de vérifier : 


1. Utilisation du journal de cluster. Ouvrez le journal du cluster dans l’éditeur de texte de votre choix et recherchez « [= = = SBL Disks = = =] ». Il s’agit d’une liste du disque sur le nœud sur lequel le journal a été généré. 

     Exemples de disques activés pour le cache : Notez ici que l’État est CacheDiskStateInitializedAndBound et qu’un GUID est présent ici. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non activé : Ici, nous pouvons voir qu’il n’y a aucun GUID présent et que l’État est CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache non activé : Lorsque tous les disques ont le même type de casse n’est pas activé par défaut. Ici, nous pouvons voir qu’il n’y a aucun GUID présent et que l’État est CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. Utilisation de Get-PhysicalDisk. XML à partir de SDDCDiagnosticInfo
    1. Ouvrez le fichier XML à l’aide de « $d = Import-Clixml GetPhysicalDisk. XML »
    2. Exécuter « stockage de bureau à partir »
    3. Exécutez « $d ». Notez que l’utilisation est sélection automatique, et non Journal, vous verrez une sortie semblable à celle-ci : 

   |FriendlyName|  SerialNumber| Type de média| CanPool| OperationalStatus| HealthStatus| Utilisation| Size|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |INTEL SSDPE7KX02 NVMe| PHLF733000372P0LGN| SSD| False|   OK|                Healthy|      Sélectionner automatiquement 1,82 to|
   |INTEL SSDPE7KX02 NVMe |PHLF7504008J2P0LGN| SSD|  False|    OK|                Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe| PHLF7504005F2P0LGN| SSD|  False|  OK|                Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe |PHLF7504002A2P0LGN| SSD| False| OK|    Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe| PHLF7504004T2P0LGN |SSD| False|OK|       Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe |PHLF7504002E2P0LGN| SSD| False| OK|      Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe |PHLF7330002Z2P0LGN| SSD| False| OK|      Healthy|Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe |PHLF733000272P0LGN |SSD| False| OK|  Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe |PHLF7330001J2P0LGN |SSD| False| OK| Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe| PHLF733000302P0LGN |SSD| False| OK|Healthy| Sélection automatique| 1,82 TO|
   |INTEL SSDPE7KX02 NVMe| PHLF7330004D2P0LGN |SSD| False| OK| Healthy| Sélection automatique |1,82 TO|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Comment détruire un cluster existant afin de pouvoir réutiliser les mêmes disques

Dans un cluster espaces de stockage direct, une fois que vous désactivez espaces de stockage direct et que vous utilisez le processus de nettoyage décrit dans [nettoyer les lecteurs](deploy-storage-spaces-direct.md#step-31-clean-drives), le pool de stockage en cluster reste dans un État hors connexion et le service de contrôle d’intégrité est supprimé du cluster.

L’étape suivante consiste à supprimer le pool de stockage fantôme : 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Maintenant, si vous exécutez la fonction **obtenir-PhysicalDisk** sur l’un des nœuds, vous verrez tous les disques qui se trouvaient dans le pool. Par exemple, dans un laboratoire avec un cluster à 4 nœuds avec 4 disques SAS, 100 Go chacun présenté à chaque nœud. Dans ce cas, une fois que l’espace de stockage direct est désactivé, ce qui supprime la couche de bus de stockage (SBL), mais laisse le filtre, si vous exécutez l’opération **de récupération sur disque physique**, il doit signaler 4 disques à l’exclusion du disque de système d’exploitation local. Au lieu de cela, il a indiqué 16. Cela est identique pour tous les nœuds du cluster. Lorsque vous exécutez une commande d' **extraction de disque** , les disques attachés localement sont numérotés en tant que 0, 1, 2, etc., comme illustré dans l’exemple de sortie suivant :

|Number| Nom convivial| Numéro de série|HealthStatus|OperationalStatus|Taille totale| Style de partition|
|-|-|-|-|-|-|-|-|
|0|Virtu msft...  ||Healthy | Online|  127 Go| GPT|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
|1|Virtu msft...||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
|2|Virtu msft...||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
|4|Virtu msft...||Healthy| Hors ligne| 100 Go| RAW|
|3|Virtu msft...||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|
||Virtu msft... ||Healthy| Hors ligne| 100 Go| RAW|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Message d’erreur relatif au type de média non pris en charge lorsque vous créez un cluster espaces de stockage direct à l’aide de Enable-ClusterS2D  

Vous pouvez voir des erreurs similaires à celles-ci quand vous exécutez l’applet de commande **Enable-ClusterS2D** :

![Message d’erreur du scénario 6](media/troubleshooting/scenario-error-message.png)

Pour résoudre ce problème, assurez-vous que l’adaptateur HBA est configuré en mode HBA. Aucun HBA ne doit être configuré en mode RAID.  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect se bloque lors de l’attente jusqu’à ce que les disques SBL soient exposés, ou à 27%

Les informations suivantes s’affichent dans le rapport de validation :

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


Le problème est lié à la carte d’extension HPE SAS qui se trouve entre les disques et la carte HBA. L’Expander SAS crée un ID en double entre le premier lecteur connecté à l’Expander et l’Expander lui-même.  Cela a été résolu dans [le microprogramme SAS Expander des contrôleurs HPE Smart Array : 4,02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>La série P4600 Intel SSD DC a un NGUID non unique
Vous pouvez voir un problème où un appareil Intel SSD DC P4600 Series semble être à l’origine de la création de plusieurs espaces de noms, tels que 0100000001000000E4D25C000014E214 ou 0100000001000000E4D25C0000EEE214 dans l’exemple ci-dessous.


|               quei               | deviceid | Type de média | BusType |               SerialNumber               |      size      | canpool | convivial | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| adresse EUI. 0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    CARTES     |   SSDPE2KE016T7   |
| adresse EUI. 0100000001000000E4D25C000014E214 |    5\.     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    CARTES     |   SSDPE2KE016T7   |
| adresse EUI. 0100000001000000E4D25C0000EEE214 |    6\.     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    CARTES     |   SSDPE2KE016T7   |
| adresse EUI. 0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    CARTES     |   SSDPE2KE016T7   |

Pour résoudre ce problème, mettez à jour le microprogramme sur les disques Intel vers la dernière version.  La version du microprogramme QDV101B1 de mai 2018 est connue pour résoudre ce problème.

La [version 2018 de l’outil Centre de données SSD Intel](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) comprend une mise à jour du microprogramme, QDV101B1, pour la série Intel SSD DC P4600.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>Le disque physique « sain » et l’état opérationnel « suppression du pool » 

Dans un cluster Windows Server 2016 espaces de stockage direct, vous pouvez voir le HealthStatus pour un ou plusieurs disques physiques comme « intègres », tandis que OperationalStatus est « (suppression du pool, OK) ». 

« La suppression du pool » est un jeu d’intention lorsque **Remove-PhysicalDisk** est appelé mais stocké dans l’intégrité pour maintenir l’État et autoriser la récupération en cas d’échec de l’opération de suppression. Vous pouvez modifier manuellement le OperationalStatus en sain avec l’une des méthodes suivantes :

- Supprimez le disque physique du pool, puis rajoutez-le.
- Exécutez le [script Clear-PhysicalDiskHealthData. ps1](https://go.microsoft.com/fwlink/?linkid=2034205) pour effacer l’intention. (Disponible en téléchargement en tant que. Fichier TXT. Vous devez l’enregistrer en tant que. Fichier PS1 avant de pouvoir l’exécuter.)

Voici quelques exemples illustrant comment exécuter le script :

- Utilisez le paramètre **SerialNumber** pour spécifier le disque que vous devez définir comme sain. Vous pouvez récupérer le numéro de série à partir de **WMI MSFT_PhysicalDisk** ou de la **récupération d’un disque physique**. (Nous utilisons simplement 0 pour le numéro de série ci-dessous.)

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Utilisez le paramètre **UniqueID** pour spécifier le disque (à nouveau à partir de **WMI MSFT_PhysicalDisk** ou de la **récupération-PhysicalDisk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>La copie de fichier est lente

Vous pouvez rencontrer un problème à l’aide de l’Explorateur de fichiers pour copier un disque dur virtuel volumineux vers le disque virtuel-la copie de fichiers prend plus de temps que prévu.

L’utilisation de l’Explorateur de fichiers, Robocopy ou Xcopy pour copier un disque dur virtuel de grande taille sur le disque virtuel n’est pas une méthode recommandée, car cela entraîne des performances plus lentes que prévu. Le processus de copie ne passe pas par la pile espaces de stockage direct, qui se trouve en dessous de la pile de stockage, et agit plutôt comme un processus de copie local.

Si vous souhaitez tester les performances de espaces de stockage direct, nous vous recommandons d’utiliser VMFleet et Diskspd pour charger et stresser les serveurs afin d’obtenir une ligne de base et de définir les attentes en matière de performances des espaces de stockage direct.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Événements attendus que vous pouvez voir sur le reste des nœuds lors du redémarrage d’un nœud. 

Vous pouvez ignorer ces événements en toute sécurité : 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Si vous exécutez des machines virtuelles Azure, vous pouvez ignorer cet événement :

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Erreurs de ralentissement des performances ou de « perte de communication », « erreur d’e/s », « détaché » ou « aucune redondance » pour les déploiements qui utilisent des appareils Intel P3x00 NVMe

Nous avons identifié un problème critique qui affecte certains espaces de stockage direct utilisateurs qui utilisent du matériel basé sur la famille Intel P3x00 d’appareils NVM Express (NVMe) avec des versions de microprogrammes antérieures à « maintenance Release 8 ». 

>[!NOTE]
> Les fabricants d’ordinateurs OEM peuvent avoir des appareils basés sur la famille Intel P3x00 d’appareils NVMe avec des chaînes de version de microprogramme uniques. Pour plus d’informations sur la dernière version du microprogramme, contactez votre fabricant d’ordinateurs OEM.

Si vous utilisez du matériel dans votre déploiement basé sur la famille Intel P3x00 d’appareils NVMe, nous vous recommandons d’appliquer immédiatement le dernier microprogramme disponible (au moins la version de maintenance 8). Cet [article support Microsoft](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fournit des informations supplémentaires sur ce problème. 
