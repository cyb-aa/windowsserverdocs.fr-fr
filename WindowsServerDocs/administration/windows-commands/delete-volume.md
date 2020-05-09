---
title: supprimer le volume
description: Rubrique de référence pour la commande Delete Volume, qui supprime le volume sélectionné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59856e89ff96d2881040365d157540dc62c1aeb0
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993095"
---
# <a name="delete-volume"></a>delete volume

Supprime le volume sélectionné. Avant de commencer, vous devez sélectionner un volume pour que cette opération aboutisse. Utilisez la commande [Sélectionner un volume](select-volume.md) pour sélectionner un volume et lui déplacer le focus.

> [!IMPORTANT]
> Vous ne pouvez pas supprimer le volume système, le volume de démarrage ou tout volume qui contient le fichier d’échange actif ou le vidage sur incident (vidage de la mémoire).

## <a name="syntax"></a>Syntaxe

```
delete volume [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour supprimer le volume qui a le focus, tapez :

```
delete volume
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [select volume](select-volume.md)

- [commande Delete](delete.md)