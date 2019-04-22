---
title: dfsdiag
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ab5c86ce7ed4760aef4941de55e8dcf8efe48c8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819140"
---
# <a name="dfsdiag"></a>dfsdiag



Le `Dfsdiag` commande fournit des informations de diagnostic pour les espaces de noms DFS.

## <a name="syntax"></a>Syntaxe

```
dfsdiag [ /TestDCs [/Domain:<Domain name>]| /TestSites </Machine:<server name>| /DFSPath:<namespace root or DFS folder> [/Recurse]> [/Full] | /TestDFSConfig /DFSRoot:<namespace> | /TestDFSIntegrity /DFSRoot:<DFS root path> [/Recurse] [/Full] | /TestReferral /DFSPath:<DFS path for getting referrals> [/Full] | /?] 

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Dfsdiag TestDCs](dfsdiag-testdcs.md)|Vérifie la configuration du contrôleur de domaine.|
|[Dfsdiag TestSites](dfsdiag-testsites.md)|Vérifications des associations de site.|
|[Dfsdiag TestDFSConfig](dfsdiag-testdfsconfig.md)|Vérifie la configuration de DFS Namespace.|
|[Dfsdiag TestDFSIntegrity](dfsdiag-testdfsintegrity.md)|Vérifie l’intégrité de DFS Namespace.|
|[Dfsdiag TestReferral](dfsdiag-testreferral.md)|Vérifie les réponses de redirection.|
|/?|Affiche l'aide à l'invite de commandes.|

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)