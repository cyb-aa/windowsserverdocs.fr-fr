---
title: À l’aide de la commande get-fichier image
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bbe5ece95d1f9821a27b96e56bc34576a0f5f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827620"
---
# <a name="using-the-get-imagefile-command"></a>À l’aide de la commande get-fichier image



Récupère des informations sur les images contenues dans un fichier Image Windows (.wim).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ Fichier image :\<chemin d’accès du fichier WIM >|Spécifie le chemin d’accès et le nom complet du fichier .wim.|
|[/Detailed]|Retourne toutes les métadonnées d’image à partir de chaque image. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, description et nom de fichier.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher des informations sur une image, tapez :
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
Pour afficher des informations détaillées, tapez :
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)