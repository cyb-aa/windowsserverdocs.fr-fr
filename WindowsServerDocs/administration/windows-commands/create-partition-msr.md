---
title: créer une partition MSR
description: Rubrique de référence pour Create partition MSR, qui crée une partition MSR (Microsoft Reserved) sur un disque GPT (GUID partition table).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d427e9f96023f8b66f72e3895b30519ab7cd2de1
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993261"
---
# <a name="create-partition-msr"></a>créer une partition MSR

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition MSR (Microsoft Reserved) sur un disque GPT (GUID partition table). Une partition réservée Microsoft est requise sur chaque disque GPT. La taille de cette partition dépend de la taille totale du disque GPT. La taille du disque GPT doit être d’au moins 32 Mo pour créer une partition réservée Microsoft.

> [!IMPORTANT]
> Soyez très prudent lorsque vous utilisez cette commande. Étant donné que les disques GPT requièrent une disposition de partition spécifique, la création de partitions réservées Microsoft peut rendre le disque illisible.
>
> Vous devez sélectionner un disque GPT de base pour que cette opération aboutisse. Vous devez utiliser la commande [Sélectionner le disque](select-disk.md) pour sélectionner un disque GPT de base et lui déplacer le focus.

## <a name="syntax"></a>Syntaxe

```
create partition msr [size=<n>] [offset=<n>] [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| taille =`<n>` | Taille de la partition en mégaoctets (Mo). La partition est au moins aussi longue en octets que le nombre spécifié par `<n>`. Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la région actuelle. |
| décalage =`<n>` | Spécifie le décalage en kilo-octets (Ko), à partir duquel la partition est créée. Le décalage est arrondi à l’entier pour remplir complètement la taille de secteur utilisée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir. |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

#### <a name="remarks"></a>Notes 

- Sur les disques GPT utilisés pour démarrer le système d’exploitation Windows, la partition système Extensible Firmware Interface (EFI) est la première partition sur le disque, suivie de la partition réservée Microsoft. les disques GPT utilisés uniquement pour le stockage de données n’ont pas de partition système EFI, auquel cas la partition réservée Microsoft est la première partition.

- Windows ne monte pas les partitions réservées Microsoft. Vous ne pouvez pas y stocker des données et vous ne pouvez pas les supprimer.

## <a name="examples"></a>Exemples

Pour créer une partition réservée Microsoft d’une taille de 1000 mégaoctets, tapez :

```
create partition msr size=1000
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [créer une commande](create.md)

- [select disk](select-disk.md)
