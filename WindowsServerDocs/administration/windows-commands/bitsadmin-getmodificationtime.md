---
title: bitsadmin getmodificationtime
description: Rubrique de référence pour la commande Bitsadmin getmodificationtime, qui récupère l’heure de la dernière modification du travail ou le transfert des données.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bab8c317917894a351c03df1efefb17842ecb7d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717837"
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
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer l’heure de dernière modification de la tâche nommée *myDownloadJob*:

```
bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
