---
title: Comprendre et déployer la mémoire persistante
description: Informations détaillées sur la mémoire persistante et la configuration des espaces de stockage direct dans Windows Server 2019.
keywords: Espaces de stockage direct, mémoire persistante, PMEM, stockage, S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 1/27/2020
ms.localizationpriority: medium
ms.openlocfilehash: a9070d2e2ab73c7882f4b2ef585ccb01986695bb
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822312"
---
# <a name="understand-and-deploy-persistent-memory"></a>Comprendre et déployer la mémoire persistante

> S’applique à : Windows Server 2019

La mémoire persistante (ou PMem) est un nouveau type de technologie de mémoire qui offre une combinaison unique de grande capacité et persistance abordable. Cet article fournit des informations générales sur PMem et les étapes à suivre pour le déployer dans Windows Server 2019 à l’aide de espaces de stockage direct.

## <a name="background"></a>Contexte

PMem est un type de RAM non volatile (NVDIMM) qui conserve son contenu par le biais de cycles de gestion de l’alimentation. Le contenu de la mémoire reste même en cas de panne de courant, en cas de perte d’alimentation inattendue, d’arrêt de l’utilisateur, de panne du système, etc. Cette caractéristique unique signifie que vous pouvez également utiliser PMem comme stockage. C’est la raison pour laquelle vous pouvez entendre que les gens font référence à PMem en tant que « mémoire de classe de stockage ».

Pour voir certains de ces avantages, examinons la démonstration suivante de Microsoft en2018.

