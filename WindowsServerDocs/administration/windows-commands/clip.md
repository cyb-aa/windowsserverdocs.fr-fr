---
title: clip
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b10876e115c1f0dcac3448948003852449012087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862390"
---
# <a name="clip"></a>clip



Redirige la sortie de la ligne de commande dans le Presse-papiers Windows. Vous pouvez ensuite coller cette sortie de texte dans d’autres programmes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Command>|Spécifie une commande dont la sortie à envoyer dans le Presse-papiers Windows.|
|\<FileName>|Spécifie un fichier dont le contenu à envoyer dans le Presse-papiers Windows.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Vous pouvez utiliser la **clip** commande pour copier des données directement dans une application qui peut recevoir le texte à partir du Presse-papiers.

## <a name="BKMK_examples"></a>Exemples

Pour copier le listing actuels dans le Presse-papiers de Windows, tapez :
```
dir | clip
```
Pour copier la sortie d’un programme appelé Generic.awk dans le Presse-papiers de Windows, tapez :
```
awk -f generic.awk input.txt | clip
```
Pour copier le contenu d’un fichier appelé fichier Readme.txt dans le Presse-papiers de Windows, tapez :
```
clip < readme.txt
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)