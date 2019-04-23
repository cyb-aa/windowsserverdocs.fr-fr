---
title: bitsadmin reset
description: Rubrique de commandes de Windows pour **bitsadmin réinitialiser** -annule toutes les tâches dans la file d’attente de transfert détenus par l’utilisateur actuel.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b7c29aac55393cd87145583814b3ffa8f0a2c3b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874250"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annule tous les travaux dans la file d’attente de transfert détenus par l’utilisateur actuel.

**BITSAdmin 1.5 et versions antérieur**: Si vous disposez des privilèges d’administrateur, **réinitialiser** annule tous les travaux dans la file d’attente. L’option /AllUsers n’est pas pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Reset [/AllUsers]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|AllUsers|Facultatif : annule tous les travaux dans la file d’attente.|

## <a name="remarks"></a>Notes

Vous devez disposer des privilèges d’administrateur pour utiliser le **AllUsers** paramètre.

> [!NOTE]
> Les administrateurs ne peuvent pas réinitialiser les travaux créés par le système Local. Utilisez le Planificateur de tâches pour planifier cette commande en tant que tâche à l’aide des informations d’identification du système Local.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant annule toutes les tâches dans la file d’attente de transfert pour l’utilisateur actuel.
```
C:\>bitsadmin /Reset
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)