---
title: bitsadmin getcompletiontime
description: Rubrique de référence pour la commande Bitsadmin getcompletiontime, qui récupère l’heure à laquelle le travail a terminé le transfert des données.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b3721401e450ae60fb77534f8eb845ff5ac3443
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718118"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Récupère l’heure à laquelle le travail a terminé le transfert des données.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer l’heure à laquelle le travail nommé *myDownloadJob* a terminé le transfert des données :

```
bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
