---
title: bitsadmin setminretrydelay
description: La rubrique commandes Windows pour **Bitsadmin setminretrydelay**, qui définit la durée minimale, en secondes, pendant laquelle bits attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddae9a62a49ca07bb03649f131a0a1ebad8ee3fe
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122875"
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
| le travail | Nom complet ou GUID du travail. |
| retrydelay | Durée minimale d’attente des BITS après une erreur pendant le transfert, en secondes. |

## <a name="examples"></a>Exemples

L’exemple suivant définit le délai minimal entre deux tentatives pour la tâche nommée *myDownloadJob* à 35 secondes.

```
C:\>bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)