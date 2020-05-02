---
title: bitsadmin setmaxdownloadtime
description: Rubrique de référence pour la commande Bitsadmin setmaxdownloadtime, qui définit le délai d’attente de téléchargement en secondes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 16b96cf1-5738-415c-9b9d-c4ea8d5e4fec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8192826570c9dae6aa9d286596336c3e589c9cbd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719689"
---
# <a name="bitsadmin-setmaxdownloadtime"></a>bitsadmin setmaxdownloadtime

Définit le délai d’attente de téléchargement en secondes.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setmaxdownloadtime <job> <timeout>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| délai d'expiration | Longueur du délai d’attente du téléchargement, en secondes. |

## <a name="examples"></a>Exemples

Pour définir le délai d’expiration de la tâche nommée *myDownloadJob* sur 10 secondes.

```
bitsadmin /setmaxdownloadtime myDownloadJob 10
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
