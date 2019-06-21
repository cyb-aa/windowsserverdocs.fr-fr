---
title: Comprendre et à déployer mémoire persistante
description: Obtenir des informations détaillées sur le type de mémoire persistante et comment configurer avec des espaces de stockage direct dans Windows Server 2019.
keywords: Espaces de stockage Direct, mémoire persistante, pmem, stockage, S2D
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: ed4b2669ad35a2ce0f818c65f7024ce905d9e4d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280045"
---
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprendre et à déployer mémoire persistante

>S’applique à : Windows Server 2019

Mémoire persistante (ou PMem) est un nouveau type de technologie de mémoire qui offre une combinaison unique de grande capacité abordable et la persistance. Cette rubrique fournit en arrière-plan sur PMem et les étapes pour déployer avec Windows Server 2019 avec espaces de stockage Direct.

## <a name="background"></a>Arrière-plan

PMem est un type de volatile non DRAM (NVDIMM) qui a la vitesse de DRAM, mais conserve son contenu de la mémoire par le biais de cycles d’alimentation (le contenu de la mémoire reste même lorsque l’alimentation du système tombe en panne en cas d’une coupure de courant inattendue, l’arrêt de lancée par l’utilisateur, la panne système, et ainsi de suite). Pour cette raison, la reprise à partir de l’endroit où vous en étiez est considérablement plus rapide, étant donné que le contenu de votre mémoire RAM n’a pas besoin être rechargé. Une autre caractéristique unique est PMem octets adressable, ce qui signifie que vous pouvez également l’utiliser en tant que stockage (c’est pourquoi vous entendez PMem référencée en tant que la mémoire de classe de stockage).


Pour afficher certains de ces avantages, nous allons examiner cette démonstration à partir de Microsoft Ignite 2018 :

