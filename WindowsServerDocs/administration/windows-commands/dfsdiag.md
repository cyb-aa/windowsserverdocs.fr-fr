---
title: Dfsdiag
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0891e67-0187-4f18-923d-5623e6127f90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61a6ab9a90e4d0220cfe27d2d21120be19b9ff1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378309"
---
# <a name="dfsdiag"></a>Dfsdiag



La commande `Dfsdiag` fournit des informations de diagnostic pour les espaces de noms DFS.

## <a name="syntax"></a>Syntaxe

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Vérifie la configuration du contrôleur de domaine.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Vérifie les associations de sites.|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Vérifie la configuration de l’espace de noms DFS.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Vérifie l’intégrité de l’espace de noms DFS.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Vérifie les réponses de référence.|
|/?|Affiche l'aide à l'invite de commandes.|

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)