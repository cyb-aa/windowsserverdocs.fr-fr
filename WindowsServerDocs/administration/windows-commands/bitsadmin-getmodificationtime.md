---
title: bitsadmin getmodificationtime
description: La rubrique commandes Windows pour **Bitsadmin getmodificationtime**, qui récupère l’heure de la dernière modification du travail ou le transfert des données a réussi.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ace0f64b1fbe7ba72174bb3df2bd4dd65e929769
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850612"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

Récupère la dernière fois que le travail a été modifié ou que les données ont été transférées avec succès.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère l’heure de la dernière modification de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)