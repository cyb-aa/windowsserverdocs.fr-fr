---
title: bitsadmin list
description: Rubrique de commandes de Windows pour **bitsadmin liste** -répertorie les tâches de transfert détenus par l’utilisateur actuel.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0b88001b9c4ae01b57006ffeef66dec0348ca77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873860"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Répertorie les tâches de transfert détenus par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ Allusers|Facultatif — répertorie les travaux pour tous les utilisateurs|
|/Verbose|Facultatif — fournit des informations détaillées pour chaque tâche.|

## <a name="remarks"></a>Notes

Vous devez disposer des privilèges d’administrateur pour utiliser le paramètre /allusers

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère des informations sur les travaux détenus par l’utilisateur actuel.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)