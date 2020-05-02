---
title: ktmutil
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47b447165ee54e6839bb6338801c6703d818caa8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724525"
---
# <a name="ktmutil"></a>ktmutil



Démarre l’utilitaire du gestionnaire de transactions du noyau. S’il est utilisé sans paramètres, **ktmutil** affiche les sous-commandes disponibles.



## <a name="syntax"></a>Syntaxe

```
ktmutil list tms 
ktmutil list transactions [{TmGuid}]
ktmutil resolve complete {TmGuid} {RmGuid} {EnGuid}
ktmutil resolve commit {TxGuid}
ktmutil resolve rollback {TxGuid}
ktmutil force commit {??Guid}
ktmutil force rollback {??Guid}
ktmutil forget
```

### <a name="parameters"></a>Paramètres

## <a name="remarks"></a>Notes 

## <a name="examples"></a>Exemples

Pour forcer une transaction incertaine avec le GUID 311a9209-03f4-11dc-918f-00188b8f707b à valider, tapez :
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)