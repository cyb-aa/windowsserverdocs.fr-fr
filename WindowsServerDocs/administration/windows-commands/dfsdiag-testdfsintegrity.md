---
title: Dfsdiag testdfsintegrity
description: Rubrique de référence pour la commande Dfsdiag testdfsintegrity, qui vérifie l’intégrité de l’espace de noms système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 173ee832-26e1-4ec8-a23a-38a7d6229ac3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b54c7f597926abc91bb9201dfec1a04f44e04ecb
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992961"
---
# <a name="dfsdiag-testdfsintegrity"></a>Dfsdiag testdfsintegrity

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie l’intégrité de l’espace de noms de l’système de fichiers DFS (DFS) en effectuant les tests suivants :

- Vérifie la corruption des métadonnées DFS ou les incohérences entre les contrôleurs de domaine.

- Valide la configuration de l’énumération basée sur l’accès pour garantir la cohérence entre les métadonnées DFS et le partage du serveur d’espaces de noms.

- Détecte le chevauchement de dossiers DFS (liens), de dossiers dupliqués et de dossiers avec des cibles de dossiers qui se chevauchent.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /testdfsintegrity /DFSroot: <DFS root path> [/recurse] [/full]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /DFSroot:`<DFS root path>` | Espace de noms DFS à diagnostiquer. |
| /recurse | Effectue le test, y compris les liens entre les espaces de noms. |
| /Full | Vérifie la cohérence du partage et des listes de contrôle d’accès NTFS, ainsi que la configuration côté client sur toutes les cibles de dossiers. Il vérifie également que la propriété Online est définie. |

## <a name="examples"></a>Exemples

Pour vérifier l’intégrité et la cohérence des espaces de noms DFS (système de fichiers DFS) dans *contoso. com\MyNamespace*, y compris les liaisons entre les liaisons, tapez :

```
dfsdiag /testdfsintegrity /DFSRoot:\contoso.com\MyNamespace /recurse /full
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Dfsdiag](dfsdiag.md)
