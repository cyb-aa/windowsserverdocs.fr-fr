---
title: clip
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82186869782c47f41930d46b4c33a710e6addedf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379338"
---
# <a name="clip"></a>clip



Redirige la sortie de commande de la ligne de commande vers le presse-papiers Windows. Vous pouvez ensuite coller cette sortie de texte dans d’autres programmes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|> de commande \<|Spécifie une commande dont vous souhaitez envoyer la sortie dans le presse-papiers Windows.|
|Nom de fichier \<>|Spécifie un fichier dont vous souhaitez envoyer le contenu dans le presse-papiers Windows.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Vous pouvez utiliser la commande **clip** pour copier des données directement dans une application qui peut recevoir du texte à partir du presse-papiers.

## <a name="BKMK_examples"></a>Illustre

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

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)