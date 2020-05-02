---
title: automount
description: Rubrique de référence pour la commande de montage automatique, qui active ou désactive la fonctionnalité de montage automatique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4635fc91-a477-4f17-8dcc-aa08854bfe45
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a3ff8782b2110dd1b8039477c0b748dc4ab8f44
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718727"
---
# <a name="automount"></a>automount

S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

> [!IMPORTANT]
> Dans les configurations de réseau de zone de stockage (SAN), la désactivation du montage automatique empêche Windows de monter automatiquement ou d’affecter des lettres de lecteur à tous les nouveaux volumes de base qui sont visibles par le système.

## <a name="syntax"></a>Syntaxe

montage automatique [{Enable | Disable | Scrub}] [noerr]

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| enable | Permet à Windows de monter automatiquement les nouveaux volumes de base et dynamiques qui sont ajoutés au système et de leur attribuer des lettres de lecteur. |
| disable | Empêche Windows de monter automatiquement les nouveaux volumes de base et dynamiques ajoutés au système.<p>**Remarque**: la désactivation du montage automatique peut provoquer l’échec des clusters de basculement dans la partie stockage de l’Assistant validation d’une configuration. |
| Balay | Supprime les répertoires de points de montage de volume et les paramètres de Registre pour les volumes qui ne sont plus dans le système. Cela empêche le montage automatique des volumes précédemment présents dans le système, ainsi que leurs anciens points de montage de volume lorsqu’ils sont rajoutés au système. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour voir si la fonctionnalité de montage automatique est activée, tapez les commandes suivantes à l’intérieur de la commande DiskPart :

```
automount
```

Pour activer la fonctionnalité de montage automatique, tapez :

```
automount enable
```

Pour désactiver la fonctionnalité de montage automatique, tapez :

```
automount disable
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commandes DiskPart](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc770877(v%3dws.11))
