---
title: bitsadmin getminretrydelay
description: La rubrique commandes Windows pour **Bitsadmin getminretrydelay**, qui récupère la durée, en secondes, pendant laquelle le service attend après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d79ffdf1f45b0198b4af535ed83154c3c2ec24f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850622"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

Récupère la durée, en secondes, pendant laquelle le service attendra après avoir rencontré une erreur temporaire avant de tenter de transférer le fichier.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le délai minimal entre deux tentatives pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)