---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: fsutil reparsepoint
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 940131a02a5cd3a6122022cf9b0dff3281d1dabf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847940"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Points d’analyse des requêtes ou des suppressions.  Le **fsutil reparsepoint** commande est généralement utilisée par les professionnels du support.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|requête|Récupère les données de point d’analyse sont associées au fichier ou répertoire identifié par le handle spécifié.|
|supprimer|Supprime un point d’analyse du fichier ou du répertoire qui est identifié par le handle spécifié, mais ne supprime pas le fichier ou répertoire.|
|<FileName>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt.|

## <a name="remarks"></a>Notes

-   Points d’analyse sont des objets système qui possèdent un attribut définissable contenant des données définies par l’utilisateur de fichiers NTFS, et ils sont utilisés pour étendre les fonctionnalités du sous-système d’entrée/sortie (e/s) de.

-   D’analyse points sont utilisés pour les points de jonction de répertoire et les points de montage de volume. Ils sont également utilisés par les pilotes de filtre de système de fichiers pour marquer certains fichiers propres à ce pilote.

-   Lorsqu’un programme définit un point d’analyse, il stocke ces données, ainsi que la balise d’analyse, qui identifie de façon unique les données qu’il stocke. Quand le système de fichiers ouvre un fichier avec un point d’analyse, il tente de trouver le filtre de système de fichier associé. Si le filtre de système de fichiers est trouvé, le filtre traite le fichier comme indiqué par les données d’analyse. Si aucun filtre de système de fichiers n’est trouvé, l’opération d’ouverture de fichier échoue.

## <a name="BKMK_examples"></a>Exemples
Pour récupérer les données de point d’analyse associées à C:\Server, tapez :

```
fsutil reparsepoint query c:\server
```

Pour supprimer un point d’analyse à partir d’un fichier ou répertoire spécifié, utilisez le format suivant :

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


