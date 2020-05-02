---
title: bitsadmin setminretrydelay
description: Rubrique de référence pour la commande Bitsadmin setminretrydelay, qui définit la durée minimale, en secondes, pendant laquelle BITS attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fc54b4466d8f0bac12bd42ebf6c5e2c66087a15
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720121"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Définit la durée minimale, en secondes, pendant laquelle BITS attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| retrydelay | Durée minimale d’attente des BITS après une erreur pendant le transfert, en secondes. |

## <a name="examples"></a>Exemples

Pour définir le délai minimal entre deux tentatives sur 35 secondes pour le travail nommé *myDownloadJob*:

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
