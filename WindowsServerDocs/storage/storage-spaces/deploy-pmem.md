---
title: Comprendre et déployer la mémoire persistante
description: Informations détaillées sur la mémoire persistante et la configuration des espaces de stockage direct dans Windows Server 2019.
keywords: Espaces de stockage direct, mémoire persistante, PMEM, stockage, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 497fa201c500919fc857d25166d37ce87613d0f0
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872006"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprendre et déployer la mémoire persistante

>S’applique à : Windows Server 2019

La mémoire persistante (ou PMem) est un nouveau type de technologie de mémoire qui offre une combinaison unique de grande capacité et persistance abordable. Cette rubrique fournit des informations générales sur PMem et les étapes de déploiement avec Windows Server 2019 avec espaces de stockage direct.

## <a name="background"></a>Présentation

PMem est un type de mémoire DRAM non volatile (NVDIMM) qui présente la vitesse de la DRAM, mais conserve son contenu de mémoire par le biais de cycles de gestion de l’alimentation (le contenu de la mémoire reste même en cas de panne d’alimentation inattendue, arrêt initié par l’utilisateur, panne du système, etc.). Pour cette raison, la reprise à partir de l’endroit où vous vous étiez arrêté est beaucoup plus rapide, car le contenu de votre RAM n’a pas besoin d’être rechargé. Une autre caractéristique unique est que le PMem est adressable en octets, ce qui signifie que vous pouvez également l’utiliser comme stockage (ce qui explique pourquoi vous pouvez entendre le mode de stockage d’un PMem appelé mémoire de classe de stockage).


Pour voir certains de ces avantages, examinons la démonstration de Microsoft en2018 :

