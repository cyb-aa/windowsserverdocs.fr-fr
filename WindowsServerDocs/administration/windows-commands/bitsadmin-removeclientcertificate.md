---
title: bitsadmin removeclientcertificate
description: Rubrique de référence pour la commande Bitsadmin removeclientcertificate, qui supprime le certificat client du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 513830f6048f78aa528fa22cb590571e718452c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717069"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

Supprime le certificat client du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour supprimer le certificat client du travail nommé *myDownloadJob*:

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
