---
title: disque en ligne
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d798bf34ec2f9d2f01b5470c4ec52f936674135
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372509"
---
# <a name="online-disk"></a>disque en ligne



Remet les disques actuellement hors connexion à un État en ligne.

> [!IMPORTANT]
> Cette commande n’est disponible dans aucune édition de Windows Vista.

> [!IMPORTANT]
> Cette commande échoue si elle est utilisée sur un disque en lecture seule.

Pour obtenir des instructions sur l’utilisation de cette commande, consultez [réactiver un disque dynamique manquant ou hors connexion](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Syntaxe

```
online disk [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   En cas d’utilisation sans paramètre dans Windows Vista, cette commande fonctionne sur un groupe de disques. Pour les disques de base, il n’y a jamais plus d’un disque par groupe. Pour les disques dynamiques, le groupe comprend tous les disques dynamiques non étrangers.
-   Pour les disques de base, cette commande tente de mettre en ligne le disque sélectionné et tous les volumes de ce disque.
-   Pour les disques dynamiques, cette commande tente de mettre en ligne tous les disques qui ne sont pas marqués comme étrangers sur l’ordinateur local. Il tente également de mettre en ligne tous les volumes sur l’ensemble de disques dynamiques.
-   Si un disque dynamique d’un groupe de disques est mis en ligne et qu’il s’agit du seul disque du groupe, le groupe d’origine est recréé et le disque est déplacé vers ce groupe. S’il y a d’autres disques dans le groupe et qu’ils sont en ligne, le disque est simplement ajouté au groupe.
-   Si le groupe d’un disque sélectionné contient des volumes en miroir ou RAID-5, cette commande resynchronise également ces volumes.
-   Pour que cette commande aboutisse, vous devez sélectionner un disque. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="BKMK_examples"></a>Illustre

Pour mettre le disque avec le focus en ligne, tapez :
```
online disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

