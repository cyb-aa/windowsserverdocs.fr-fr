---
title: bitsadmin setvalidationstate
description: Rubrique de référence pour la commande Bitsadmin setvalidationstate, qui définit l’état de validation du contenu du fichier donné au sein du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3f22dc09eb1f70ce3c1ebd80fd6ba721e864377
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720461"
---
# <a name="bitsadmin-setvalidationstate"></a>bitsadmin setvalidationstate

Définit l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| Travail | Nom complet ou GUID du travail. |
| file_index | Commence à 0. |
| TRUE ou FALSe | **True** active la validation du contenu pour le fichier spécifié, tandis que **false** le désactive. |

## <a name="examples"></a>Exemples

Pour définir l’état de validation du contenu du fichier 2 sur TRUE pour le travail nommé *myDownloadJob*:

```
bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
