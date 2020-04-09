---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 587ff48bd0af80667f9a336323641b87be808b1d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843932"
---
# <a name="fsutil-volume"></a>Fsutil volume
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Démonte un volume ou interroge le lecteur de disque dur pour déterminer la quantité d’espace libre actuellement disponible sur le lecteur de disque dur ou le fichier qui utilise un cluster particulier.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil volume [allocationreport] <VolumePath>
fsutil volume [diskfree] <VolumePath>
fsutil volume [dismount] <VolumePath>
fsutil volume [filelayout] <VolumePath> <fileid>
fsutil volume [list]
fsutil volume [querycluster] <VolumePath> <Cluster> [<Cluster>] … …
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|allocationreport|Affiche des informations sur l’utilisation du stockage sur un volume donné.|
|\<VolumePath >|Spécifie la lettre de lecteur (suivie d’un signe deux-points).|
|diskfree|Interroge le lecteur de disque dur pour déterminer la quantité d’espace disponible sur celui-ci.|
|démonter|Démonte un volume.|
|filelayout|Affiche les métadonnées NTFS pour le fichier donné.|
|\<fileid >|Spécifie l’ID du fichier.|
|list|Répertorie tous les volumes du système.|
|querycluster|Recherche le fichier qui utilise un cluster spécifié. Vous pouvez spécifier plusieurs clusters avec le paramètre **querycluster** .<p>Ce paramètre s’applique à : Windows Server 2008 R2 et Windows 7.|
|> du cluster \<|Spécifie le LCN (Logical cluster Number).|

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour afficher un rapport sur les clusters alloués, tapez :

```
fsutil volume allocationreport C:
```

Pour démonter un volume sur le lecteur C, tapez :

```
fsutil volume dismount c:
```

Pour interroger la quantité d’espace libre d’un volume sur le lecteur C, tapez :

```
fsutil volume diskfree c:
```

Pour afficher toutes les informations relatives à un ou plusieurs fichiers spécifiés, tapez :

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Pour répertorier les volumes sur le disque, tapez :

```
fsutil volume list
```

Pour rechercher le ou les fichiers qui utilisent les clusters, spécifiés par les numéros de cluster logique 50 et 0x2000, sur le lecteur C, tapez :

```
fsutil volume querycluster C: 50 0x2000
```

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[Fonctionnement de NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


