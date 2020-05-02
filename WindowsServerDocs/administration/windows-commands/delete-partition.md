---
title: supprimer la partition
description: Rubrique de référence pour la suppression d’une partition, qui supprime la partition qui a le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 259abdfc6e3ba8db22d5582bff08d7a4bc8b807b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716729"
---
# <a name="delete-partition"></a>supprimer la partition

Supprime la partition qui a le focus.

## <a name="syntax"></a>Syntaxe

```
delete partition [noerr] [override]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|override|Permet à DiskPart de supprimer n’importe quelle partition quel que soit le type. En règle générale, DiskPart vous permet uniquement de supprimer des partitions de données connues.|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes 

> [!CAUTION]
> La suppression d’une partition sur un disque dynamique peut supprimer tous les volumes dynamiques sur le disque, détruisant ainsi les données et laissant le disque dans un état endommagé. Pour supprimer un volume dynamique, utilisez toujours la commande **Delete Volume** à la place. Les partitions peuvent être supprimées des disques dynamiques, mais elles ne doivent pas être créées. Par exemple, il est possible de supprimer une partition GPT (GUID partition table) non reconnue sur un disque GPT dynamique. La suppression d’une telle partition n’entraîne pas la disponibilité de l’espace libre qui en résulte. Cette commande a pour but de vous permettre de récupérer de l’espace sur un disque dynamique hors connexion endommagé dans une situation d’urgence où la commande **Clean** dans DiskPart ne peut pas être utilisée.
> -   Vous ne pouvez pas supprimer la partition système, la partition de démarrage ou une partition qui contient les informations de fichier d’échange actif ou de vidage sur incident.
> -   Une partition doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner une partition et y déplacer le focus.

## <a name="examples"></a>Exemples

Pour supprimer la partition qui a le focus, tapez :
```
delete partition
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

