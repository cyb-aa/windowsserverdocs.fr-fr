---
title: bitsadmin getnotifyinterface
description: Rubrique de référence pour la commande Bitsadmin getnotifyinterface, qui détermine si un autre programme a inscrit une interface de rappel COM pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2158759067010292ca213f97014857354247b9c7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717738"
---
# <a name="bitsadmin-getnotifyinterface"></a>bitsadmin getnotifyinterface

Détermine si un autre programme a inscrit une interface de rappel COM (l’interface de notification) pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getnotifyinterface <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

#### <a name="output"></a>Output

La sortie de cette commande affiche, **inscrite** ou **désinscrite**.

> [!NOTE]
> Il n’est pas possible de déterminer le programme qui a inscrit l’interface de rappel.

## <a name="examples"></a>Exemples

Pour récupérer l’interface Notify pour le travail nommé *myDownloadJob*:

```
bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
