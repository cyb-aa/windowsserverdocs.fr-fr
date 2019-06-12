---
title: Résolution des problèmes directe des espaces de stockage
description: Découvrez comment résoudre les problèmes de votre déploiement d’espaces de stockage Direct.
keywords: Espaces de stockage
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 44bcf48f3e4a3b4b49ff027d3aa3e5704865e7b5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447882"
---
# <a name="troubleshoot-storage-spaces-direct"></a>Résoudre les espaces de stockage Direct

> S’applique à : Windows Server 2019, Windows Server 2016

Utilisez les informations suivantes pour résoudre les problèmes de votre déploiement d’espaces de stockage Direct.

En règle générale, vous démarrez avec les étapes suivantes :

1. Confirmer que la marque/modèle de disque SSD est certifié pour Windows Server 2016 et Windows Server 2019 à l’aide du catalogue Windows Server. Vérifiez auprès des fournisseurs que les lecteurs sont pris en charge pour les espaces de stockage Direct.
2. Inspecter le stockage pour tous les lecteurs défectueux. Utilisez le logiciel de gestion de stockage pour vérifier l’état des disques. Si tous les lecteurs sont défectueux, demandez à votre fournisseur. 
3. Mettre à jour le stockage et le microprogramme de lecteur, si nécessaire.
   Vérifiez que les dernières mises à jour Windows sont installés sur tous les nœuds. Vous pouvez obtenir les dernières mises à jour pour Windows Server 2016 à partir de [l’historique de mise à jour de Windows 10 et Windows Server 2016](https://aka.ms/update2016) et pour Windows Server 2019 à partir de [l’historique de mise à jour de Windows 10 et Windows Server 2019](https://support.microsoft.com/help/4464619).
4. Mettre à jour des microprogrammes et les pilotes de carte réseau.
5. Exécuter la validation de cluster et passez en revue la section espace de stockage Direct, vérifiez les lecteurs qui seront utilisés pour le cache sont signalées correctement et qu’aucune erreur.

Si vous rencontrez toujours des problèmes, passez en revue les scénarios ci-dessous.

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>Ressources de disque virtuel sont dans un état non redondance
Les nœuds d’un système d’espaces de stockage Direct redémarrer de façon inattendue en raison d’une défaillance de l’incident ou hors tension. Ensuite, un ou plusieurs des disques virtuels ne soient pas en ligne, et vous voyez la description « pas suffisamment d’informations redondance. »

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Size| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disquette n° 4| Mirror| OK|  Healthy| True|  10 TO|  Nœud-01.conto...|
|Disk3         |Mirror                 |OK                          |Healthy       |True            |10 TO | Nœud-01.conto...|
|Disk2         |Mirror                 |Pas de redondance               |Unhealthy     |True            |10 TO | Nœud-01.conto...|
|Disque 1         |Mirror                 |{Pas de redondance, InService}  |Unhealthy     |True            |10 TO | Nœud-01.conto...| 

En outre, après une tentative de mettre le disque virtuel en ligne, les informations suivantes sont enregistrées dans le journal de Cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

Le **état opérationnel de redondance non** peut se produire si un disque a échoué ou si le système ne peut pas accéder aux données sur le disque virtuel. Ce problème peut se produire si un redémarrage se produit sur un nœud lors de la maintenance sur les nœuds.

Pour résoudre ce problème, procédez comme suit :

1. Supprimez les disques virtuels affectés de volume partagé de cluster. Cela les placer dans le groupe « Stockage disponible » dans le cluster et commencer à afficher comme un type de ressource de « Disque physique ».

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Sur le nœud qui possède le groupe de stockage disponible, exécutez la commande suivante sur chaque disque qui se trouve dans un état non redondance. Pour identifier quel nœud du groupe « Stockage disponible » se trouve sur vous pouvez exécuter la commande suivante.

   ```powershell
   Get-ClusterGroup
   ```
3. Définir l’action de récupération de disque, puis démarrez l’ou les disques.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Une réparation doit démarrer automatiquement. Attendez que la réparation se termine. Il peut aller dans un état suspendu et recommencer. Pour surveiller la progression : 
    - Exécutez **Get-StorageJob** pour surveiller l’état de la réparation et voir que l’opération est terminée.
    - Exécutez **Get-VirtualDisk** et vérifiez que l’espace retourne HealthStatus est sain.
5. Après la réparation terminée et les disques virtuels sont sain, changez les paramètres de disque virtuel de sauvegarde.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Prenez l’ou les disques en mode hors connexion et puis en ligne à nouveau pour que la DiskRecoveryAction prennent effet :

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Ajoutez les disques virtuels affectés au volume partagé de cluster.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** est un commutateur de remplacement qui permet d’attacher le volume d’espace en mode lecture-écriture sans aucun contrôle. La propriété vous permet de faire des diagnostics dans pourquoi un volume ne sont pas mis en ligne. Il est très semblable au Mode de Maintenance, mais vous pouvez l’appeler sur une ressource dans un état d’échec. Il vous permet également d’accéder aux données, qui peuvent être utiles dans les situations telles que « No redondance », où vous pouvez accéder à toutes les données, vous pouvez et copiez-la. La propriété DiskRecoveryAction a été ajoutée dans le 22 février 2018, mise à jour, 4077525 de la base de connaissances.


## <a name="detached-status-in-a-cluster"></a>État détaché dans un cluster 

Lorsque vous exécutez le **Get-VirtualDisk** applet de commande, le OperationalStatus pour un ou plusieurs espaces de stockage Direct des disques virtuels est détachée. Toutefois, le HealthStatus signalés par le **Get-PhysicalDisk** applet de commande indique que tous les disques physiques sont dans un état sain.

Voici un exemple de sortie de la **Get-VirtualDisk** applet de commande.

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Size|   PSComputerName|
|-|-|-|-|-|-|-|
|Disquette n° 4|         Mirror|                 OK|                  Healthy|       True|            10 TO|  Nœud-01.conto...|
|Disk3|         Mirror|                 OK|                  Healthy|       True|            10 TO|  Nœud-01.conto...|
|Disk2|         Mirror|                 Détaché|            Inconnu|       True|            10 TO|  Nœud-01.conto...|
|Disque 1|         Mirror|                 Détaché|            Inconnu|       True|            10 TO|  Nœud-01.conto...| 


En outre, les événements suivants peuvent être enregistrés sur les nœuds :

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

Le **état opérationnel détachée** peut se produire si la région modifiée (DRT) de suivi des journaux est plein. Espaces de stockage utilise des régions modifiées suivi (DRT) pour les espaces en miroir pour s’assurer que lorsqu’une panne de courant se produit, les mises à jour en cours aux métadonnées sont connectés pour vous assurer que l’espace de stockage peut rétablir ou annuler les opérations afin de rétablir l’espace de stockage dans un flexible et un état cohérent lors de l’alimentation est restaurée et le système fonctionne à nouveau. Si le journal de la fonction DRT est plein, le disque virtuel ne peut pas être mis en ligne jusqu'à ce que les métadonnées de la fonction DRT sont synchronisée et vidées. Ce processus nécessite une analyse complète, ce qui peut prendre plusieurs heures en cours d’exécution.

Pour résoudre ce problème, procédez comme suit :
1. Supprimez les disques virtuels affectés de volume partagé de cluster.

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Exécutez les commandes suivantes sur chaque disque qui n’est pas mis en ligne. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. Exécutez la commande suivante sur chaque nœud dans lequel le volume détaché est en ligne. 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   Cette tâche doit être initiée sur tous les nœuds sur lesquels le volume détaché est en ligne. Une réparation doit démarrer automatiquement. Attendez que la réparation se termine. Il peut aller dans un état suspendu et recommencer. Pour surveiller la progression : 
   - Exécutez **Get-StorageJob** pour surveiller l’état de la réparation et voir que l’opération est terminée.
   - Exécutez **Get-VirtualDisk** et vérifiez l’espace retourne HealthStatus est sain.
     - Le « intégrité d’analyse pour se bloquer récupération des données » est une tâche qui n’apparaît pas comme un travail de stockage, et il n’existe aucun indicateur de progression. Si la tâche ne s’affichent comme en cours d’exécution, il est en cours d’exécution. Lorsqu’elle est terminée, elle indiquera terminée.

       En outre, vous pouvez afficher l’état d’une tâche planifiée en cours d’exécution à l’aide de l’applet de commande suivante : 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Dès que les « données l’intégrité d’analyse pour récupération après blocage » est terminée, la fin de la réparation et les disques virtuels sont sain, changez les paramètres de disque virtuel de sauvegarde. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Ajoutez les disques virtuels affectés au volume partagé de cluster.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   **Valeur DiskRunChkdsk 7** est utilisé pour attacher le volume d’espace et avoir la partition en mode lecture seule. Ainsi, les espaces de découvrir eux-mêmes et réparer en déclenchant une réparation. Réparation s’exécute automatiquement une fois monté. Il vous permet également d’accéder aux données, qui peuvent être utiles pour accéder à toutes les données permettant de copier. Pour certaines conditions d’erreur, tels qu’un journal DRT complète, vous devez exécuter l’analyse de l’intégrité des données pour la tâche planifiée de récupération après blocage.

**Analyse de l’intégrité des données pour la tâche de récupération après blocage** est utilisé pour synchroniser et effacer le journal de suivi (DRT) une région modifiée complète. Cette tâche peut prendre plusieurs heures. Le « intégrité d’analyse pour se bloquer récupération des données » est une tâche qui n’apparaît pas comme un travail de stockage, et il n’existe aucun indicateur de progression. Si la tâche ne s’affichent comme en cours d’exécution, il est en cours d’exécution. Lorsqu’elle est terminée, elle affichera comme étant terminés. Si vous annulez la tâche ou un nœud pendant l’exécution de cette tâche, la tâche devrez recommencer depuis le début.

Pour plus d’informations, consultez [intégrité résolution des espaces de stockage Direct et les états opérationnels](storage-spaces-states.md).

## <a name="event-5120-with-statusiotimeout-c00000b5"></a>Événement 5120 avec STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> **Pour Windows Server 2016 :** Pour réduire le risque de ces symptômes lors de l’application de la mise à jour avec le correctif, il est recommandé d’utiliser la procédure de Mode de Maintenance de stockage ci-dessous pour installer le [18 octobre 2018, la mise à jour cumulative pour Windows Server 2016](https://support.microsoft.com/help/4462928)ou une version ultérieure, lorsque les nœuds actuellement installé une mise à jour cumulative Windows Server 2016 à qui a été publiée à partir de [le 8 mai 2018](https://support.microsoft.com/help/4103723) à [9 octobre 2018](https://support.microsoft.com/help/KB4462917).

Vous pouvez obtenir l’événement 5120 avec STATUS_IO_TIMEOUT c00000b5 après le redémarrage d’un nœud sur Windows Server 2016 avec mise à jour cumulative qui ont été publiés à partir de [8 mai 2018 Ko 4103723](https://support.microsoft.com/help/4103723) à [le 9 octobre 2018 Ko 4462917](https://support.microsoft.com/help/4462917)installé.

Lorsque vous redémarrez le nœud, 5120 d’événement est consigné dans le journal des événements système et inclut l’une des codes d’erreur suivants :

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Lorsqu’un événement de 5120 est connecté, un vidage en direct est généré pour collecter des informations de débogage qui peuvent provoquer des problèmes supplémentaires ou qui ont un effet sur les performances. Génération de l’image mémoire dynamique crée une courte pause pour activer la prise d’un instantané de mémoire pour écrire le fichier de vidage. Les systèmes qui ont beaucoup de mémoire et sont en situation de stress risque de nœuds à supprimer en dehors de l’appartenance au cluster et également provoquer le 1135 événement suivant à enregistrer.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Un changement introduit dans le 8 mai 2018 vers Windows Server 2016, ce qui était une mise à jour cumulative pour ajouter gère résilientes SMB pour les espaces de stockage Direct intra-cluster réseau les sessions SMB. Cela a été fait pour améliorer la résilience aux défaillances réseau temporaires et améliorer la façon dont RoCE gère la congestion du réseau. Ces améliorations augmenté par inadvertance des délais d’expiration lorsque les connexions SMB essaient de vous reconnecter et attend de délai d’attente lorsqu’un nœud est redémarré. Ces problèmes peuvent affecter un système est en situation de stress. Pendant les temps d’arrêt non planifié, interruptions d’e/s de jusqu'à 60 secondes également ont été observées pendant que le système en attente pour les connexions à un délai d’attente. Pour résoudre ce problème, installez le [18 octobre 2018, la mise à jour cumulative pour Windows Server 2016](https://support.microsoft.com/help/4462928) ou une version ultérieure.

*Remarque* cette mise à jour aligne les délais d’attente CSV avec des délais d’expiration de connexion SMB pour résoudre ce problème. Il n’implémente pas les modifications pour désactiver la génération dynamique de vidage mentionnée dans la section solution de contournement.

### <a name="shutdown-process-flow"></a>Flux de processus d’arrêt :

1. Exécutez l’applet de commande Get-VirtualDisk et vous assurer que la valeur HealthStatus est sain.
2. Drainer le nœud en exécutant l’applet de commande suivante :

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Placez les disques sur ce nœud en Mode de Maintenance de stockage en exécutant l’applet de commande suivante : 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Exécutez le **Get-PhysicalDisk** applet de commande et vous assurer que la valeur OperationalStatus est en Mode de Maintenance.
5. Exécutez le **Restart-Computer** applet de commande pour redémarrer le nœud.
6. Après le redémarrage du nœud, supprimez les disques sur ce nœud du Mode de Maintenance de stockage en exécutant l’applet de commande suivante :

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Relancer le nœud en exécutant l’applet de commande suivante :

   ```powershell
   Resume-ClusterNode
   ```
8. Vérifiez l’état des travaux de resynchronisation en exécutant l’applet de commande suivante :

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>La désactivation de vidages en direct
Pour atténuer l’effet de la génération de débogage en direct sur les systèmes qui ont beaucoup de mémoire et sont en situation de stress, en outre voulez-vous désactiver la génération de vidage en direct. Trois options sont fournies ci-dessous.

>[!Caution]
>Cette procédure peut empêcher la collecte des informations de diagnostic devant le Support Microsoft peut analyser ce problème. Un agent de Support peut avoir à vous inviter à nouveau activer la génération du vidage en direct basée sur des scénarios de résolution des problèmes spécifiques.

Il existe deux méthodes pour désactiver les images en direct, comme décrit ci-dessous.

#### <a name="method-1-recommended-in-this-scenario"></a>Méthode 1 (recommandé dans ce scénario)
Pour désactiver complètement tous les dumps, y compris les dumps dynamiques au niveau du système, procédez comme suit :

1. Créez la clé de Registre suivante : HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Sous la nouvelle **ForceDumpsDisabled** clé, créez une propriété REG_DWORD comme GuardedHost et puis définissez sa valeur sur 0 x 10000000.
3. Appliquer la nouvelle clé de Registre pour chaque nœud du cluster.

>[!NOTE]
>Vous devez redémarrer l’ordinateur pour que la modification %nregistry entrent en vigueur.

Une fois cette clé de Registre est définie, en direct vidage création échoue et génère une erreur « STATUS_NOT_SUPPORTED » lorsque.

#### <a name="method-2"></a>Méthode 2
Par défaut, le rapport d’erreurs Windows permettra LiveDump qu’une seule par type de rapport par 7 jours et LiveDump uniquement 1 par machine par 5 jours. Vous pouvez modifier qui en définissant les clés de Registre suivantes afin d’autoriser uniquement un LiveDump sur l’ordinateur indéfiniment.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Remarque* vous devez redémarrer l’ordinateur pour que la modification prenne effet.

### <a name="method-3"></a>Méthode 3
Pour désactiver la génération de cluster d’images en direct (par exemple, lorsqu’un événement de 5120 est ouvert une session), exécutez l’applet de commande suivante :

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Cette applet de commande a un effet immédiat sur tous les nœuds de cluster sans un redémarrage de l’ordinateur.

## <a name="slow-io-performance"></a>Ralentissement des performances d’e/s

Si vous rencontrez un ralentissement des performances d’e/s, vérifiez si le cache est activé dans votre configuration d’espaces de stockage Direct. 

Il existe deux manières de vérifier : 


1. À l’aide du journal de cluster. Ouvrez le journal de cluster dans l’éditeur de texte de choix et recherchez « [=== SBL disques ===]. » Il s’agit d’une liste du disque sur le nœud que le journal a été généré sur. 

     Cache activé disques exemple : Notez ici que l’état est CacheDiskStateInitializedAndBound et il est un GUID présent ici. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache n’est ne pas activé : Ici, nous pouvons voir aucun GUID présent et l’état est CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Cache n’est ne pas activé : Lorsque tous les disques sont de la même casse de type n’est pas activée par défaut. Ici, nous pouvons voir aucun GUID présent et l’état est CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. À l’aide de Get-PhysicalDisk.xml à partir de la SDDCDiagnosticInfo
    1. Ouvrez le fichier XML en utilisant « $d = Import-Clixml GetPhysicalDisk.XML »
    2. Exécutez « stockage ipmo »
    3. Exécutez « $d ». Notez que l’utilisation est la sélection automatique, pas Journal vous obtiendrez une sortie comme suit : 

   |FriendlyName|  SerialNumber| Type de média| CanPool| OperationalStatus| HealthStatus| Utilisation| Size|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   OK|                Healthy|      TO 1,82 sélection automatique|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    OK|                Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  OK|                Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| OK|    Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|OK|       Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| OK|      Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| OK|      Healthy|Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| OK|  Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| OK| Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| OK|Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| OK| Healthy| Sélection automatique |1,82 TO|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>Comment détruire un cluster existant, vous pouvez donc utiliser les mêmes disques à nouveau

Dans un cluster d’espaces de stockage Direct, une fois vous les désactiver espaces de stockage Direct et utilisez la procédure de nettoyage décrite dans [nettoyer les lecteurs](deploy-storage-spaces-direct.md#step-31-clean-drives), le pool de stockage en cluster se trouve toujours dans un état hors connexion, et le Service d’intégrité est supprimé de Grappe.

L’étape suivante consiste à supprimer le pool de stockage fantôme : 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

Maintenant, si vous exécutez **Get-PhysicalDisk** sur les nœuds, vous pouvez voir tous les disques qui étaient dans le pool. Par exemple, dans un laboratoire avec un cluster à 4 nœuds avec 4 disques SAS, 100 Go chacune présenté à chaque nœud. Dans ce cas, une fois que l’espace de stockage Direct est désactivé, ce qui supprime le SBL (couche de Bus de stockage), mais laisse le filtre, si vous exécutez **Get-PhysicalDisk**, il doit signaler 4 disques à l’exclusion du disque du système d’exploitation local. Au lieu de cela il signalé 16 à la place. Ceci est le même pour tous les nœuds du cluster. Lorsque vous exécutez un **Get-Disk** commande, vous verrez les disques connectés localement numérotées en tant que 0, 1, 2 et ainsi de suite, comme illustré dans cet exemple de sortie :

|Numéro| Nom convivial| Numéro de série|HealthStatus|OperationalStatus|Taille totale| Style de partition|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||Healthy | La licence|  127 Go| GPT|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
|1|Msft Virtu...||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
|2|Msft Virtu...||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
|4|Msft Virtu...||Healthy| Hors connexion| 100 GO| RAW|
|3|Msft Virtu...||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|
||Msft Virtu... ||Healthy| Hors connexion| 100 GO| RAW|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>Message d’erreur sur « type de média non pris en charge » lorsque vous créez un cluster d’espaces de stockage Direct à l’aide d’Enable-ClusterS2D  

Vous pouvez rencontrer des erreurs similaires à ceci lorsque vous exécutez le **Enable-ClusterS2D** applet de commande :

![Message d’erreur scénario 6](media/troubleshooting/scenario-error-message.png)

Pour résoudre ce problème, vérifiez que l’adaptateur HBA est configuré en mode HBA. Aucun adaptateur de bus hôte ne doit être configuré en mode RAID.  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>Enable-ClusterStorageSpacesDirect se bloque au « En attente jusqu'à ce que SBL disques sont signalées » ou au 27 %

Vous voyez les informations suivantes dans le rapport de validation :

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


Le problème est avec la carte d’expander HPE SAS qui se trouve entre les disques et la carte HBA. L’expandeur SAS crée un ID en double entre le premier lecteur connecté à l’icône de développement et de l’icône de développement.  Ce problème a été résolu dans [HPE Smart tableau contrôleurs SAS Expander microprogramme : 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 série a un NGUID non uniques
Vous pouvez voir un problème où un appareil de série Intel SSD DC P4600 semble être reporting similaire 16 octets NGUID pour plusieurs espaces de noms tels que 0100000001000000E4D25C000014E214 ou 0100000001000000E4D25C0000EEE214 dans l’exemple ci-dessous.


|               uniqueid               | deviceid | Type de média | BusType |               SerialNumber (Numéro_série)               |      size      | canpool | FriendlyName | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| eui.0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |

Pour résoudre ce problème, vous devez mettre à jour le microprogramme sur les lecteurs Intel vers la dernière version.  Version du microprogramme QDV101B1 à partir de mai 2018 est connue pour résoudre ce problème.

Le [version de mai 2018 de l’outil de centre de données SSD Intel](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) inclut un microprogramme mise à jour, QDV101B1, pour la série Intel SSD DC P4600.


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>Physique disque « Sain » et l’état opérationnel est « Suppression de Pool de » 

Dans un cluster d’espaces de stockage Windows Server 2016 Direct, vous verrez la HealthStatus de disques physiques d’un ou plusieurs en tant que « Intègre », tandis que le OperationalStatus est « (suppression de Pool, OK). » 

« Suppression de Pool » est une intention définie lorsque **Remove-PhysicalDisk** est appelé, mais stockées dans le contrôle d’intégrité pour maintenir l’état et autoriser la récupération si l’opération de suppression échoue. Vous pouvez modifier manuellement le OperationalStatus sain avec l’une des méthodes suivantes :

- Supprimer le disque physique dans le pool, puis rajoutez-le.
- Exécutez le [Clear-PhysicalDiskHealthData.ps1 script](https://go.microsoft.com/fwlink/?linkid=2034205) pour effacer l’intention. (Disponible en téléchargement en tant qu’un. Fichier TXT. Vous devrez l’enregistrer en tant qu’un. Fichier ps1 avant de pouvoir l’exécuter.)

Voici quelques exemples montrant comment exécuter le script :

- Utilisez le **SerialNumber** paramètre pour spécifier le disque, vous devez définir sur l’état intègre. Vous pouvez obtenir le numéro de série à partir de **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**. (Nous les utilisons 0 s pour le numéro de série ci-dessous.)

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Utilisez le **UniqueId** paramètre pour spécifier le disque (à partir de **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>Copie de fichiers est lente

Vous pouvez vu un problème à l’aide de l’Explorateur de fichiers pour copier un disque dur virtuel volumineux vers le disque virtuel : la copie de fichier est plue longue que prévu.

L’Explorateur de fichiers, Robocopy ou Xcopy pour copier un disque dur virtuel volumineux vers le disque virtuel n’est pas une méthode recommandée car cela entraîne le moindre que les performances attendues. Le processus de copie ne traverse pas la pile d’espaces de stockage Direct, qui se trouve plus sur la pile de stockage et à la place agit comme un processus de copie locale.

Si vous souhaitez tester les performances d’espaces de stockage Direct, nous vous recommandons d’utiliser VMFleet et Diskspd à charge et tests de contrainte sur les serveurs pour obtenir une ligne de base et de définir les attentes des performances espaces de stockage Direct.

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>Attendu des événements que vous verriez sur le reste des nœuds pendant le redémarrage d’un nœud. 

Vous pouvez sans risque ignorer ces événements : 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Si vous exécutez des machines virtuelles Azure, vous pouvez ignorer cet événement :

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>Ralentissement des performances ou « Perte de Communication, » « Erreur d’e/s », « Détachée » ou « No redondance » des erreurs pour les déploiements qui utilisent des périphériques Intel P3x00 NVMe

Nous avons identifié un problème critique qui affecte certains utilisateurs d’espaces de stockage Direct qui sont à l’aide de matériel en fonction de la famille Intel P3x00 de NVM Express (NVMe) des appareils avec les versions de microprogramme avant « Mise à jour 8 ». 

>[!NOTE]
> Les fabricants OEM individuels peuvent avoir des périphériques qui sont basées sur la famille Intel P3x00 de périphériques NVMe avec des chaînes de version du microprogramme unique. Pour plus d’informations de la dernière version du microprogramme, contactez votre fabricant OEM.

Si vous utilisez le matériel dans votre déploiement en fonction de la famille Intel P3x00 de périphériques NVMe, nous vous recommandons d’appliquer immédiatement le dernier microprogramme disponible (au moins 8 de version de Maintenance). Cela [article du Support Microsoft](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fournit des informations supplémentaires sur ce problème. 