[![Démonstration PMEM de Microsoft d’allumage 2018](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Tout système de stockage qui fournit la tolérance de panne effectue nécessairement des copies distribuées des écritures, qui doivent traverser le réseau et engendrer une amplification du backend en écriture. Pour cette raison, les numéros d’évaluation des IOPS les plus importants sont généralement obtenus avec des lectures uniquement, surtout si le système de stockage a des optimisations de sens commun à lire à partir de la copie locale chaque fois que cela est possible, ce qui espaces de stockage direct.

**Avec des lectures de 100%, le cluster fournit 13 798 674 IOPS.**

![capture d’écran enregistrement IOPS 13.7 m](media/deploy-pmem/iops-record.png)

Si vous regardez la vidéo de près, vous remarquerez que la suppression de la mâchoire est encore plus importante : même au-delà de 13,7 M e/s par seconde, le système de fichiers de Windows signale une latence inférieure à 40 μs ! (Il s’agit du symbole pour les microsecondes, du 1 au millionième de seconde.) Il s’agit d’un ordre de grandeur plus rapide que celui des fournisseurs habituels de tous les fournisseurs de Flash.

Ensemble, espaces de stockage direct dans Windows Server 2019 et Intel® Optane™ la mémoire persistante DC offre des performances exceptionnelles. Ce point de référence HCI de plus en plus de 13.7 M IOPS, avec une latence prévisible et extrêmement faible, est plus que doubler notre premier point de référence du secteur de 6,7 millions d’e/s par seconde. En outre, nous avions besoin de 12 nœuds de serveur, 25% moins de deux ans auparavant.

![Gains d’e/s par seconde](media/deploy-pmem/iops-gains.png)

Le matériel utilisé était un cluster à 12 serveurs utilisant des volumes ReFS de mise en miroir triple et des volumes ReFS délimités, **12** x Intel® S2600WFT, **384 Gio** de mémoire, 2 x 28-cœurs "CASCADELAKE", **1,5 to** Intel® OPTANE™ la mémoire persistante DC comme cache, **32 to** NVMe ( 4 x 8 to Intel® DC P4510) en tant que capacité, **2** x Mellanox ConnectX-4 25 Gbits/s

Le tableau ci-dessous contient les numéros de performances complets : 

| TPC                   | Performances         |
|-----------------------------|---------------------|
| Lecture aléatoire de 4 Ko 100%         | 13,8 millions e/s par seconde   |
| 4 Ko 90/10% de lecture/écriture aléatoire | 9 450 000 e/s par seconde   |
| 2 Mo de lecture séquentielle         | Débit de 549 Go/s |

### <a name="supported-hardware"></a>Matériel pris en charge

Le tableau ci-dessous montre le matériel de mémoire persistante pris en charge pour Windows Server 2019 et Windows Server 2016. Notez qu’Intel Optane prend spécifiquement en charge le mode mémoire et le mode application-direct. Windows Server 2019 prend en charge les opérations en mode mixte.

| Technologie de mémoire persistante                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** en mode d’application directe                                       | Pris en charge                | Pris en charge                |
| **Mémoire persistante Intel Optane™ DC** en mode application-direct             | Non pris en charge            | Pris en charge                |
| **Intel Optane™ DC persistent Memory** en mode de mémoire à deux niveaux (2LM) | Non pris en charge            | Pris en charge                |

Voyons maintenant comment configurer la mémoire persistante.

## <a name="interleave-sets"></a>Ensembles entrelacés

### <a name="understanding-interleave-sets"></a>Fonctionnement des ensembles entrelacés

Souvenez-vous que le NVDIMM-N réside dans un emplacement DIMM standard (mémoire), en plaçant les données plus près du processeur (ce qui réduit la latence et l’extraction de meilleures performances). Pour ce faire, un ensemble entrelacé est quand deux ou plusieurs NVDIMMs créent un entrelacement N-Way défini pour fournir des opérations de lecture/écriture de répartition pour un débit accru. Les configurations les plus courantes sont l’entrelacement à 2 ou 4 voies.

Les jeux entrelacés peuvent souvent être créés dans le BIOS d’une plateforme pour que plusieurs périphériques de mémoire persistants apparaissent comme un seul disque logique sur Windows Server. Chaque disque logique de mémoire persistante contient un ensemble entrelacé d’appareils physiques en exécutant :

```PowerShell
Get-PmemDisk
```

Voici un exemple de sortie :

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Nous pouvons voir que le disque PMEM logique #2 a des unités physiques de Id20 et Id120 et que le disque PMEM logique #3 a des unités physiques de Id1020 et Id1120. Nous pouvons également alimenter un disque PMEM spécifique vers PmemPhysicalDevice pour obtenir tous ses NVDIMMs physiques dans l’entrelacement défini comme indiqué ci-dessous.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

Voici un exemple de sortie :

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>Configuration de groupes entrelacés

Pour configurer un ensemble entrelacé, exécutez l’applet de commande PowerShell suivante :

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Cela affiche toutes les régions de la mémoire persistante non affectées à un disque de mémoire persistante logique sur le système.

Pour afficher toutes les informations sur les périphériques de mémoire persistants dans le système, y compris le type d’appareil, l’emplacement, l’intégrité et l’état opérationnel, etc., vous pouvez exécuter l’applet de commande suivante sur le serveur local :

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile
                                                                                                                      memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Étant donné que nous disposons d’une région PMEM non utilisée, nous pouvons créer de nouveaux disques de mémoire persistants. Nous pouvons créer plusieurs disques de mémoire persistants à l’aide des régions inutilisées par :

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Lancé cette opération, nous pouvons voir les résultats en exécutant :

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Il est à noter que nous aurions pu exécuter **obtenir-PhysicalDisk | Où MediaType-EQ SCM** au lieu de l' **PmemDisk** pour obtenir les mêmes résultats. Le disque de mémoire persistante qui vient d’être créé correspond à 1:1 pour les lecteurs qui s’affichent dans PowerShell et le centre d’administration Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Utilisation de la mémoire persistante pour le cache ou la capacité

Espaces de stockage direct sur Windows Server 2019 prend en charge l’utilisation de la mémoire persistante en tant que lecteur de cache ou de capacité. Pour plus d’informations sur la configuration des lecteurs de cache et de capacité, consultez cette [documentation](understand-the-cache.md) .

## <a name="creating-a-dax-volume"></a>Création d’un volume DAX

### <a name="understanding-dax"></a>Fonctionnement de DAX

Il existe deux méthodes pour accéder à la mémoire persistante. Celles-ci sont les suivantes :

1. **Direct Access (DAX)** , qui fonctionne comme la mémoire pour bénéficier de la latence la plus faible. L’application modifie directement la mémoire persistante, en ignorant la pile. Notez que cela ne peut être utilisé qu’avec NTFS.
2. **Bloquer l’accès**, qui fonctionne comme le stockage pour la compatibilité des applications. Les données transitent par la pile dans cette configuration, et peuvent être utilisées avec NTFS et ReFS.

Vous pouvez voir un exemple ci-dessous :

![Pile DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configuration de DAX

Nous devons utiliser des applets de commande PowerShell pour créer un volume DAX sur une mémoire persistante. En utilisant le commutateur **-IsDax** , vous pouvez mettre en forme un volume pour qu’il soit compatible avec Dax.

```PowerShell
Format-Volume -IsDax:$true
```

L’extrait de code suivant vous aidera à créer un volume DAX sur un disque mémoire persistant.

```PowerShell
# Here we use the first pmem disk to create the volume as an example
$disk = (Get-PmemDisk)[0] | Get-PhysicalDisk | Get-Disk
# Initialize the disk to GPT if it is not initialized
If ($disk.partitionstyle -eq "RAW") {$disk | Initialize-Disk -PartitionStyle GPT}
# Create a partition with drive letter 'S' (can use any available drive letter)
$disk | New-Partition -DriveLetter S -UseMaximumSize


   DiskPath: \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{53f56307-b6
bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                               Size Type
---------------  ----------- ------                                               ---- ----
2                S           16777216                                        251.98 GB Basic

# Format the volume with drive letter 'S' to DAX Volume
Format-Volume -FileSystem NTFS -IsDax:$true -DriveLetter S

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining      Size
----------- ------------ -------------- --------- ------------ ----------------- -------------      ----
S                        NTFS           Fixed     Healthy      OK                    251.91 GB 251.98 GB

# Verify the volume is DAX enabled
Get-Partition -DriveLetter S | fl


UniqueId             : {00000000-0000-0000-0000-000100000000}SCMLD\VEN_8980&DEV_097A&SUBSYS_89804151&REV_0018\3&1B1819F6&0&03018089F
                       B63494DB728D8418B3CBBF549997891:WIN-8KGI228ULGA
AccessPaths          : {S:\, \\?\Volume{cf468ffa-ae17-4139-a575-717547d4df09}\}
DiskNumber           : 2
DiskPath             : \\?\scmld#ven_8980&dev_097a&subsys_89804151&rev_0018#3&1b1819f6&0&03018089fb63494db728d8418b3cbbf549997891#{5
                       3f56307-b6bf-11d0-94f2-00a0c91efb8b}
DriveLetter          : S
Guid                 : {cf468ffa-ae17-4139-a575-717547d4df09}
IsActive             : False
IsBoot               : False
IsHidden             : False
IsOffline            : False
IsReadOnly           : False
IsShadowCopy         : False
IsDAX                : True                   # <- True: DAX enabled
IsSystem             : False
NoDefaultDriveLetter : False
Offset               : 16777216
OperationalStatus    : Online
PartitionNumber      : 2
Size                 : 251.98 GB
Type                 : Basic
```

## <a name="monitoring-health"></a>Surveillance de l’intégrité

Lorsque vous utilisez la mémoire persistante, il existe quelques différences dans l’expérience de surveillance :

1. La mémoire persistante ne crée pas de compteurs de performances de disque physique. vous ne verrez donc pas si les graphiques apparaissent dans le centre d’administration Windows.
2. La mémoire persistante ne crée pas de données Storport 505, vous n’obtiendrez donc pas la détection proactive des valeurs hors norme.

En outre, l’expérience de surveillance est la même que pour tout autre disque physique. Vous pouvez interroger l’intégrité d’un disque de mémoire persistante en exécutant :

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Unhealthy    None          True         {20, 120}         2
3          252 GB Healthy      None          True         {1020, 1120}      0

Get-PmemDisk | Get-PhysicalDisk | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails

SerialNumber               HealthStatus OperationalStatus  OperationalDetails
------------               ------------ ------------------ ------------------
802c-01-1602-117cb5fc      Healthy      OK
802c-01-1602-117cb64f      Warning      Predictive Failure {Threshold Exceeded,NVDIMM_N Error}
```

**HealthStatus** indique si le disque de mémoire persistante est sain ou non. Le **UnsafeshutdownCount** suit le nombre d’arrêts qui peuvent entraîner une perte de données sur ce disque logique. Il s’agit de la somme du nombre d’arrêts non sécurisés de tous les périphériques de mémoire persistants sous-jacents de ce disque. Nous pouvons également utiliser les commandes ci-dessous pour interroger les informations d’intégrité. **OperationalStatus** et **OperationalDetails** fournissent des informations supplémentaires sur l’état d’intégrité.

Pour interroger l’intégrité de l’appareil de mémoire persistante :

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Cela indique quel périphérique de mémoire persistante est défectueux. L’appareil non sain (**DeviceID**) 20 correspond au cas dans l’exemple ci-dessus. Le **PhysicalLocation** du BIOS peut aider à identifier l’appareil de mémoire persistante dans un état défectueux.

## <a name="replacing-persistent-memory"></a>Remplacement de la mémoire persistante

Nous avons décrit ci-dessus comment afficher l’état d’intégrité de votre mémoire persistante. Si vous devez remplacer un module qui a échoué, vous devez reconfigurer le disque de mémoire persistante (reportez-vous aux étapes décrites ci-dessus).

Lors du dépannage, vous devrez peut-être utiliser **Remove-PmemDisk pour supprimer**un disque de mémoire persistante spécifique. Nous pouvons supprimer tous les disques persistants actuels en :

```PowerShell
Get-PmemDisk | Remove-PmemDisk

cmdlet Remove-PmemDisk at command pipeline position 1
Supply values for the following parameters:
DiskNumber: 2

This will remove the persistent memory disk(s) from the system and will result in data loss.
Remove the persistent memory disk(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
Removing the persistent memory disk. This may take a few moments.
```

Il est important de noter que la suppression d’un disque de mémoire persistante entraîne une perte de données sur ce disque.

Une autre applet de commande dont vous pouvez avoir besoin est **Initialize-PmemPhysicalDevice**, qui initialise la zone de stockage des étiquettes sur les périphériques de mémoire persistante physiques. Cela peut être utilisé pour effacer les informations de stockage des étiquettes endommagées sur les périphériques de mémoire persistants.

```PowerShell
Get-PmemPhysicalDevice | Initialize-PmemPhysicalDevice

This will initialize the label storage area on the physical persistent memory device(s) and will result in data loss.
Initializes the physical persistent memory device(s)?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): A
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
Initializing the physical persistent memory device. This may take a few moments.
```

Il est important de noter que cette commande doit être utilisée en dernier recours pour résoudre les problèmes liés à la mémoire persistante. Cela entraînera une perte de données dans la mémoire persistante.

## <a name="see-also"></a>Voir aussi

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- [Gestion de l’intégrité de la mémoire de classe stockage (NVDIMM-N) dans Windows](storage-class-memory-health.md)
- [Fonctionnement du cache](understand-the-cache.md)