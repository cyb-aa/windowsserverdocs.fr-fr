---
title: bitsadmin geterror
description: La rubrique commandes Windows pour **Bitsadmin GetError**, qui récupère des informations d’erreur détaillées pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c65b072bb190e3e516b917c310942146bb3f3d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850702"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Récupère des informations d’erreur détaillées pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère les informations d’erreur pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)