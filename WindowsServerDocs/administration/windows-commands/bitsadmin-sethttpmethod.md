---
title: Bitsadmin sethttpmethod
description: La rubrique commandes Windows pour **Bitsadmin sethttpmethod**, qui définit le verbe http à utiliser.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: d349dcad7bdf6a6fc566ed961c3160836d7f49da
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122968"
---
# <a name="bitsadmin-sethttpmethod"></a>Bitsadmin sethttpmethod

Définit le verbe HTTP à utiliser.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| HttpMethod | Verbe HTTP à utiliser. Pour plus d’informations sur les verbes disponibles, consultez [définitions de méthode](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)