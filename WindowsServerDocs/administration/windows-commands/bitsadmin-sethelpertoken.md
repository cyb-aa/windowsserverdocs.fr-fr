---
title: Bitsadmin sethelpertoken
description: Rubrique relative aux commandes Windows pour **Bitsadmin sethelpertoken** -définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme jeton d’assistance d’une tâche de transfert bits.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 91c03366998168dad9ab4530ef36a5020b8ad6ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380569"
---
# <a name="bitsadmin-sethelpertoken"></a>Bitsadmin sethelpertoken

Définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme [jeton d’assistance](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)d’une tâche de transfert bits.

**BITS 3,0 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail.|
|\<username@domain\> mot de passe \<\>|Facultatif&mdash;les informations d’identification d’un compte d’utilisateur local dont le jeton doit être utilisé.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
