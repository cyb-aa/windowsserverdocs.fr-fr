---
title: Bitsadmin sethelpertoken
description: La rubrique commandes Windows pour Bitsadmin sethelpertoken, qui définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme jeton d’assistance d’une tâche de transfert BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a1e8fd0054cadf3bf06b6e5b7bdf5010b18781e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849532"
---
# <a name="bitsadmin-sethelpertoken"></a>Bitsadmin sethelpertoken

Définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme [jeton d’assistance](/windows/desktop/bits/helper-tokens-for-bits-transfer-jobs)d’une tâche de transfert bits.

**BITS 3,0 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetHelperToken <Job> [\<username@domain\> \<password\>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail.|
|\<username@domain\> mot de passe \<\>|Facultatif&mdash;les informations d’identification d’un compte d’utilisateur local dont le jeton doit être utilisé.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
