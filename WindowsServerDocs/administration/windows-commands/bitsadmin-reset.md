---
title: bitsadmin reset
description: La rubrique commandes Windows pour **Bitsadmin Reset** -annule tous les travaux de la file d’attente de transfert détenus par l’utilisateur actuel.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: adc6b07a7b5d1414c733fe6a3ac05eba7cb3029e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380808"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annule tous les travaux de la file d’attente de transfert détenus par l’utilisateur actuel.

**BITSAdmin 1,5 et versions antérieures**: Si vous disposez de privilèges d’administrateur, **Réinitialisez** cancels tous les travaux dans la file d’attente. L’option/AllUsers n’est pas prise en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|AllUsers|Facultatif : annule tous les travaux de la file d’attente.|

## <a name="remarks"></a>Notes

Vous devez disposer de privilèges d’administrateur pour utiliser le paramètre **ALLUSERS** .

> [!NOTE]
> Les administrateurs ne peuvent pas réinitialiser les travaux créés par le système local. Utilisez le planificateur de tâches pour planifier cette commande en tant que tâche à l’aide des informations d’identification du système local.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant annule tous les travaux de la file d’attente de transfert pour l’utilisateur actuel.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)