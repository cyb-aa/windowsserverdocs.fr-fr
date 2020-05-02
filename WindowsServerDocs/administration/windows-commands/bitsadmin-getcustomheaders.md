---
title: bitsadmin getcustomheaders
description: Rubrique de référence pour la commande Bitsadmin getcustomheaders, qui récupère les en-têtes HTTP personnalisés du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fe839cd0e629af88b3ee3642abcce339442d03a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718099"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

Récupère les en-têtes HTTP personnalisés du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour obtenir les en-têtes personnalisés pour le travail nommé *myDownloadJob*:

```
bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
