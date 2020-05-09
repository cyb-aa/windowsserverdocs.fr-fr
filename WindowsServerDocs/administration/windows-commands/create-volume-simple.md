---
title: créer un volume simple
description: Rubrique de référence pour la commande CREATE volume simple, qui crée un volume simple sur le disque dynamique spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb5aeebbcbde581fe3259d6cb5aca6445a3b27aa
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993220"
---
# <a name="create-volume-simple"></a>créer un volume simple

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume simple sur le disque dynamique spécifié. Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.

## <a name="syntax"></a>Syntaxe

```
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| taille =`<n>`  | Taille du volume en mégaoctets (Mo). Si aucune taille n’est indiquée, le nouveau volume occupe l’espace libre restant sur le disque. |
| disque =`<n>`  | Disque dynamique sur lequel le volume est créé. Si aucun disque n’est spécifié, le disque actuel est utilisé. |
| aligner =`<n>` | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec les tableaux d’unités logiques RAID matériels pour améliorer les performances. `<n>`nombre de kilo-octets (Ko) entre le début du disque et la limite d’alignement la plus proche. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour créer un volume de 1000 mégaoctets de taille, sur le disque 1, tapez :

```
create volume simple size=1000 disk=1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [créer une commande](create.md)
