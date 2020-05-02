---
title: bitsadmin info
description: Rubrique de référence pour la commande Bitsadmin info, qui affiche des informations récapitulatives sur le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 904f70c82ab4bcc4fb25f759898674cc719b1954
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717437"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Affiche des informations récapitulatives sur le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| /verbose | facultatif. Fournit des informations détaillées sur chaque travail. |

## <a name="examples"></a>Exemples

Pour récupérer des informations sur le travail nommé *myDownloadJob*:

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)
