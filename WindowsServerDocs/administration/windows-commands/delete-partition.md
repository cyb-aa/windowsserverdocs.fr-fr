---
title: Supprimer la partition
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3b47338b74cf71a4754b7320d6b3842f342d324d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436140"
---
# <a name="delete-partition"></a>Supprimer la partition



Supprime la partition qui a le focus.

## <a name="syntax"></a>Syntaxe

```
delete partition [noerr] [override]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|override|Permet de supprimer n’importe quelle partition, quel que soit le type de DiskPart. En règle générale, DiskPart permet uniquement de supprimer des partitions de données connues.|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

> [!CAUTION]
> Suppression d’une partition sur un disque dynamique peut supprimer tous les volumes dynamiques sur le disque, par conséquent la destruction de toutes les données et en laissant le disque dans un état endommagé. Pour supprimer un volume dynamique, utilisez toujours le **supprimer volume** commande à la place. Les partitions peuvent être supprimées à partir de disques dynamiques, mais ils ne doivent pas être créées. Par exemple, il est possible de supprimer une partition de Table de Partition GUID (GPT) non reconnue sur un disque GPT dynamique. Suppression d’une partition de ce type n’entraîne pas de libérer de l’espace soit disponible. Cette commande est conçue pour vous permettre à l’espace de reclame sur un disque dynamique en mode hors connexion corrompu dans une situation d’urgence où le **propre** commandes dans DiskPart ne peut pas être utilisé.
> -   Vous ne pouvez pas supprimer la partition système, partition de démarrage ou n’importe quelle partition qui contient les actives la pagination une panne ou fichier de vidage des informations.
> -   Une partition doit être sélectionnée pour cette opération réussisse. Utilisez le **sélectionnez partition** commande pour sélectionner une partition et déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour supprimer la partition qui a le focus, tapez :
```
delete partition
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

