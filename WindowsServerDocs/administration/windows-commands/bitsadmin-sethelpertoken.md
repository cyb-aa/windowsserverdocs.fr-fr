---
title: bitsadmin sethelpertoken
description: Rubrique de référence pour la commande Bitsadmin sethelpertoken, qui définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme jeton d’assistance d’une tâche de transfert BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b125f95e262c2fd78f20266e3e2b6c80cea5a789
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719394"
---
# <a name="bitsadmin-sethelpertoken"></a>bitsadmin sethelpertoken

Définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme [jeton d’assistance](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)d’une tâche de transfert bits.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 3,0 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /sethelpertoken <job> [<user_name@domain> <password>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| `<username@domain>` `<password>` | facultatif. Informations d’identification du compte d’utilisateur local pour le jeton à utiliser. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
