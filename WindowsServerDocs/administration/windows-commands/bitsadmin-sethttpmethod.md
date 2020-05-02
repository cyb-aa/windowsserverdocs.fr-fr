---
title: bitsadmin sethttpmethod
description: Rubrique de référence pour la commande Bitsadmin sethttpmethod, qui définit le verbe HTTP à utiliser.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: daf1c23565bc4f398fd29e51aaaeef23b3b0d018
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719357"
---
# <a name="bitsadmin-sethttpmethod"></a>bitsadmin sethttpmethod

Définit le verbe HTTP à utiliser.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /sethttpmethod <job> <httpmethod>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| HttpMethod | Verbe HTTP à utiliser. Pour plus d’informations sur les verbes disponibles, consultez [définitions de méthode](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