[![démonstration PMEM de Microsoft 2018](http://img.youtube.com/vi/8WMXkMLJORc/0.jpg)](http://www.youtube.com/watch?v=8WMXkMLJORc)

Tout système de stockage qui fournit la tolérance de panne effectue nécessairement des copies distribuées des écritures. Ces opérations doivent traverser le réseau et amplifier le trafic d’écriture du backend. Pour cette raison, les valeurs d’évaluation des IOPS les plus élevées sont généralement obtenues en mesurant uniquement les lectures, en particulier si le système de stockage a des optimisations de sens commun à lire à partir de la copie locale chaque fois que cela est possible. Espaces de stockage direct est optimisée pour ce faire.

**Lorsqu’il est mesuré en utilisant uniquement des opérations de lecture, le cluster fournit 13 798 674 e/s par seconde.**

![capture d’écran enregistrement IOPS 13.7 m](media/deploy-pmem/iops-record.png)

Si vous regardez la vidéo de près, vous remarquerez que ce qui est encore plus la suppression de la mâchoire est la latence. Même avec plus de 13,7 M e/s par seconde, le système de fichiers dans Windows signale une latence qui est constamment inférieure à 40 μs ! (Il s’agit du symbole pour les microsecondes, du 1 au millionième de seconde.) Cette vitesse est un ordre de grandeur plus rapide que celui des fournisseurs habituels de tous les fournisseurs de Flash.

Ensemble, espaces de stockage direct dans Windows Server 2019 et Intel® Optane™ la mémoire persistante DC offre des performances exceptionnelles. Ce point de référence HCI de plus en plus de 13.7 M IOPS, accompagné d’une latence prévisible et extrêmement faible, est plus que doubler notre premier point de référence du secteur de 6,7 millions d’e/s par seconde. De plus, cette fois, nous avons besoin de 12 nœuds de serveur&mdash;25% moins de deux ans auparavant.

![Gains d’e/s par seconde](media/deploy-pmem/iops-gains.png)

Le matériel de test était un cluster à 12 serveurs qui a été configuré pour utiliser des volumes ReFS de mise en miroir triple et délimités. **12** x Intel® S2600WFT, **384 Gio** de mémoire, 2 x 28-cœurs « CASCADELAKE », **1,5 to** Intel® Optane™ DC persistent Memory comme cache, **32 to** NVME (4 x 8 to Intel® DC P4510) as, **2** x Mellanox ConnectX-4 25 Gbits/s.

Le tableau suivant présente les nombres de performances complets.  

| TPC                   | Performances         |
|-----------------------------|---------------------|
| lecture aléatoire de 4 Ko 100%         | 13,8 millions e/s par seconde   |
| 4 Ko 90/10% de lecture/écriture aléatoire | 9 450 000 e/s par seconde   |
| 2 Mo en lecture séquentielle         | débit de 549 Go/s |

### <a name="supported-hardware"></a>Matériel pris en charge

Le tableau suivant indique le matériel de mémoire persistante pris en charge pour Windows Server 2019 et Windows Server 2016.  

| Technologie de mémoire persistante                                      | Windows Server 2016 | Windows Server 2019 |
|-------------------------------------------------------------------|--------------------------|--------------------------|
| **NVDIMM-N** en mode persistant                                  | Pris en charge                | Pris en charge                |
| **Mémoire persistante Intel Optane™ DC** en mode direct de l’application             | Pas de prise en charge            | Pris en charge                |
| **Mémoire persistante Intel Optane™ DC** en mode mémoire | Pris en charge            | Pris en charge                |

> [!NOTE]  
> Intel Optane prend en charge les modes *mémoire* (volatile) et *application directe* (persistante).
   
> [!NOTE]  
> Lorsque vous redémarrez un système qui contient plusieurs modules Intel® Optane™ PMem en mode direct de l’application qui sont divisés en plusieurs espaces de noms, vous risquez de perdre l’accès à certains ou à tous les disques de stockage logiques associés. Ce problème se produit sur les versions de Windows Server 2019 antérieures à la version 1903.
>   
> Cette perte d’accès se produit parce qu’un module PMem n’est pas formé ou échoue au démarrage du système. Dans ce cas, tous les espaces de noms de stockage sur n’importe quel module PMem du système échouent, y compris les espaces de noms qui ne sont pas physiquement mappés au module défaillant.
>   
> Pour restaurer l’accès à tous les espaces de noms, remplacez le module qui a échoué.
>   
> Si un module échoue sur Windows Server 2019 version 1903 ou des versions plus récentes, vous perdez l’accès aux seuls espaces de noms qui sont physiquement mappés au module affecté. Les autres espaces de noms ne sont pas affectés.

Voyons maintenant comment configurer la mémoire persistante.

## <a name="interleaved-sets"></a>Jeux entrelacés

### <a name="understanding-interleaved-sets"></a>Fonctionnement des jeux entrelacés

Souvenez-vous qu’une NVDIMM réside dans un emplacement DIMM standard (mémoire), ce qui rapproche les données du processeur. Cette configuration réduit la latence et améliore les performances de récupération. Pour augmenter davantage le débit, plusieurs NVDIMMs créent un ensemble entrelacé bidirectionnel pour entrelacer les opérations de lecture/écriture. Les configurations les plus courantes sont l’entrelacement bidirectionnel ou à quatre voies. Un jeu entrelacé fait également en sorte que plusieurs périphériques de mémoire persistants apparaissent comme un seul disque logique sur Windows Server. Vous pouvez utiliser l’applet de commande Windows PowerShell **PmemDisk** pour vérifier la configuration de tels disques logiques, comme suit :

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Nous pouvons voir que le disque PMem logique #2 utilise les unités physiques Id20 et Id120, et que le disque PMem logique #3 utilise les unités physiques Id1020 et Id1120.  

Pour récupérer des informations supplémentaires sur l’ensemble entrelacé utilisé par un lecteur logique, exécutez l’applet de commande Set **-PmemPhysicalDevice** :

```PowerShell
(Get-PmemDisk)[0] | Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
20       Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_C1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
```

### <a name="configuring-interleaved-sets"></a>Configuration de jeux entrelacés

Pour configurer un jeu entrelacé, commencez par passer en revue toutes les régions de mémoire persistante qui ne sont pas attribuées à un disque PMem logique sur le système. Pour ce faire, exécutez l’applet de commande PowerShell suivante :

```PowerShell
Get-PmemUnusedRegion

RegionId TotalSizeInBytes DeviceId
-------- ---------------- --------
       1     270582939648 {20, 120}
       3     270582939648 {1020, 1120}
```

Pour afficher toutes les informations de l’appareil PMem dans le système, y compris le type d’appareil, l’emplacement, l’intégrité et l’état opérationnel, et ainsi de suite, exécutez l’applet de commande suivante sur le serveur local :

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

Étant donné que nous disposons d’une région PMem non utilisée, nous pouvons créer de nouveaux disques de mémoire persistants. Nous pouvons utiliser la région inutilisée pour créer plusieurs disques de mémoire persistants en exécutant les applets de commande suivantes :

```PowerShell
Get-PmemUnusedRegion | New-PmemDisk
Creating new persistent memory disk. This may take a few moments.
```

Une fois cette opération effectuée, nous pouvons voir les résultats en exécutant :

```PowerShell
Get-PmemDisk

DiskNumber Size   HealthStatus AtomicityType CanBeRemoved PhysicalDeviceIds UnsafeShutdownCount
---------- ----   ------------ ------------- ------------ ----------------- -------------------
2          252 GB Healthy      None          True         {20, 120}         0
3          252 GB Healthy      None          True         {1020, 1120}      0
```

Il est à noter que nous pouvons exécuter la opération **de récupération sur le disque physique | Où MediaType-EQ SCM** au lieu de l' **PmemDisk** pour obtenir les mêmes résultats. Le disque PMem nouvellement créé correspond à un à un avec les lecteurs qui s’affichent dans PowerShell et dans le centre d’administration Windows.

### <a name="using-persistent-memory-for-cache-or-capacity"></a>Utilisation de la mémoire persistante pour le cache ou la capacité

Espaces de stockage direct sur Windows Server 2019 prend en charge l’utilisation de la mémoire persistante en tant que lecteur de cache ou de capacité. Pour plus d’informations sur la configuration des lecteurs de cache et de capacité, consultez [Présentation du cache dans espaces de stockage direct](understand-the-cache.md).

## <a name="creating-a-dax-volume"></a>Création d’un volume DAX

### <a name="understanding-dax"></a>Fonctionnement de DAX

Il existe deux méthodes pour accéder à la mémoire persistante. Ils sont les suivants :

1. **Direct Access (DAX)** , qui fonctionne comme la mémoire pour bénéficier de la latence la plus faible. L’application modifie directement la mémoire persistante, en ignorant la pile. Notez que vous pouvez uniquement utiliser DAX en association avec NTFS.
1. **Bloquer l’accès**, qui fonctionne comme le stockage pour la compatibilité des applications. Dans ce configuration, les données circulent dans la pile. Vous pouvez utiliser cette configuration en combinaison avec NTFS et ReFS.

L’illustration suivante montre un exemple de configuration DAX :

![Pile DAX](media/deploy-pmem/dax.png)

### <a name="configuring-dax"></a>Configuration de DAX

Nous devons utiliser des applets de commande PowerShell pour créer un volume DAX sur un disque de mémoire persistante. En utilisant le commutateur **-IsDax** , nous pouvons mettre en forme un volume pour qu’il soit compatible avec Dax.

```PowerShell
Format-Volume -IsDax:$true
```

L’extrait de code suivant vous aide à créer un volume DAX sur un disque de mémoire persistante.

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

- La mémoire persistante ne crée pas de compteurs de performances de disque physique. vous ne verrez donc pas qu’elle apparaîtra sur les graphiques dans le centre d’administration Windows.
- La mémoire persistante ne crée pas de données Storport 505, vous n’obtiendrez donc pas la détection proactive des valeurs hors norme.

En outre, l’expérience de surveillance est la même que pour n’importe quel autre disque physique. Vous pouvez interroger l’intégrité d’un disque de mémoire persistante en exécutant les applets de commande suivantes :

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

**HealthStatus** indique si le disque PMEM est sain.  

La valeur **UnsafeshutdownCount** suit le nombre d’arrêts qui peuvent entraîner une perte de données sur ce disque logique. Il s’agit de la somme du nombre d’arrêts non sécurisés de tous les appareils PMem sous-jacents de ce disque. Pour plus d’informations sur l’état d’intégrité, utilisez l’applet de commande **PmemPhysicalDevice** pour rechercher des informations telles que **OperationalStatus**.

```PowerShell
Get-PmemPhysicalDevice

DeviceId DeviceType           HealthStatus OperationalStatus PhysicalLocation FirmwareRevision Persistent memory size Volatile memory size
-------- ----------           ------------ ----------------- ---------------- ---------------- ---------------------- --------------------
1020     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_C1     102005310        126 GB                 0 GB
1120     Intel INVDIMM device Healthy      {Ok}              CPU2_DIMM_F1     102005310        126 GB                 0 GB
120      Intel INVDIMM device Healthy      {Ok}              CPU1_DIMM_F1     102005310        126 GB                 0 GB
20       Intel INVDIMM device Unhealthy    {HardwareError}   CPU1_DIMM_C1     102005310        126 GB                 0 GB
```

Cette applet de commande indique quel périphérique de mémoire persistante est défectueux. L’appareil défectueux (**DeviceID** 20) correspond au cas de l’exemple précédent. Le **PhysicalLocation** dans le BIOS peut aider à identifier l’appareil de mémoire persistante dans un état défectueux.

## <a name="replacing-persistent-memory"></a>Remplacement de la mémoire persistante

Cet article explique comment afficher l’état d’intégrité de votre mémoire persistante. Si vous devez remplacer un module défaillant, vous devez reconfigurer le disque PMem (reportez-vous aux étapes décrites précédemment).

Lorsque vous dépannez, vous devrez peut **-être utiliser Remove-PmemDisk**. Cette applet de commande supprime un disque de mémoire persistante spécifique. Nous pouvons supprimer tous les disques PMem actuels en exécutant les applets de commande suivantes :

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

> [!IMPORTANT]  
> La suppression d’un disque de mémoire persistante entraîne une perte de données sur ce disque.

**Initialize-PmemPhysicalDevice**est une autre applet de commande dont vous aurez besoin. Cette applet de commande initialise les zones de stockage des étiquettes sur les périphériques physiques de mémoire persistante et peut effacer les informations de stockage des étiquettes endommagées sur les appareils PMem.

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

> [!IMPORTANT]  
> **Initialize-PmemPhysicalDevice provoque une** perte de données dans la mémoire persistante. Utilisez-le en dernier recours pour résoudre les problèmes persistants liés à la mémoire.

## <a name="see-also"></a>Articles associés

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- [Gestion de l’intégrité de la mémoire de classe stockage (NVDIMM-N) dans Windows](storage-class-memory-health.md)
- [Fonctionnement du cache](understand-the-cache.md)
