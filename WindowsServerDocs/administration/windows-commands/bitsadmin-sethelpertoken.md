---
title: Bitsadmin sethelpertoken
description: La rubrique commandes Windows pour **Bitsadmin sethelpertoken**, qui définit le jeton principal de l’invite de commandes en cours (ou le jeton d’un compte d’utilisateur local arbitraire, s’il est spécifié) comme jeton d’assistance d’une tâche de transfert bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: ba4b9a4ed1b59d1b1aeda30353317739b7fdfa9e
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122981"
---
# <a name="bitsadmin-sethelpertoken"></a>Bitsadmin sethelpertoken

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
| le travail | Nom complet ou GUID du travail. |
| `<username@domain>` `<password>` | Ce paramètre est facultatif. Informations d’identification du compte d’utilisateur local pour le jeton à utiliser. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
