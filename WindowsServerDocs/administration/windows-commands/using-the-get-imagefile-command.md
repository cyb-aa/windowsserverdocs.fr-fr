---
title: Obtient-image
description: Rubrique relative aux commandes Windows pour la commande obtenir-image, qui récupère des informations sur les images contenues dans un fichier image Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef1cf2b9eec6739690d286c32d26dd84b07e348c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830992"
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
|/ImageFile :\<chemin d’accès du fichier WIM >|Spécifie le chemin d’accès complet et le nom de fichier du fichier. wim.|
|/Detailed|Retourne toutes les métadonnées d’image de chaque image. Si cette option n’est pas utilisée, le comportement par défaut consiste à retourner uniquement le nom de l’image, la description et le nom du fichier.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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