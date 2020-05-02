---
title: bitsadmin addfileset
description: Rubrique de référence pour la commande Bitsadmin ADDFILESET, qui ajoute un ou plusieurs fichiers au travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d610c1330818cf820923b6d4f2e3555dc477444b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718480"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Ajoute un ou plusieurs fichiers au travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /addfileset <job> <textfile>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| TextFile | Un fichier texte, chaque ligne contenant un nom de fichier distant et un nom de fichier local. **Remarque :** Les noms doivent être délimités par des espaces. Les lignes commençant par `#` un caractère sont traitées comme un commentaire. |

## <a name="examples"></a>Exemples

```
bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
