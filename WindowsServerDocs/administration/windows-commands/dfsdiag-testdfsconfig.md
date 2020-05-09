---
title: Dfsdiag testdfsconfig
description: Rubrique de référence pour le testdfsconfig Dfsdiag, qui vérifie la configuration d’un espace de noms système de fichiers DFS (DFS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 106aeeb9-ea79-4e6e-829c-eca06309bab2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9490f35c2d509c83d9008aa87627bd3c55a875
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992991"
---
# <a name="dfsdiag-testdfsconfig"></a>Dfsdiag testdfsconfig

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration d’un espace de noms système de fichiers DFS (DFS) en effectuant les actions suivantes :

- Vérifie que le service d’espace de noms DFS est en cours d’exécution et que son type de démarrage est défini sur **automatique** sur tous les serveurs d’espaces de noms.

- Vérifie que la configuration du Registre DFS est cohérente entre les serveurs d’espaces de noms.

- Valide les dépendances suivantes sur les serveurs d’espaces de noms en cluster :

  - Dépendance de la ressource racine de l’espace de noms sur la ressource de nom réseau.

  - Dépendance de ressource de nom de réseau sur la ressource d’adresse IP.

  - Dépendance de la ressource racine de l’espace de noms sur la ressource de disque physique.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /testdfsconfig /DFSroot:<namespace>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /DFSroot:`<namespace>` | Espace de noms (racine DFS) à diagnostiquer. |

## <a name="examples"></a>Exemples

Pour vérifier la configuration des espaces de noms système de fichiers DFS (DFS) dans *contoso. com\MyNamespace*, tapez :

```
dfsdiag /testdfsconfig /DFSroot:\contoso.com\MyNamespace
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Dfsdiag](dfsdiag.md)
