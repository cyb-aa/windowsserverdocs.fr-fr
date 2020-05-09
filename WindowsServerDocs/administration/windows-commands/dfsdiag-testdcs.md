---
title: Dfsdiag testdcs
description: Rubrique de référence pour la commande Dfsdiag testdcs, qui vérifie la configuration des contrôleurs de domaine dans le domaine spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: abb915ab-23eb-45d7-9a2e-b6b9a5756a70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0bbe47474f99edb1626e61a372b02090d3a45ee3
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993000"
---
# <a name="dfsdiag-testdcs"></a>Dfsdiag testdcs

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vérifie la configuration des contrôleurs de domaine en effectuant les tests suivants sur chaque contrôleur de domaine dans le domaine spécifié :

- Vérifie que le service d’espace de noms système de fichiers DFS (DFS) est en cours d’exécution et que son type de démarrage est défini sur **automatique**.

- Vérifie la prise en charge des références de site à coût pour NETLOGon et SYSvol.

- Vérifie la cohérence de l’Association de sites par nom d’hôte et adresse IP.

## <a name="syntax"></a>Syntaxe

```
dfsdiag /testdcs [/domain:<domain_name>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /Domain`<domain_name>` | Nom du domaine à vérifier. Ce paramètre est facultatif. La valeur par défaut est le domaine local auquel l’hôte local est joint. |

## <a name="examples"></a>Exemples

Pour vérifier la configuration des contrôleurs de domaine dans le domaine *contoso.com* , tapez :

```
dfsdiag /testdcs /domain:contoso.com
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Dfsdiag](dfsdiag.md)
