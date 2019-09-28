---
title: Utilisation de la commande d’extraction de image
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6c8136585e04caca02ab16c7b4ca11a825cf400d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392135"
---
# <a name="using-the-get-imagefile-command"></a>Utilisation de la commande d’extraction de image



Récupère des informations sur les images contenues dans un fichier image Windows (. wim).

## <a name="syntax"></a>Syntaxe

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ImageFile : chemin d’accès du fichier \<WIM >|Spécifie le chemin d’accès complet et le nom de fichier du fichier. wim.|
|/Detailed|Retourne toutes les métadonnées d’image de chaque image. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|

## <a name="BKMK_examples"></a>Illustre

Pour afficher des informations sur une image, tapez :
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
Pour afficher des informations détaillées, tapez :
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)