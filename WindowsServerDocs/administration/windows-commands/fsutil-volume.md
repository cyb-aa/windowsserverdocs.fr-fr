---
ms.assetid: 0397c204-b3f8-4fd8-b71d-b7efb117766d
title: Fsutil volume
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: a8576dce4be639a516f8898e78bb6db12c91e171
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882450"
---
# <a name="fsutil-volume"></a>Fsutil volume
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Démonte un volume ou interroge le lecteur de disque dur pour déterminer la quantité d’espace libre est disponible sur le lecteur de disque dur ou le fichier qui utilise un cluster particulier.

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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|allocationreport|Affiche des informations sur l’utilisation du stockage sur un volume donné.|
|\<VolumePath>|Spécifie la lettre de lecteur (suivie d’un signe deux-points).|
|diskfree|Interroge le lecteur de disque dur pour déterminer la quantité d’espace libre sur ce dernier.|
|démonter|Démonte un volume.|
|filelayout|Affiche les métadonnées NTFS pour le fichier donné.|
|\<fileid>|Spécifie l’id de fichier.|
|list|Répertorie tous les volumes sur le système.|
|querycluster|Recherche le fichier qui utilise un cluster spécifié. Vous pouvez spécifier plusieurs clusters avec le **querycluster** paramètre.<br /><br />Ce paramètre s’applique à :  Windows Server 2008 R2 et Windows 7.|
|\<cluster>|Spécifie le nombre de cluster logique (LCN).|

## <a name="BKMK_examples"></a>Exemples
Pour afficher un rapport de clusters alloués, tapez :

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

Pour afficher toutes les informations concernant un ou les fichiers spécifiés, tapez :

```
fsutil volume C: *
fsutil volume C:\Windows
fsutil volume C: 0x00040000000001bf
```

Pour répertorier les volumes sur le disque, tapez :

```
fsutil volume list
```

Pour rechercher l’ou les fichiers qui sont à l’aide de clusters, spécifiés par les numéros de cluster logique 50 et 0 x 2000, sur le lecteur C, tapez :

```
fsutil volume querycluster C: 50 0x2000
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fonctionnement de NTFS](https://go.microsoft.com/fwlink/?LinkId=183396)


