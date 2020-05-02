---
title: Obtient-image
description: Pour obtenir des informations sur les images contenues dans un fichier image Windows (. wim), consultez la rubrique de référence relative à la fonction.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f60be17f13e1436a0e895991c72d5ccb7130782
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719913"
---
# <a name="get-imagefile"></a>Obtient-image

Récupère des informations sur les images contenues dans un fichier image Windows (. wim).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ImageFile :\<chemin d’accès du fichier WIM>|Spécifie le chemin d’accès complet et le nom de fichier du fichier. wim.|
|/Detailed|Retourne toutes les métadonnées d’image de chaque image. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|

## <a name="examples"></a>Exemples

Pour afficher des informations sur une image, tapez :
```
WDSUTIL /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Pour afficher des informations détaillées, tapez :
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)