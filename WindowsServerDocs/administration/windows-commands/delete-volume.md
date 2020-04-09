---
title: delete volume
description: La rubrique commandes Windows pour supprimer le volume, qui supprime le volume sélectionné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2e958785c278306563999b09c1fecc0fdfa7ecb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846552"
---
# <a name="delete-volume"></a>delete volume

Supprime le volume sélectionné.

## <a name="syntax"></a>Syntaxe

```
delete volume [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="remarks"></a>Notes

-   Vous ne pouvez pas supprimer le volume système, le volume de démarrage ou un volume qui contient le fichier de pagination ou le vidage sur incident (vidage mémoire) actif.
-   Vous devez sélectionner un volume pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour supprimer le volume qui a le focus, tapez :
```
delete volume
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

