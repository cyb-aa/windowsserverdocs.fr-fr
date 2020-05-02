---
title: chcp
description: Rubrique de référence pour la commande CHCP, qui modifie la page de codes active de la console.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0f1291176ed5245b06c68491f0d5cb0ae9b0b600
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82715328"
---
# <a name="chcp"></a>chcp

Modifie la page de codes active de la console. En cas d’utilisation sans paramètre, **chcp** affiche le numéro de la page de codes active de la console.

## <a name="syntax"></a>Syntaxe

```
chcp [<nnn>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<nnn>` | Spécifie la page de codes. |
| /? | Affiche l'aide à l'invite de commandes. |

Le tableau suivant répertorie chaque page de codes prise en charge et son pays/région ou langue :

| Page de codes | Pays/région ou langue |
| --------- | -------------------------- |
| 437 | États-Unis |
| 850 | Multilingue (I latin) |
| 852 | Slave (latin II) |
| 855 | Cyrillique (russe) |
| 857 | Turc |
| 860 | Portugais |
| 861 | Islandais |
| 863 | Canada-français |
| 865 | Nordique |
| 866 | Russe |
| 869 | Grec moderne |
| 936 | Chinois |

#### <a name="remarks"></a>Notes 

- Seule la page de codes OEM (Original Equipment Manufacturer) installée avec Windows s’affiche correctement dans une fenêtre d’invite de commandes qui utilise des polices Raster. D’autres pages de codes s’affichent correctement en mode plein écran ou dans les fenêtres d’invite de commandes qui utilisent des polices TrueType.

- Vous n’avez pas besoin de préparer les pages de codes (comme dans MS-DOS).

- Les programmes que vous démarrez après avoir affecté une nouvelle page de codes utilisent la nouvelle page de codes. Toutefois, les programmes (sauf cmd. exe) que vous avez démarré avant d’assigner la nouvelle page de codes continuent d’utiliser la page de codes d’origine.

## <a name="examples"></a>Exemples

Pour afficher le paramètre de page de codes active, tapez :

```
chcp
```

Un message semblable au suivant s’affiche :`Active code page: 437`

Pour modifier la page de codes active en 850 (multilingue), tapez :

```
chcp 850
```

Si la page de codes spécifiée n’est pas valide, le message d’erreur suivant s’affiche :`Invalid code page`

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
