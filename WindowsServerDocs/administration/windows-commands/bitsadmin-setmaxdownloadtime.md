---
title: Bitsadmin setmaxdownloadtime
description: Rubrique relative aux commandes Windows pour **Bitsadmin setmaxdownloadtime**, qui définit le délai d’attente de téléchargement en secondes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f07931dfb9fabaec272384dced6d60f1335b6a94
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122914"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>Bitsadmin setmaxdownloadtime

Définit le délai d’attente de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| timeout | Longueur du délai d’attente du téléchargement, en secondes. |

## <a name="examples"></a>Exemples

L’exemple suivant définit le délai d’attente pour la tâche nommée *myDownloadJob* sur 10 secondes.

```
C:\>bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)