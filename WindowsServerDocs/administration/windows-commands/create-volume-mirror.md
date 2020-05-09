---
title: créer une mise en miroir du volume
description: Rubrique de référence pour la commande Create Volume Mirror, qui crée un miroir de volume à l’aide des deux disques dynamiques spécifiés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: be6e4496876636351b6e0853626a9ff9bb421f18
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993247"
---
# <a name="create-volume-mirror"></a>créer une mise en miroir du volume

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un miroir de volume à l’aide des deux disques dynamiques spécifiés. Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.

## <a name="syntax"></a>Syntaxe

```
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| taille =`<n>` | Spécifie la quantité d’espace disque, en mégaoctets (Mo), que le volume occupera sur chaque disque. Si aucune taille n’est donnée, le nouveau volume occupe l’espace libre restant sur le disque le plus petit et une quantité égale d’espace sur chaque disque suivant. |
| Disk =`<n>`,`<n>`[`,<n>,...`] | Spécifie les disques dynamiques sur lesquels le volume miroir est créé. Vous avez besoin de deux disques dynamiques pour créer un volume miroir. Une quantité d’espace égale à la taille spécifiée avec le paramètre de **taille** est allouée sur chaque disque. |
| aligner =`<n>` | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Ce paramètre est généralement utilisé avec les tableaux d’unités logiques RAID matériels pour améliorer les performances. `<n>`nombre de kilo-octets (Ko) entre le début du disque et la limite d’alignement la plus proche. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec une erreur. |

## <a name="examples"></a>Exemples

Pour créer un volume en miroir d’une taille de 1000 mégaoctets, sur les disques 1 et 2, tapez :

```
create volume mirror size=1000 disk=1,2
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [créer une commande](create.md)
