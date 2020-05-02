---
title: bitsadmin complete
description: Rubrique de référence pour la commande Bitsadmin Complete, qui termine la tâche.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b61f3475afdb0e29e5777940e6426a04fe33e78
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718226"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Termine le travail. Utilisez ce commutateur après que le travail passe à l’état transféré. Dans le cas contraire, seuls les fichiers qui ont été transférés avec succès seront disponibles.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |

## <a name="example"></a> Exemple

Pour terminer la tâche *myDownloadJob* , une fois qu’elle `TRANSFERRED` a atteint l’État :

```
bitsadmin /complete myDownloadJob
```

Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez utiliser le GUID du travail pour l’identifier de manière unique pour l’achèvement.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
