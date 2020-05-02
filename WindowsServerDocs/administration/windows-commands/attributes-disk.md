---
title: disque d’attributs
description: Rubrique de référence sur la commande attributs Disk, qui affiche, définit ou efface les attributs d’un disque.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3d378439b30328e4df48020fa4b3288f7af31c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718897"
---
# <a name="attributes-disk"></a>disque d’attributs

Affiche, définit ou efface les attributs d’un disque. Lorsque cette commande est utilisée pour afficher les attributs actuels d’un disque, l’attribut disque de démarrage indique le disque utilisé pour démarrer l’ordinateur. Pour un miroir dynamique, il affiche le disque qui contient le plex de démarrage du volume de démarrage.

> [!IMPORTANT]
> Pour que la commande de disque des **attributs** aboutisse, vous devez sélectionner un disque. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="syntax"></a>Syntaxe

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| set | Définit l’attribut spécifié du disque avec le focus. |
| clear | Efface l’attribut spécifié du disque qui a le focus. |
| readonly | Spécifie que le disque est en lecture seule. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour afficher les attributs du disque sélectionné, tapez :

```
attributes disk
```

Pour définir le disque sélectionné en lecture seule, tapez :

```
attributes disk set readonly
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [sélectionner le disque, commande](select-disk.md)
