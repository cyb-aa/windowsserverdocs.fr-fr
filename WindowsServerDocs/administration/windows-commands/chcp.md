---
title: chcp
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d5784b052ff1d7084d68cca0589caf518b8e44a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379527"
---
# <a name="chcp"></a>chcp



Modifie la page de codes active de la console. En cas d’utilisation sans paramètre, **chcp** affiche le numéro de la page de codes active de la console.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
chcp [<NNN>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@NO__T 0NNN >|Spécifie la page de codes.|
|/?|Affiche l'aide à l'invite de commandes.|

Le tableau suivant répertorie chaque page de codes prise en charge et son pays/région ou langue :

|Page de codes|Pays/région ou langue|
|---------|--------------------------|
|437|États-Unis|
|850|Multilingue (I latin)|
|852|Slave (latin II)|
|855|Cyrillique (russe)|
|857|Turc|
|860|Portugais|
|861|Islandais|
|863|Canada-français|
|865|Nordique|
|866|Russe|
|869|Grec moderne|
|936|Chinois|

## <a name="remarks"></a>Notes

-   Seule la page de codes OEM (Original Equipment Manufacturer) installée avec Windows s’affiche correctement dans une fenêtre d’invite de commandes qui utilise des polices Raster. D’autres pages de codes s’affichent correctement en mode plein écran ou dans les fenêtres d’invite de commandes qui utilisent des polices TrueType.
-   Vous n’avez pas besoin de préparer les pages de codes (comme dans MS-DOS).
-   Les programmes que vous démarrez après avoir affecté une nouvelle page de codes utilisent la nouvelle page de codes. Toutefois, les programmes (sauf cmd. exe) que vous démarrez avant d’affecter la nouvelle page de codes utilisent la page de codes d’origine.

## <a name="BKMK_examples"></a>Illustre

Pour afficher le paramètre de page de codes active, tapez :
```
chcp
```
Un message semblable au suivant s’affiche :

`Active code page: 437`

Pour modifier la page de codes active en 850 (multilingue), tapez :
```
chcp 850
```
Si la page de codes spécifiée n’est pas valide, le message d’erreur suivant s’affiche :

`Invalid code page`

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
