---
title: ktmutil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7af47ab8697345b81018c2539e0c451359bd2a2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826460"
---
# <a name="ktmutil"></a>ktmutil



Lance l’utilitaire Gestionnaire de transactions du noyau. Si utilisée sans paramètres, **ktmutil** affiche des sous-commandes disponibles.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

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

## <a name="parameters"></a>Paramètres

## <a name="remarks"></a>Notes

## <a name="BKMK_examples"></a>Exemples

Pour forcer une transaction Indoubt avec 311a9209-03f4-11dc-918f-00188b8f707b GUID pour valider, tapez :
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)