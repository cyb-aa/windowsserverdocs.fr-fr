---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: Fsutil fsinfo
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 472c3b91285810ac1ff528da24de50533bae526d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376942"
---
# <a name="fsutil-fsinfo"></a>Fsutil fsinfo
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Répertorie tous les lecteurs, interroge le type de lecteur, interroge les informations sur le volume, interroge les informations de volume spécifiques à NTFS ou interroge les statistiques du système de fichiers.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil fsinfo [drives]
fsutil fsinfo [drivetype] <VolumePath>
fsutil fsinfo [ntfsinfo] <RootPath>
fsutil fsinfo [statistics] <VolumePath>
fsutil fsinfo [volumeinfo] <RootPath>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|durs|Répertorie tous les lecteurs de l’ordinateur.|
|DriveType|Interroge un lecteur et répertorie son type, par exemple lecteur de CD-ROM.|
|NTFSInfo|Répertorie les informations de volume spécifiques à NTFS pour le volume spécifié, telles que le nombre de secteurs, le nombre total de clusters, les clusters libres et le début et la fin de la zone MFT.|
|sectorinfo|Répertorie des informations sur la taille et l’alignement des secteurs du matériel.|
|portent|Répertorie les statistiques du système de fichiers pour le volume spécifié, telles que les métadonnées, le fichier journal et les lectures et écritures MFT.|
|volumeinfo|Répertorie des informations sur le volume spécifié, telles que le système de fichiers, et indique si le volume prend en charge les noms de fichiers sensibles à la casse, Unicode dans les noms de fichiers, les quotas de disque ou s’il s’agit d’un volume DirectAccess (DAX).|
|< « VolumePath » >|Spécifie la lettre de lecteur (suivie d’un signe deux-points).|
|< « RootPathname » >|Spécifie la lettre de lecteur (suivie d’un signe deux-points) du lecteur racine.|

## <a name="BKMK_examples"></a>Illustre
Pour répertorier tous les lecteurs de l’ordinateur, tapez :

```
fsutil fsinfo drives
```

Une sortie similaire à ce qui suit s’affiche :

```
Drives: A:\ C:\ D:\ E:\       
```

Pour interroger le type de lecteur C, tapez :

```
fsutil fsinfo drivetype c:
```

Les résultats possibles de la requête sont les suivants :

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Pour interroger les informations sur le volume E, tapez :

```
fsinfo volumeinfo e:\
```

Une sortie similaire à ce qui suit s’affiche :

```
Volume Name :Volume
Serial Number : 0xd0b634d9
Max Component Length : 255
File System Name : NTFS
.
.
.
Supports Named Streams
Is DAX Volume
```

Pour rechercher des informations sur les volumes spécifiques à NTFS dans le lecteur F, tapez :

```
fsutil fsinfo ntfsinfo f:
```

Une sortie similaire à ce qui suit s’affiche :

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Pour rechercher les informations de secteur dans le matériel sous-jacent du système de fichiers, tapez :

```
fsinfo sectorinfo d:
```

Une sortie similaire à ce qui suit s’affiche :

```
D:\>fsutil fsinfo sectorinfo d:
LogicalBytesPerSector :                                 4096
PhysicalBytesPerSectorForAtomicity :                    4096
.
.
.
Trim Not Supported
DAX capable
```

Pour interroger les statistiques du système de fichiers pour le lecteur E, tapez :

```
fsinfo statistics e:
```

Une sortie similaire à ce qui suit s’affiche :

```
File System Type :     NTFS
Version :              1
UserFileReads :        75021
UserFileReadBytes :    1305244512
.
.
.
LogFileWriteBytes :    180936704       
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)
[fsutil](Fsutil.md)


