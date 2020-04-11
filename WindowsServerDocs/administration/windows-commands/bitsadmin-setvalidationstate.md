---
title: Bitsadmin setvalidationstate
description: La rubrique commandes Windows pour **Bitsadmin setvalidationstate**, qui définit l’état de validation du contenu du fichier donné au sein du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bec42ae926050cd21df594a38f1c441a40a527f
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122718"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate

Définit l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setvalidationstate <job> <file_index> <TRUE|FALSE>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ---------- |
| Tâche | Nom complet ou GUID du travail. |
| file_index | Commence à 0. |
| TRUE ou FALSe | **True** active la validation du contenu pour le fichier spécifié, tandis que **false** le désactive. |

## <a name="examples"></a>Exemples

L’exemple suivant définit l’état de validation du contenu du fichier 2 sur TRUE pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /setvalidationstate myDownloadJob 2 TRUE
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)