---
title: bitsadmin getnotifyinterface
description: La rubrique commandes Windows pour **Bitsadmin getnotifyinterface**, qui détermine si un autre programme a inscrit une interface de rappel com pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40bf9dd8-b167-406a-80a6-a5a6f1b8cf7f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5eb5aee42446c70f16fd6785a3645f42c1987e4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850572"
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
| le travail | Nom complet ou GUID du travail. |

#### <a name="output"></a>Sortie

La sortie de cette commande affiche, **inscrite** ou **désinscrite**.

> [!NOTE]
> Il n’est pas possible de déterminer le programme qui a inscrit l’interface de rappel.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère l’interface Notify pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getnotifyinterface myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)