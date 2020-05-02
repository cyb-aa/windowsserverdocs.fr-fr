---
title: bitsadmin resume
description: Rubrique de référence pour la commande Bitsadmin Resume, qui active un travail nouveau ou suspendu dans la file d’attente de transfert.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba4cd57ddeeb3c35ca0871c2953fd409ddb57e73
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716992"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Active une tâche nouvelle ou suspendue dans la file d’attente de transfert. Si vous avez redémarré votre travail par erreur ou si vous avez simplement besoin de suspendre votre travail, vous pouvez utiliser le commutateur de [suspension Bitsadmin](bitsadmin-suspend.md) pour interrompre le travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour reprendre la tâche nommée *myDownloadJob*:

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin suspend](bitsadmin-suspend.md)

- [commande Bitsadmin](bitsadmin.md)
