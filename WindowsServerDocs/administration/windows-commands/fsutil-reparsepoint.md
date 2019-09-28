---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: Fsutil reparsepoint
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fe274ad9a6dffc72607102d3430ba7527d3cc558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376853"
---
# <a name="fsutil-reparsepoint"></a>Fsutil reparsepoint
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Interroge ou supprime des points d’analyse.  La commande **fsutil reparsepoint** est généralement utilisée par les professionnels du support technique.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Paramètres

| Paramètre  |                                                                Description                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   requête    |            Récupère les données du point d’analyse associées au fichier ou au répertoire identifié par le handle spécifié.             |
|   supprimer   | Supprime un point d’analyse du fichier ou du répertoire identifié par le handle spécifié, mais ne supprime pas le fichier ou le répertoire. |
| <FileName> |             Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt.             |

## <a name="remarks"></a>Notes

-   Les points d’analyse sont des objets de système de fichiers NTFS qui ont un attribut définissable qui contient des données définies par l’utilisateur, et ils sont utilisés pour étendre les fonctionnalités dans le sous-système d’entrée/sortie (e/s).

-   Les points d’analyse sont utilisés pour les points de jonction de répertoires et les points de montage de volume. Ils sont également utilisés par les pilotes de filtre de système de fichiers pour marquer certains fichiers comme étant spéciaux pour ce pilote.

-   Lorsqu’un programme définit un point d’analyse, il stocke ces données, ainsi qu’une balise d’analyse, qui identifie de façon unique les données qu’il stocke. Lorsque le système de fichiers ouvre un fichier avec un point d’analyse, il tente de trouver le filtre du système de fichiers associé. Si le filtre de système de fichiers est trouvé, le filtre traite le fichier comme indiqué par les données d’analyse. Si aucun filtre de système de fichiers n’est trouvé, l’opération d’ouverture de fichier échoue.

## <a name="BKMK_examples"></a>Illustre
Pour récupérer les données du point d’analyse associées à C:\Server, tapez :

```
fsutil reparsepoint query c:\server
```

Pour supprimer un point d’analyse d’un fichier ou d’un répertoire spécifié, utilisez le format suivant :

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


