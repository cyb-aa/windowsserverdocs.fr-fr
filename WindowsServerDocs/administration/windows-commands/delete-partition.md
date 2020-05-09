---
title: supprimer la partition
description: Rubrique de référence pour la commande delete partition, qui supprime la partition qui a le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13c79b826480171af578334942af8f73b77d796b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993124"
---
# <a name="delete-partition"></a>supprimer la partition

Supprime la partition qui a le focus. Avant de commencer, vous devez sélectionner une partition pour que cette opération aboutisse. Utilisez la commande [Sélectionner une partition](select-partition.md) pour sélectionner une partition et y déplacer le focus.

> [!WARNING]
> La suppression d’une partition sur un disque dynamique peut supprimer tous les volumes dynamiques sur le disque, en détruisant les données et en laissant le disque dans un état endommagé.
>
> Vous ne pouvez pas supprimer la partition système, la partition de démarrage ou une partition qui contient les informations de fichier d’échange actif ou de vidage sur incident.

## <a name="syntax"></a>Syntaxe

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
| override | Permet à DiskPart de supprimer n’importe quelle partition quel que soit le type. En règle générale, DiskPart vous permet uniquement de supprimer des partitions de données connues. |

#### <a name="remarks"></a>Notes 

- Pour supprimer un volume dynamique, utilisez toujours la commande [Delete Volume](delete-volume.md) à la place.

- Les partitions peuvent être supprimées des disques dynamiques, mais elles ne doivent pas être créées. Par exemple, il est possible de supprimer une partition GPT (GUID partition table) non reconnue sur un disque GPT dynamique. La suppression d’une telle partition n’entraîne pas la disponibilité de l’espace libre qui en résulte. Au lieu de cela, cette commande est conçue pour vous permettre de récupérer de l’espace sur un disque dynamique hors connexion endommagé dans une situation d’urgence où la commande [Clean](clean.md) dans DiskPart ne peut pas être utilisée.

## <a name="examples"></a>Exemples

Pour supprimer la partition qui a le focus, tapez :

```
delete partition
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [sélectionner une partition](select-partition.md)

- [commande Delete](delete.md)

- [commande Delete Volume](delete-volume.md)

- [Clean, commande](clean.md)
