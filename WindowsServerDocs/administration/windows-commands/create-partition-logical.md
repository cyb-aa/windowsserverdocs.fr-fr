---
title: créer une partition logique
description: Rubrique de référence pour la commande CREATE partition Logical, qui crée une partition logique dans une partition étendue existante.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99b8c837fe5da295087f9b146bf429d91ebc8693
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993265"
---
# <a name="create-partition-logical"></a>créer une partition logique

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition logique sur une partition étendue existante. Une fois la partition créée, le focus se déplace automatiquement vers la nouvelle partition.

>[!IMPORTANT]
> Vous pouvez utiliser cette commande uniquement sur les disques de l’enregistrement de démarrage principal (MBR). Vous devez utiliser la commande [Sélectionner le disque](select-disk.md) pour sélectionner un disque MBR de base et lui déplacer le focus.
>
> Vous devez créer une [partition étendue](create-partition-extended.md) avant de pouvoir créer des lecteurs logiques.

## <a name="syntax"></a>Syntaxe

```
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| taille =`<n>` | Spécifie la taille de la partition logique en mégaoctets (Mo), qui doit être plus petite que la partition étendue. Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la partition étendue. |
| décalage =`<n>` | Spécifie le décalage en kilo-octets (Ko), à partir duquel la partition est créée. Le décalage est arrondi pour remplir complètement la taille de cylindre utilisée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir. La partition est au moins aussi longue en octets que le nombre spécifié par **size =`<n>`**. Si vous spécifiez une taille pour la partition logique, celle-ci doit être plus petite que la partition étendue. |
| aligner =`<n>` | Aligne toutes les étendues de volume ou de partition sur la limite d’alignement la plus proche. Généralement utilisé avec les tableaux d’unités logiques RAID matériels pour améliorer les performances. `<n>`nombre de kilo-octets (Ko) entre le début du disque et la limite d’alignement la plus proche. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

#### <a name="remarks"></a>Notes 

- Si les paramètres **taille** et **décalage** ne sont pas spécifiés, la partition logique est créée dans la plus grande étendue de disque disponible dans la partition étendue.

## <a name="examples"></a>Exemples

Pour créer une partition logique d’une taille de 1000 mégaoctets, dans la partition étendue du disque sélectionné, tapez :

```
create partition logical size=1000
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [créer une commande](create.md)

- [select disk](select-disk.md)
