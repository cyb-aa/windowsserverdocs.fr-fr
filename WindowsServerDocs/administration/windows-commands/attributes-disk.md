---
title: disque d’attributs
description: La rubrique commandes Windows pour le **disque des attributs**, qui affiche, définit ou efface les attributs d’un disque.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3c29a009a1efdfb7fed3d04d194cc8cfd2ea4eb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851242"
---
# <a name="attributes-disk"></a>disque d’attributs

Affiche, définit ou efface les attributs d’un disque.

## <a name="syntax"></a>Syntaxe

```
attributes disk [{set | clear}] [readonly] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| set | Définit l’attribut spécifié du disque avec le focus. |
| non activée | Efface l’attribut spécifié du disque qui a le focus. |
| readonly | Spécifie que le disque est en lecture seule. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="remarks"></a>Notes

-   Lorsque le **disque des attributs** est utilisé pour afficher les attributs actuels d’un disque, l’attribut disque de démarrage indique le disque utilisé pour démarrer l’ordinateur. Pour un miroir dynamique, il est affiché pour le disque qui contient le plex de démarrage du volume de démarrage.

-   Pour que la commande de disque des **attributs** aboutisse, vous devez sélectionner un disque. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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