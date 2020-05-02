---
title: Dfsdiag TestDFSIntegrity
description: Rubrique de référence pour **Dfsdiag TestDFSIntegrity**, qui vérifie l’intégrité de l’espace de noms système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21aa6ef3d7d4a7b4a9c64fc51aec77f49f1e0a0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719575"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag TestDFSIntegrity

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie l’intégrité de l’espace de noms de l’système de fichiers DFS (DFS) en effectuant les tests suivants :

- Vérifie la corruption des métadonnées DFS ou les incohérences entre les contrôleurs de domaine.

- Valide la configuration de l’énumération basée sur l’accès pour garantir la cohérence entre les métadonnées DFS et le partage du serveur d’espaces de noms.

- Détecte le chevauchement de dossiers DFS (liens), de dossiers dupliqués et de dossiers avec des cibles de dossiers qui se chevauchent.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /TestDFSIntegrity /DFSRoot: <DFS root path> [/Recurse] [/Full]
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
|-------|--------|
| /DFSRoot:`<DFS root path>`| Espace de noms DFS à diagnostiquer. |
| /Recurse | Effectue les tests, y compris les liaisons d’espace de noms. |
| /Full | Vérifie la cohérence des ACL de partage et NTFS, ainsi que la configuration côté client sur toutes les cibles de dossiers. Il vérifie également que la propriété Online est définie. |

## <a name="examples"></a>Exemples

```
dfsdiag /TestDFSIntegrity /DFSRoot:\\Contoso.com\MyNamespace /Recurse /Full
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

-   [Dfsdiag](dfsdiag.md)