[![Démonstration de Microsoft Ignite 2018 Pmem](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

N’importe quel système de stockage qui fournit une tolérance de panne nécessairement crée des copies distribuées d’écritures, ce qui doivent parcourir le réseau et entraîne l’amplification d’écriture de back-end. Pour cette raison, les plus grands nombres de banc d’essai IOPS absolus sont généralement réalisés avec des lectures uniquement, de surtout si le système de stockage a des optimisations de bon sens pour lire à partir de la copie locale si possible, les espaces de stockage Direct est.

**Avec des lectures de 100 %, le cluster fournit 13,798,674 e/s.**

![capture d’écran enregistrement de m 13,7 e/s](media/deploy-pmem/iops-record.png)

Si vous regardez la vidéo près, vous remarquerez plus encore de thatwhat saisissants est la latence : même au plus 13,7 M IOPS, le système de fichiers dans Windows signale une latence est régulièrement inférieure à 40 µs ! (C’est le symbole pour microsecondes, millionième de seconde). Il s’agit d’un ordre de grandeur plus rapidement que ce que les fournisseurs exclusivement flash typiques fièrement publient dès aujourd'hui.

Ensemble, espaces de stockage Direct dans Windows Server 2019 et mémoire persistante Intel® Optane™ DC offrent des performances exceptionnelles. Ce leader du secteur HCL banc d’essai plus 13,7 m e/s, avec une latence prévisible et extrêmement faible, n’existe plus de double notre précédente leader du secteur de 6,7 M e/s. Quel est le plus, cette fois nous devions que 12 nœuds de serveur, 25 % de moins de deux ans.

![Gains d’e/s](media/deploy-pmem/iops-gains.png)

Le matériel utilisé a été un cluster de serveur de 12 à l’aide de la mise en miroir triple et délimité par des volumes ReFS, **12** x Intel® S2600WFT, **Gio 384** mémoire, 2 x 28-core « CascadeLake » **1,5 to**Mémoire persistante d’Intel® Optane™ DC en tant que cache, **32 To** NVMe (4 x 8 TB Intel® DC P4510) en tant que capacité, **2** x Gbits/s de 25 Mellanox ConnectX-4

Le tableau ci-dessous présente les chiffres de performances complet : 

| Test d’évaluation                   | Performances         |
|-----------------------------|---------------------|
| Lecture aléatoire de 100 % de 4K         | E/s 13,8 millions   |
| 4 K 90/10 % en lecture/écriture aléatoires | e/s 9.45 millions   |
| 2 Mo séquentielle de lecture         | Débit 549 Go/s |

### <a name="supported-hardware"></a>Matériel pris en charge

Le tableau ci-dessous présente le matériel de mémoire persistante pris en charge pour Windows Server 2019 et Windows Server 2016. Notez que Intel Optane spécifiquement prend en charge les deux modes de mémoire et direct de l’application. Windows Server 2019 prend en charge les opérations en mode mixte.

| Technologie de mémoire persistante                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** en Mode d’application-Direct                                       | Prise en charge                | Prise en charge                |
| **Mémoire persistante de Intel Optane™ DC** en Mode d’application-Direct             | Non prise en charge            | Prise en charge                |
| **Mémoire persistante de Intel Optane™ DC** en Mode de mémoire d’au niveau de deux (2LM) | Non prise en charge            | Prise en charge                |

Maintenant, nous allons approfondir la configuration de mémoire persistante.

## <a name="interleave-sets"></a>Entrelacement de jeux

### <a name="understanding-interleave-sets"></a>Définit un entrelacement de présentation

Rappelez-vous que le dispositif NVDIMM-N réside dans un emplacement DIMM (mémoire) standard, placer des données plus près le processeur (par conséquent, ce qui réduit la latence et l’extraction de meilleures performances). Pour générer sur cela, un ensemble de l’entrelacement est lorsque deux ou plusieurs NVDIMMs créez un entrelacement de N directions pour fournir des opérations de lecture/écriture de bandes pour un débit plus élevé. Les configurations les plus courantes sont voies 2 ou 4 voies entrelacement.

Jeux entrelacés peut souvent être créés dans le BIOS d’une plateforme pour rendre plusieurs dispositifs de mémoire persistante apparaissent sous la forme d’un seul disque logique pour Windows Server. Chaque disque logique de mémoire persistante contient un ensemble entrelacé de périphériques physiques en exécutant :

```PowerShell
Get-PmemDisk
```

Vous trouverez ci-dessous un exemple de sortie :

```
DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Nous pouvons voir que le disque logique pmem #2 a des appareils physiques de Id20 et Id120 et disque logique pmem #3 a des appareils physiques de Id1020 et Id1120. Nous pouvons également alimenter un disque spécifique pmem à Get-PmemPhysicalDevice pour obtenir tous les sa NVDIMMs physiques dans l’intervalle défini comme indiqué ci-dessous.


```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice
```

Vous trouverez ci-dessous un exemple de sortie :

```
DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleave-sets"></a>Configuration d’un entrelacement de jeux

Pour configurer un ensemble d’entrelacement, exécutez l’applet de commande PowerShell suivante :

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Cela montre toutes les régions de mémoire persistante ne pas affectées à un disque logique mémoire persistante sur le système.

Pour afficher toutes les informations sur les périphériques mémoire persistante dans le système, y compris le type d’appareil, emplacement, l’intégrité et état opérationnel, etc. vous pouvez exécuter l’applet de commande suivante sur le serveur local :

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

Étant donné que nous avons pmem inutilisée disponible région, nous pouvons créer des nouveaux disques mémoire persistante. Nous pouvons créer plusieurs disques mémoire persistante à l’aide de régions inutilisées par :

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Après avoir que cette opération est effectuée, nous pouvons voir les résultats en exécutant :

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Il est important de noter que nous pourrions avoir exécuter **Get-PhysicalDisk | Où MediaType - Eq SCM** au lieu de **Get-PmemDisk** pour obtenir les mêmes résultats. Le disque mémoire persistante nouvellement créé correspond à 1:1 pour les lecteurs qui s’affichent dans PowerShell et Windows Admin Center.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>À l’aide de la mémoire persistante du cache, ou capacité

Espaces de stockage Direct sur Windows Server 2019 prend en charge à l’aide de la mémoire persistante en tant que lecteur d’un cache ou une capacité. Consultez ce [documentation](understand-the-cache.md) pour plus d’informations sur la configuration des lecteurs de cache et la capacité.

## <a name="creating-a-dax-volume"></a>Création d’un Volume DAX

### <a name="understanding-dax"></a>Fonctionnement de DAX

Il existe deux méthodes pour accéder à la mémoire persistante. Celles-ci sont les suivantes :

1. **Accès (DAX) direct**, qui fonctionne comme la mémoire pour obtenir la latence la plus faible. L’application modifie directement la mémoire persistante, en contournant la pile. Notez que cela peut uniquement être utilisée avec NTFS.
2. **Bloquer l’accès**, qui fonctionne comme le stockage pour la compatibilité des applications. Les données circulent dans la pile dans cette configuration, et celui-ci peut être utilisé avec NTFS et ReFS.

Un exemple de ceci est affiché ci-dessous :

![Pile DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configuration de DAX

Nous devrons à utiliser des applets de commande PowerShell pour créer un volume DAX sur une mémoire persistante. À l’aide de la **- IsDax** commutateur, nous pouvons mettre en forme un volume pour être DAX activé.

```PowerShell
Format-Volume -IsDax:$true
```

L’extrait de code suivant vous aidera à créer un volume DAX sur un disque mémoire persistante.

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

Lorsque vous utilisez mémoire persistante, il existe quelques différences dans l’expérience d’analyse :

1. Mémoire persistante ne crée pas les compteurs de performance, donc vous ne verrez si apparaissent sur les graphiques dans Windows Admin Center de disque physique.
2. Mémoire persistante ne crée pas données 505 de Storport, donc vous n’obtiendrez pas la détection proactive de valeurs hors norme.

Mis à part cela, l’expérience de surveillance est identique à n’importe quel autre disque physique. Vous pouvez interroger pour l’intégrité d’un disque mémoire persistante en exécutant :

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

**HealthStatus** indique si le disque mémoire persistante est intègre ou non. Le **UnsafeshutdownCount** suit le nombre d’arrêts qui peut entraîner une perte de données sur ce disque logique. C’est la somme des nombres unsafe arrêt de tous les périphériques de mémoire persistante sous-jacent de ce disque. Nous pouvons également utiliser les commandes ci-dessous pour demander des informations sur l’intégrité. Le **OperationalStatus** et **OperationalDetails** fournissent des informations sur l’état d’intégrité.

Pour interroger l’intégrité du périphérique de mémoire persistante :

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Cela indique que le périphérique de mémoire persistante n’est pas intègre. L’appareil défectueux (**DeviceId**) 20 respecte la casse dans l’exemple ci-dessus. Le **PhysicalLocation** du BIOS peut aider à identifier quelle mémoire persistante périphérique est dans un état défectueux.

## <a name="replacing-persistent-memory"></a>Remplacement de mémoire persistante

Ci-dessus, nous avons décrit comment afficher l’état d’intégrité de votre mémoire persistante. Si vous avez besoin de remplacer un module défectueux, vous devrez reconfigurer le disque mémoire persistante (reportez-vous à la procédure que nous avons présenté ci-dessus).

Lors du dépannage, vous devrez peut-être utiliser **Remove-PmemDisk**, qui supprime un disque mémoire persistante spécifique. Nous pouvons supprimer tous les disques persistants actuels par :

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

Il est important de noter que la suppression d’un disque mémoire persistante entraîne une perte de données sur ce disque.

Est une autre applet de commande que vous devrez peut-être **Initialize-PmemPhysicalDevice**, qui initialisera la zone de stockage étiquette sur les appareils physiques de mémoire persistante. Cela peut être utilisé pour effacer les informations sur le stockage étiquette endommagé sur les appareils de mémoire persistante.

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

Il est important de noter que cette commande doit être utilisée en dernier recours pour corriger la mémoire persistante les problèmes liés. Il entraîne une perte de données à la mémoire persistante.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- [Gestion de l’intégrité de classe de stockage (NVDIMM-N) de mémoire dans Windows](storage-class-memory-health.md)
- [Fonctionnement du cache](understand-the-cache.md)