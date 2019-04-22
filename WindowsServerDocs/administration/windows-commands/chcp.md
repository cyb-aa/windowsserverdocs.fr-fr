---
title: chcp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 622d4b64128c7e39cc761e4f5e9d69cf54383760
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819200"
---
# <a name="chcp"></a>chcp



Modifie la page de codes active de la console. Si utilisée sans paramètres, **chcp** affiche le nombre de la page de codes active de la console.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
chcp [<NNN>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<NNN>|Spécifie la page de codes.|
|/?|Affiche l'aide à l'invite de commandes.|

Le tableau suivant répertorie chaque code pris en charge de page et son pays/région ou la langue :

|Page de codes|Pays/région ou la langue|
|---------|--------------------------|
|437|États-Unis|
|850|Multilingue (Latin I)|
|852|Slave (Latin II)|
|855|Cyrillique (russe)|
|857|Turc|
|860|Portugais|
|861|Islandais|
|863|Français (Canada)|
|865|Europe du Nord|
|866|Russe|
|869|Grec moderne|
|936|Chinois|

## <a name="remarks"></a>Notes

-   Uniquement la page de codes OEM (OEM) est installée avec Windows s’affiche correctement dans une fenêtre d’invite de commandes qui utilise les polices Raster. Autres pages de codes s’affichent correctement dans le mode plein écran ou dans les fenêtres d’invite de commandes qui utilisent des polices TrueType.
-   Vous n’avez pas besoin préparer les pages de codes (comme dans MS-DOS).
-   Les programmes que vous démarrez après vous attribuer une nouvelle page de codes utilisent la nouvelle page de code. Toutefois, (sauf Cmd.exe), les programmes que vous démarrez avant d’affectent à la page Nouveau code, utilisez la page de codes d’origine.

## <a name="BKMK_examples"></a>Exemples

Pour afficher le paramètre de la page de codes active, tapez :
```
chcp
```
Un message semblable au suivant s’affiche :

`Active code page: 437`

Pour modifier la page de codes active 850 (multilingue), tapez :
```
chcp 850
```
Si la page de codes spécifiée n’est pas valide, le message d’erreur suivant s’affiche :

`Invalid code page`

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
