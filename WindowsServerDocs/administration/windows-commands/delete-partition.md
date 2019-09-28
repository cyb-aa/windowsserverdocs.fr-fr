---
title: supprimer la partition
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65752312-cb16-46f6-870f-1b95c507b101
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46a214f26e7c21f6ae08eb16d95fd898bd949b0f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378665"
---
# <a name="delete-partition"></a>supprimer la partition



Supprime la partition qui a le focus.

## <a name="syntax"></a>Syntaxe

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|remplacer|Permet à DiskPart de supprimer n’importe quelle partition quel que soit le type. En règle générale, DiskPart vous permet uniquement de supprimer des partitions de données connues.|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

> [!CAUTION]
> La suppression d’une partition sur un disque dynamique peut supprimer tous les volumes dynamiques sur le disque, détruisant ainsi les données et laissant le disque dans un état endommagé. Pour supprimer un volume dynamique, utilisez toujours la commande **Delete Volume** à la place. Les partitions peuvent être supprimées des disques dynamiques, mais elles ne doivent pas être créées. Par exemple, il est possible de supprimer une partition GPT (GUID partition table) non reconnue sur un disque GPT dynamique. La suppression d’une telle partition n’entraîne pas la disponibilité de l’espace libre qui en résulte. Cette commande a pour but de vous permettre de récupérer de l’espace sur un disque dynamique hors connexion endommagé dans une situation d’urgence où la commande **Clean** dans DiskPart ne peut pas être utilisée.
> -   Vous ne pouvez pas supprimer la partition système, la partition de démarrage ou une partition qui contient les informations de fichier d’échange actif ou de vidage sur incident.
> -   Une partition doit être sélectionnée pour que cette opération aboutisse. Utilisez la commande **Sélectionner une partition** pour sélectionner une partition et y déplacer le focus.

## <a name="BKMK_examples"></a>Illustre

Pour supprimer la partition qui a le focus, tapez :
```
delete partition
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

