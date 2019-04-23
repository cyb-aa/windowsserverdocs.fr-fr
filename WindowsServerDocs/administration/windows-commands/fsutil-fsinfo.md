---
ms.assetid: 7787a72e-a26b-415f-b700-a32806803478
title: fsutil fsinfo
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 434dfde2286538367fb96d168b06983cb4357067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873040"
---
# <a name="fsutil-fsinfo"></a>fsutil fsinfo
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Répertorie tous les lecteurs, le type de lecteur, interroge les informations de volume, interroge les informations de volume NTFS spécifiques ou interroge les statistiques sur les fichiers système.

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
|lecteurs|Répertorie tous les lecteurs de l’ordinateur.|
|DriveType|Interroge un lecteur et affiche son type, le lecteur de CD-ROM par exemple.|
|ntfsinfo|Répertorie des informations de volume spécifique de NTFS pour le volume spécifié, comme le nombre de secteurs, nombre total de clusters, les clusters libres et début et de fin de la Zone MFT.|
|sectorinfo|Répertorie des informations sur la taille de secteur et l’alignement de matériel.|
|Statistiques|Listes des statistiques du système pour le volume spécifié, telles que les métadonnées, fichier journal et MFT lectures et écritures de fichiers.|
|volumeinfo|Répertorie des informations pour le volume spécifié, telles que le système de fichiers et si le volume prend en charge les noms de fichiers respectant la casse, unicode dans les noms de fichiers, les quotas de disque, ou est un volume DirectAccess (DAX).|
|<"VolumePath">|Spécifie la lettre de lecteur (suivie d’un signe deux-points).|
|<"RootPathname">|Spécifie la lettre de lecteur (suivie d’un signe deux-points) du lecteur racine.|

## <a name="BKMK_examples"></a>Exemples
Pour répertorier tous les lecteurs de l’ordinateur, tapez :

```
fsutil fsinfo drives
```

Sortie similaire à ce qui suit affiche :

```
Drives: A:\ C:\ D:\ E:\       
```

Pour interroger le type de lecteur du lecteur C, tapez :

```
fsutil fsinfo drivetype c:
```

Les résultats possibles de la requête sont les suivantes :

```
Unknown Drive
No such Root Directory
Removable Drive, for example floppy
Fixed Drive
Remote/Network Drive
CD-ROM Drive
Ram Disk
```

Pour interroger les informations de volume pour le volume E, tapez :

```
fsinfo volumeinfo e:\
```

Sortie similaire à ce qui suit affiche :

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

Pour interroger le lecteur F pour des informations spécifiques de NTFS, tapez :

```
fsutil fsinfo ntfsinfo f:
```

Sortie similaire à ce qui suit affiche :

```
NTFS Volume Serial Number : 0xe660d46a60d442cb
Number Sectors :            0x00000000010ea04f
Total Clusters :            0x000000000021d409
.
.
.
Mft Zone End   :            0x0000000000004700       
```

Pour interroger le fichier matériel du système sous-jacent pour les informations de secteur, tapez :

```
fsinfo sectorinfo d:
```

Sortie similaire à ce qui suit affiche :

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

Pour obtenir les statistiques de système de fichiers pour le lecteur E, tapez :

```
fsinfo statistics e:
```

Sortie similaire à ce qui suit affiche :

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
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)
[Fsutil](Fsutil.md)


