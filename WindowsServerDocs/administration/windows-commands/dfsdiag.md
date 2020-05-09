---
title: Dfsdiag
description: Rubrique de référence pour la commande Dfsdiag, qui fournit des informations de diagnostic pour les espaces de noms DFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7e9e0de18b48a4233b950ad6aa8f1e450a99da62
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992825"
---
# <a name="dfsdiag"></a>Dfsdiag

Fournit des informations de diagnostic pour les espaces de noms DFS.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /testdcs [/domain:<domain name>]
dfsdiag /testsites </machine:<server name>| /DFSPath:<namespace root or DFS folder> [/recurse]> [/full]
dfsdiag /testdfsconfig /DFSRoot:<namespace>
dfsdiag /testdfsintegrity /DFSRoot:<DFS root path> [/recurse] [/full]
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| [Dfsdiag testdcs](dfsdiag-testdcs.md) | Vérifie la configuration du contrôleur de domaine. |
| [Dfsdiag testsites](dfsdiag-testsites.md) | Vérifie les associations de sites. |
| [Dfsdiag testdfsconfig](dfsdiag-testdfsconfig.md) | Vérifie la configuration de l’espace de noms DFS. |
| [Dfsdiag testdfsintegrity](dfsdiag-testdfsintegrity.md) | Vérifie l’intégrité de l’espace de noms DFS. |
| [Dfsdiag testreferral](dfsdiag-testreferral.md) | Vérifie les réponses de référence. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
