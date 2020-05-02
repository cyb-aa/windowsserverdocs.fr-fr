---
title: bitsadmin peercaching and getconfigurationflags
description: Rubrique de référence pour la commande Bitsadmin et getconfigurationflags, qui obtient les indicateurs de configuration qui déterminent si l’ordinateur fournit du contenu aux pairs et s’il peut télécharger du contenu à partir de pairs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62b6848dec30a9a9fef401b1b2372605dbb9934a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717326"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin peercaching and getconfigurationflags

Obtient les indicateurs de configuration qui déterminent si l’ordinateur fournit du contenu aux pairs et s’il peut télécharger du contenu à partir de pairs.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour obtenir les indicateurs de configuration pour le travail nommé *myDownloadJob*:

```
bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)

- [commande Bitsadmin de la surcache](bitsadmin-peercaching.md)
