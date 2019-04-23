---
title: Bitsadmin sethelpertoken
description: Rubrique de commandes de Windows pour **bitsadmin sethelpertoken** -définit le jeton principal de l’invite de commandes actuel (ou un jeton d’un compte d’utilisateur local arbitraire, si spécifié) en tant que jeton de d’assistance d’un travail de transfert BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 558a1aca66a7b3ec447136ceff9237d13efe4ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853000"
---
# <a name="bitsadmin-sethelpertoken"></a>Bitsadmin sethelpertoken

Définit le jeton principal de l’invite de commandes actuel (ou un jeton d’un compte d’utilisateur local arbitraire, si spécifié) en tant qu’une tâche de transfert BITS [jeton d’application d’assistance](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs).

**BITS 3.0 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail.|
|\<username@domain\> \<password\>|Facultatif&mdash;les informations d’identification d’un utilisateur local compte dont le jeton à utiliser.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
