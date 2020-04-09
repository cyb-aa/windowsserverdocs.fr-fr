---
title: bitsadmin resume
description: La rubrique commandes Windows pour **Bitsadmin Resume**, qui active un travail nouveau ou suspendu dans la file d’attente de transfert.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a3f464ba00c5cc233c42a40c063372dc0d584e9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849752"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Active une tâche nouvelle ou suspendue dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant reprend la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)