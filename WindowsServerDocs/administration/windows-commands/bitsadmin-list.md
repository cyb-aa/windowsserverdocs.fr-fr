---
title: bitsadmin list
description: Rubrique relative aux commandes Windows pour la **liste Bitsadmin** -répertorie les tâches de transfert détenues par l’utilisateur actuel.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bd4787f51dc2a7843ff6cf5c4f786658e530ad8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381093"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Répertorie les tâches de transfert détenues par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ALLUSERS|Facultatif : répertorie les travaux de tous les utilisateurs|
|/Verbose|Facultatif : fournit des informations détaillées pour chaque tâche.|

## <a name="remarks"></a>Notes

Vous devez disposer de privilèges d’administrateur pour utiliser le paramètre/ALLUSERS

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère des informations sur les travaux détenus par l’utilisateur actuel.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)