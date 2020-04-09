---
title: clip
description: La rubrique commandes Windows pour clip, qui redirige la sortie de commande de la ligne de commande vers le presse-papiers Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d997154a382cf39aa2b877d7a2b84f4ff34157d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847642"
---
# <a name="clip"></a>clip

Redirige la sortie de commande de la ligne de commande vers le presse-papiers Windows. Vous pouvez ensuite coller cette sortie de texte dans d’autres programmes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
<Command> | clip
clip < <FileName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|> de commande \<|Spécifie une commande dont vous souhaitez envoyer la sortie dans le presse-papiers Windows.|
|Nom de fichier \<>|Spécifie un fichier dont vous souhaitez envoyer le contenu dans le presse-papiers Windows.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Vous pouvez utiliser la commande **clip** pour copier des données directement dans une application qui peut recevoir du texte à partir du presse-papiers.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour copier la liste de répertoires active dans le presse-papiers Windows, tapez :
```
dir | clip
```
Pour copier la sortie d’un programme appelé Generic. awk dans le presse-papiers Windows, tapez :
```
awk -f generic.awk input.txt | clip
```
Pour copier le contenu d’un fichier nommé Readme. txt dans le presse-papiers Windows, tapez :
```
clip < readme.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)