---
title: Résolution des problèmes Direct d’espaces de stockage
description: Découvrez comment résoudre les problèmes de votre déploiement d’espaces de stockage Direct.
keywords: Espaces de stockage
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: c7c9573732ff1cf5a998588b1aec81915c227ee2
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256932"
---
# Résoudre les problèmes d’espaces de stockage Direct

Utilisez les informations suivantes pour résoudre les problèmes de votre déploiement d’espaces de stockage Direct.

En règle générale, commencez par les étapes suivantes:

1. Vérifiez que la marque/modèle de disque SSD est certifié pour Windows Server 2016 à l’aide du catalogue Windows Server. Avec le fournisseur, vérifiez que les lecteurs sont pris en charge pour les espaces de stockage Direct.
2. Vérifiez que le stockage pour les lecteurs défectueux. Utiliser des logiciels de gestion de stockage pour vérifier l’état des disques. Si un des lecteurs sont défectueuses, fonctionnent avec votre fournisseur. 
3. Mettre à jour de stockage et le microprogramme de lecteur si nécessaire.
   Assurez-vous que les dernières mises à jour Windows sont installées sur tous les nœuds. Vous pouvez obtenir les dernières mises à jour pour Windows Server 2016 à partir de [https://aka.ms/update2016](https://aka.ms/update2016).
4. Mettre à jour du microprogramme et des pilotes de carte réseau.
5. Exécuter la validation du cluster et passez en revue la section de l’espace de stockage Direct, vérifiez les disques qui seront utilisés pour le cache sont signalés correctement et sans erreur.

Si vous rencontrez toujours des problèmes, passez en revue les scénarios ci-dessous.

## Ressources de disque virtuel sont en état non redondance
Les nœuds d’un système d’espaces de stockage Direct redémarrer de façon inattendue en raison d’une défaillance de se bloquer ou hors tension. Ensuite, un ou plusieurs des disques virtuels ne peuvent pas être en ligne et vous voir la description «pas suffisamment d’informations redondance.»
    
|friendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Taille| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disquette n° 4| Mirror| OK|  Healthy| Vrai|  10 TO|  Nœud-01.conto …|
|Disquette n° 3         |Mirror                 |OK                          |Healthy       |Vrai            |10 TO | Nœud-01.conto …|
|2         |Mirror                 |Pas de redondance               |Unhealthy     |Vrai            |10 TO | Nœud-01.conto …|
|Disque 1         |Mirror                 |{Aucune redondance, InService}  |Unhealthy     |Vrai            |10 TO | Nœud-01.conto …| 

En outre, après une tentative de remettre le disque virtuel en ligne, les informations suivantes sont enregistrées dans le journal de Cluster (DiskRecoveryAction).  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

L' **État opérationnel de redondance aucun** peut se produire si un disque a échoué ou si le système ne peut pas accéder aux données sur le disque virtuel. Ce problème peut se produire si un redémarrage se produit sur un nœud durant la maintenance sur les nœuds.
    
Pour résoudre ce problème, procédez comme suit:

1. Supprimez les disques virtuels affectés de volume partagé de cluster. Cela permet de les placer dans le groupe «Stockage disponible» dans le cluster et de démarrer affichés sous la forme d’un type de ressource de «Disque physique».

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. Sur le nœud propriétaire du groupe de stockage disponible, exécutez la commande suivante sur chaque disque qui est dans un état non redondance. Pour identifier le nœud le groupe «Stockage disponible» se trouve sur vous pouvez exécuter la commande suivante.

   ```powershell
   Get-ClusterGroup
   ```
3. Définir l’action de récupération de disque, puis démarrez l’ou les disques.
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. Une réparation doit démarrer automatiquement. Attendez que la réparation terminer. Il peut accéder à un état suspendu et recommencer. Pour surveiller la progression: 
    - Exécutez **Get-StorageJob** pour surveiller l’état de la réparation et pour déterminer lorsqu’il est terminé.
    - Exécutez **Get-VirtualDisk** et vérifiez que l’espace retourne un HealthStatus sain.
5. Après la réparation se termine et les disques virtuels sont sain, rétablir les paramètres de disque virtuel.

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. Tirez l’ou les disques en mode hors connexion et puis en ligne à nouveau pour que la DiskRecoveryAction prennent effet:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. Ajoutez les disques virtuels affectés revenir au format CSV.

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction** est un commutateur de substitution qui permet d’attacher le volume de l’espace en mode lecture-écriture sans toutes les vérifications. La propriété vous permet de vous permettent d’effectuer des tests de diagnostic dans pourquoi un volume ne sont pas mis en ligne. Il est très similaire au Mode de Maintenance, mais vous pouvez l’appeler sur une ressource en état d’échec. Elle vous permet également d’accéder aux données, qui peuvent être utiles dans les situations telles que «No redondance», où vous pouvez obtenir l’accès à toutes les données, vous pouvez et copiez-le. La propriété DiskRecoveryAction a été ajoutée dans la février 22 2018, mise à jour, 4077525 Ko.
    
    
## État détaché dans un cluster 

Lorsque vous exécutez l’applet de commande **Get-VirtualDisk** , le OperationalStatus pour un ou plusieurs disques virtuels espaces de stockage Direct est détachée. Toutefois, le HealthStatus signalées par l’applet de commande **Get-PhysicalDisk** indique que tous les disques physiques sont dans un état intègre.

Voici un exemple de la sortie de l’applet de commande **Get-VirtualDisk** .

|friendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Taille|   PSComputerName|
|-|-|-|-|-|-|-|
|Disquette n° 4|         Mirror|                 OK|                  Healthy|       Vrai|            10 TO|  Nœud-01.conto …|
|Disquette n° 3|         Mirror|                 OK|                  Healthy|       Vrai|            10 TO|  Nœud-01.conto …|
|2|         Mirror|                 Détaché|            Inconnu|       Vrai|            10 TO|  Nœud-01.conto …|
|Disque 1|         Mirror|                 Détaché|            Inconnu|       Vrai|            10 TO|  Nœud-01.conto …| 


En outre, les événements suivants peuvent être enregistrés sur les nœuds:

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

L' **État opérationnel détaché** peut se produire si la région modifiée de suivi (DRT) journal est plein. Les espaces de stockage utilise région modifiée de suivi (DRT) pour les espaces de mise en miroir pour s’assurer que lorsqu’une coupure se produit, les mises à jour en cours d’exécution aux métadonnées sont enregistrés pour vous assurer que l’espace de stockage peut rétablir ou annuler les opérations pour ramener l’espace de stockage dans un flexible et état cohérent lorsque l’alimentation est restaurée et le système fonctionne à nouveau. Si le journal DRT est saturée, le disque virtuel ne peut pas être mis en ligne jusqu'à ce que les métadonnées DRT sont synchronisée et vidée. Ce processus nécessite l’exécution d’une analyse complète, ce processus peut prendre plusieurs heures pour terminer.
    
Pour résoudre ce problème, procédez comme suit:
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
   Cette tâche doit être lancée sur tous les nœuds sur lesquels le volume détaché est en ligne. Une réparation doit démarrer automatiquement. Attendez que la réparation terminer. Il peut accéder à un état suspendu et recommencer. Pour surveiller la progression: 
   - Exécutez **Get-StorageJob** pour surveiller l’état de la réparation et pour déterminer lorsqu’il est terminé.
   -  Exécutez **Get-VirtualDisk** et vérifiez que l’espace renvoie un HealthStatus sain.
    - La «l’intégrité analyse pour se bloque pas restauration des données» est une tâche qui ne s’affiche en tant qu’un travail de stockage, et il n’existe aucun indicateur de progression. Si la tâche s’affiche en cours d’exécution, il est en cours d’exécution. Lorsqu’il a terminé, il affiche terminée.
    
       En outre, vous pouvez afficher l’état d’une tâche planifiée en cours d’exécution à l’aide de l’applet de commande suivante: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. Dès que le «données d’intégrité du analyse pour se bloquer restauration» est terminée, la fin de réparation et les disques virtuels sont sain, rétablir les paramètres de disque virtuel. 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. Ajoutez les disques virtuels affectés revenir au format CSV.    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**DiskRunChkdsk valeur 7** est utilisé pour joindre le volume de l’espace et la partition en mode lecture seule. Ainsi, pour détecter automatiquement les espaces et autoréparation en déclenchant une réparation. Opérations de réparation seront exécute automatiquement une fois montés. Il vous permet également d’accéder aux données, qui peuvent être utiles obtenir un accès à toutes les données vous pouvez copier. Pour certaines conditions d’erreur, par exemple, un journal DRT complète, vous devez exécuter l’analyse de l’intégrité des données pour la tâche planifiée de récupération.
    
**Analyse des données d’intégrité pour la tâche de récupération** est utilisé pour synchroniser et effacer un journal de suivi (DRT) région complète modifiée. Cette tâche peut prendre plusieurs heures. La «l’intégrité analyse pour se bloque pas restauration des données» est une tâche qui ne s’affiche en tant qu’un travail de stockage, et il n’existe aucun indicateur de progression. Si la tâche s’affiche en cours d’exécution, il est en cours d’exécution. Lorsqu’il a terminé, il apparaît comme terminé. Si vous annulez la tâche ou que vous redémarrez un nœud pendant l’exécution de cette tâche, la tâche devez recommencer depuis le début.

Pour plus d’informations, voir [intégrité dépannage des espaces de stockage Direct et états opérationnels](storage-spaces-states.md).
    
## Événement 5120 avec STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> Pour réduire le risque de ces symptômes lors de l’application de la mise à jour avec le correctif, il est recommandé d’utiliser la procédure de Mode de gestion de stockage ci-dessous pour installer [18 octobre 2018, la mise à jour cumulative pour Windows Server 2016](https://support.microsoft.com/help/4462928) ou une version ultérieure Lorsque les nœuds actuellement ont installé une mise à jour cumulative de Windows Server 2016 qui a été publiée à partir de [8 mai 2018](https://support.microsoft.com/help/4103723) à [9 octobre 2018](https://support.microsoft.com/help/KB4462917).

Vous pouvez obtenir l’événement 5120 avec STATUS_IO_TIMEOUT c00000b5 après le redémarrage d’un nœud sur Windows Server 2016 avec la mise à jour cumulative qui ont été publiées à partir de [peut 8 Ko 2018 4103723](https://support.microsoft.com/help/4103723) à [9 octobre 2018 Ko 4462917](https://support.microsoft.com/help/4462917) installé.

Lorsque vous redémarrez le nœud, 5120 d’événement est consigné dans le journal des événements système et inclut un des codes d’erreur suivants:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

Lorsqu’un 5120 d’événement est connecté, un vidage direct est généré pour collecter des informations de débogage qui peuvent provoquer des problèmes supplémentaires, ou avoir une incidence sur les performances. Générer le vidage direct crée une courte pause pour activer la prise d’un instantané de la mémoire d’écrire le fichier de vidage. Systèmes qui dispose d’un grand nombre de mémoire et sous tension peuvent entraîner des nœuds disparaissent l’appartenance au cluster et également provoquer le 1135 événement suivant être connectés.

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Une modification a été introduite dans la mise à jour cumulative 8 mai 2018, pour ajouter gère résilient SMB pour les sessions de réseau SMB intra-cluster espaces de stockage Direct. Cela a été fait pour améliorer la résilience pour réseau temporaire et améliorer la façon dont RoCE gère encombrement du réseau.

Ces améliorations ont augmenté également par inadvertance les délais d’attente lorsque connexions SMB tentent de se reconnecter et attend de délai d’expiration lorsqu’un nœud a redémarré. Ces problèmes peuvent affecter un système qui est sous tension. Au cours des arrêts, met en pause des e/s de jusqu'à 60 secondes ont été observés également pendant que le système en attente pour les connexions à un délai d’expiration.

Pour résoudre ce problème, installez le [18 octobre 2018, la mise à jour cumulative pour Windows Server 2016](https://support.microsoft.com/help/4462928) ou une version ultérieure.

*Remarque* Cette mise à jour s’aligne les délais d’attente de volume partagé de cluster avec les délais de connexion SMB pour résoudre ce problème. Il n’implémente pas les modifications pour désactiver la génération de vidage direct mentionnée dans la section de la solution de contournement.
    
### Flux de processus d’arrêt:

1. Exécutez l’applet de commande Get-VirtualDisk et vous assurer que la valeur HealthStatus est sain.
2. Décharger le nœud en exécutant l’applet de commande suivante:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. Placez les disques sur ce nœud en Mode de gestion de stockage en exécutant l’applet de commande suivante: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. Exécutez l’applet de commande **Get-PhysicalDisk** et vous assurer que la valeur OperationalStatus est en Mode de Maintenance.
5. Exécutez l’applet de commande **Restart-Computer** pour redémarrer le nœud.
6. Après le redémarrage de nœud, supprimez les disques sur ce nœud à partir du Mode de gestion de stockage en exécutant l’applet de commande suivante:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. Reprendre le nœud en exécutant l’applet de commande suivante:

   ```powershell
   Resume-ClusterNode
   ```
8. Vérifier l’état de la resynchronisation en exécutant l’applet de commande suivante:

   ```powershell
   Get-StorageJob
   ```

### La désactivation de vidages dynamiques
Pour limiter l’effet de la génération de vidage direct sur les systèmes qui dispose d’un grand nombre de mémoire et sous tension, vous voudrez en outre désactiver la génération de vidage dynamiques. Trois options sont fournies ci-dessous.

>[!Caution]
>Cette procédure peut empêcher la collecte d’informations de diagnostics qui Support Microsoft peuvent nécessiter d’examiner ce problème. Un agent du Support devra peut-être vous invitera à réactiver génération de vidage dynamiques basée sur des scénarios de résolution des problèmes spécifiques.

Il existe deux méthodes pour désactiver les vidages dynamiques, comme décrit ci-dessous.

#### Méthode 1 (recommandé dans ce scénario)
Pour désactiver complètement toutes les vidages, y compris les vidages dynamiques à l’échelle du système, procédez comme suit:

1. Créez la clé de Registre suivante: HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. Sous la nouvelle clé **ForceDumpsDisabled** , créez une propriété REG_DWORD comme GuardedHost et puis définissez sa valeur sur 0 x 10000000.
3. Appliquez la nouvelle clé de Registre sur chaque nœud du cluster.

>[!NOTE]
>Vous devez redémarrer l’ordinateur pour que la modification nregistry prennent effet.

Une fois que cette clé de Registre est défini, dynamiques création d’un vidage échoue et générer une erreur «STATUS_NOT_SUPPORTED» lorsque.

#### Méthode 2
Par défaut, rapport d’erreurs Windows permettra LiveDump qu’un seul par type de rapport par 7 jours et 1 seul LiveDump par ordinateur par des 5 derniers jours. Vous pouvez modifier qui en définissant les clés de Registre suivantes pour autoriser uniquement un LiveDump sur l’ordinateur indéfiniment.
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*Remarque* Vous devez redémarrer l’ordinateur pour que les modifications prennent effet.

### Méthode 3
Pour désactiver la génération de cluster de vidages dynamiques (par exemple, lorsqu’un événement de 5120 est connecté), exécutez l’applet de commande suivante:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
Cette applet de commande a une incidence immédiate sur tous les nœuds de cluster sans redémarrage de l’ordinateur.
    
## Baisse des performances d’e/s

Si vous rencontrez une baisse des performances d’e/s, vérifiez si le cache est activé dans votre configuration d’espaces de stockage Direct. 

Il existe deux façons de vérifier: 
     
 
1. L’utilisation du journal de cluster. Ouvrez le journal de cluster dans un éditeur de texte de choix et recherchez «[=== disques SBL ===].» Il s’agit d’une liste du disque sur le nœud sur que du journal a été généré. 

     Exemple de disques cache activé: Notez ici que l’état est CacheDiskStateInitializedAndBound et qu’il s’agit d’un GUID présent ici. 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Le cache n’est pas activé: Ici, nous pouvons voir il n’existe aucun GUID présent et l’état est CacheDiskStateNonHybrid. 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    Le cache n’est pas activé: Lorsque tous les disques sont du même type cas n’est pas activé par défaut. Ici, nous pouvons voir il n’existe aucun GUID présent et l’état est CacheDiskStateIneligibleDataPartition. 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. À l’aide de Get-PhysicalDisk.xml à partir de la SDDCDiagnosticInfo
    1. Ouvrez le fichier XML à l’aide «$d = Import-Clixml GetPhysicalDisk.XML»
    2. Exécutez le stockage «bureau»
    3. Exécutez «$d». Notez que l’utilisation est sélection automatique, pas feuille vous verrez sortie comme suit: 

   |friendlyName|  SerialNumber| Type de média| CanPool| OperationalStatus| HealthStatus| Utilisation| Taille|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| Faux|   OK|                Healthy|      Sélection automatique 1,82 to|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  Faux|    OK|                Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  Faux|  OK|                Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| Faux| OK|    Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| Faux|OK|       Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| Faux| OK|      Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| Faux| OK|      Healthy|Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| Faux| OK|  Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| Faux| OK| Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| Faux| OK|Healthy| Sélection automatique| 1,82 TO|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| Faux| OK| Healthy| Sélection automatique |1,82 TO|
    
## Comment détruire un cluster existant, afin que vous pouvez réutiliser les mêmes disques

Dans un cluster d’espaces de stockage Direct, une fois que vous désactivez les espaces de stockage Direct et que vous utilisez le processus de nettoyage décrit dans le [nettoyage des lecteurs](deploy-storage-spaces-direct.md#step-31-clean-drives), le pool de stockage en cluster qu’il reste encore dans un état hors connexion, et le Service d’intégrité est supprimé de cluster.

L’étape suivante consiste à supprimer le pool de stockage fantôme: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

À présent, si vous exécutez **Get-PhysicalDisk** sur les nœuds, vous verrez tous les disques qui étaient dans le pool. Par exemple, dans un laboratoire avec un cluster de 4 nœuds avec 4 disques SAS, 100 Go chaque présentée à chaque nœud. Dans ce cas, une fois que l’espace de stockage Direct est désactivé, ce qui supprime la SBL (couche de Bus de stockage), mais laisse le filtre, si vous exécutez **Get-PhysicalDisk**, elle doit signaler 4 disques en excluant le disque du système d’exploitation local. Au lieu de cela il signalé 16 à la place. Il s’agit de la même pour tous les nœuds du cluster. Lorsque vous exécutez une commande **Get-Disk** , vous verrez les disques connectés localement numérotés en tant que 0, 1, 2, et ainsi de suite, comme illustré dans cet exemple de sortie:

|Nombre| Nom convivial| Numéro de série|HealthStatus|OperationalStatus|Taille totale| Style de partition|
|-|-|-|-|-|-|-|-|
|0|Manière unique msft …  ||Healthy | Online|  127 GO| GPT|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
|1|Manière unique msft …||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
|2|Manière unique msft …||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
|4|Manière unique msft …||Healthy| Hors connexion| 100 GO| Cru|
|3|Manière unique msft …||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|
||Manière unique msft … ||Healthy| Hors connexion| 100 GO| Cru|

    
## Message d’erreur indiquant que «type de média non pris en charge» lorsque vous créez un cluster d’espaces de stockage Direct à l’aide de Enable-ClusterS2D  

Vous pouvez voir des erreurs semblables à celle-ci lorsque vous exécutez l’applet de commande **Enable-ClusterS2D** :

![Message d’erreur scénario 6](media/troubleshooting/scenario-error-message.png)

Pour résoudre ce problème, vérifiez que la carte HBA est configurée en mode HBA. Aucun adaptateur de bus hôte ne doit être configuré en mode RAID.  
    
## Enable-ClusterStorageSpacesDirect se bloque au «Attendre SBL disques sont exposées» ou 27 %

Vous verrez les informations suivantes dans le rapport de validation:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
Le problème est avec la carte de développement HPE SAS qui se trouve entre les disques et la carte HBA. Le système d’extension SAS crée un ID en double entre le premier lecteur connecté à l’agrandissement et le développeur proprement dit.  Cela a été résolu dans [HPE actives tableau contrôleurs SAS agrandissement microprogramme: 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).
    
## Série Intel SSD DC P4600 a un NGUID non unique
Vous constaterez peut-être un problème dans lequel un périphérique de la gamme Intel SSD DC P4600 semble être reporting similaires de 16 octets NGUID pour plusieurs espaces de noms tels que 0100000001000000E4D25C000014E214 ou 0100000001000000E4D25C0000EEE214 dans l’exemple ci-dessous.

|ID unique| ID d’appareil |Type de média| BusType| numéro de série| size|canpool| FriendlyName| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| HDD| SAS| 7PKR197G|                  10000831348736 |Faux|HGST| HUH721010AL4200| OK|
|EUI.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214.|1600321314816|Vrai| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214.|  1600321314816|    Vrai| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    Vrai| INTEL| SSDPE2KE016T7|  OK|
|EUI.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214.|  1600321314816|    Vrai| INTEL| SSDPE2KE016T7|  OK|

Pour résoudre ce problème, mettez à jour le microprogramme sur les lecteurs Intel vers la dernière version.  Version du microprogramme QDV101B1 à partir du mois de mai 2018 est connu pour résoudre ce problème.

Le [mois de mai 2018 libérer de l’outil de centre de données SSD Intel](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf) inclut un microprogramme mise à jour, QDV101B1, de la série Intel SSD DC P4600.

    
## Physiques disque «Sain» et l’état opérationnel est «Suppression de Pool de» 

Dans un cluster d’espaces de stockage Windows Server 2016 Direct, vous pouvez voir les HealthStatus pour les disques physiques une ou plusieurs en tant que «Saines», tandis que le OperationalStatus est «(suppression pool, OK).» 

«Supprimer de Pool» est une intention définie lorsque **Remove-PhysicalDisk** est appelée mais stockés dans la santé pour conserver l’état et permettre la récupération en cas d’échec de l’opération de suppression. Vous pouvez modifier manuellement le OperationalStatus sain avec l’une des méthodes suivantes:

- Supprimer le disque physique du pool et puis ajoutez-le.
- Exécutez le [script Clear-PhysicalDiskHealthData.ps1](https://go.microsoft.com/fwlink/?linkid=2034205) pour effacer l’intention. (Disponible en téléchargement en tant qu’un. Fichier TXT. Vous devez enregistrer en tant qu’un. Fichier ps1 avant que vous pouvez l’exécuter.)

Voici quelques exemples illustrant comment exécuter le script:

- Utilisez le paramètre de **numéro de série** pour spécifier le disque, que vous devez définir sain. Vous pouvez obtenir le numéro de série à partir de **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**. (Nous utilisons simplement 0 pour le numéro de série ci-dessous.)
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- Utilisez le paramètre de **l’ID unique** pour spécifier le disque (à partir de **WMI MSFT_PhysicalDisk** ou **Get-PhysicalDIsk**).

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## Copie des fichiers est lente

Vous pouvez vu un problème dans l’Explorateur de fichiers à copier un disque dur virtuel de grande taille sur le disque virtuel - la copie du fichier est plue longue que prévu.

L’Explorateur de fichiers, Robocopy ou Xcopy pour copier un disque dur virtuel de grande taille sur le disque virtuel n’est pas une méthode recommandée pour que cela se traduit par plus lente que les performances attendues. Le processus de copie ne va pas par le biais de la pile d’espaces de stockage Direct, ce qui se trouve en bas de la pile de stockage et à la place se comporte comme un processus de copie locale.

Si vous souhaitez tester les performances d’espaces de stockage Direct, nous vous recommandons d’utiliser VMFleet et Diskspd pour charger et de test de contrainte les serveurs pour obtenir une ligne de base et définissez les attentes des performances espaces de stockage Direct.

## Prévu des événements que vous verriez dans le reste des nœuds lors du redémarrage d’un nœud. 

Il est sûr d’ignorer ces événements: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

Si vous exécutez des machines virtuelles Azure, vous pouvez ignorer cet événement:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## Baisse des performances ou «Perte de Communication,» «Erreur d’e/s», «Détaché», ou des erreurs «No redondance» pour les déploiements qui utilisent des appareils Intel P3x00 NVMe

Nous avons identifié un problème critique qui a une incidence sur certains utilisateurs espaces de stockage Direct, qui sont à l’aide de matériel en fonction de la famille Intel P3x00 de NVM Express (NVMe) avec les versions du microprogramme avant la «Mise à jour 8». 

>[!NOTE]
> Les fabricants OEM individuels peuvent employer des appareils qui sont basées sur la famille Intel P3x00 de périphériques NVMe avec des chaînes de version du microprogramme unique. Pour plus d’informations de la dernière version du microprogramme, contactez votre fabricant OEM.

Si vous utilisez le matériel dans votre déploiement basé sur la famille Intel P3x00 de périphériques NVMe, nous recommandons d’appliquer immédiatement le microprogramme le plus récent disponible (au moins 8 de publication de Maintenance). Cet [article du support technique Microsoft](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda) fournit des informations supplémentaires sur ce problème. 
