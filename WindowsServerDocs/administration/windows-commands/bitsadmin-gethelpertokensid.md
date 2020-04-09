---
title: Bitsadmin gethelpertokensid
description: La rubrique commandes Windows pour **Bitsadmin gethelpertokensid**, qui retourne le SID d’un jeton d’assistance d’une tâche de transfert bits, si elle est définie.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a2e26ff459b068595529fbd24e6165c130660570
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850642"
---
# <a name="bitsadmin-gethelpertokensid"></a>Bitsadmin gethelpertokensid

Retourne le SID du [jeton d’assistance](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)d’une tâche de transfert bits, si celui-ci est défini.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 3,0 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
