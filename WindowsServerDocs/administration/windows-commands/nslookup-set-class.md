---
title: nslookup set class
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1ae3a5336815a5273aafa976b1dcad8b60fac9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723656"
---
# <a name="nslookup-set-class"></a>nslookup set class



Modifie la classe de requête. La classe spécifie le groupe de protocoles des informations.

## <a name="syntax"></a>Syntaxe

```
set class=<Class>
```

### <a name="parameters"></a>Paramètres

| Paramètre |                                                                                                                                    Description                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<Class>  | La classe par défaut est dans. La liste suivante répertorie les valeurs valides pour cette commande.</br>-IN : spécifie la classe Internet.</br>-CHAOS : spécifie la classe chaos.</br>-HESIOD : spécifie la classe MIT Athena Hesiod.</br>-ANY : spécifie l’un des caractères génériques précédemment listés. |
|   {aide   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)