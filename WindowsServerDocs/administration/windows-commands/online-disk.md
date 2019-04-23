---
title: disque en ligne
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4c30d0853ff0ae065f02c0ee198c8cdcb90c950b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858060"
---
# <a name="online-disk"></a>disque en ligne



Apporte des disques qui sont actuellement hors connexion à un état connecté.

> [!IMPORTANT]
> Cette commande n’est pas disponible dans n’importe quelle édition de Windows Vista.

> [!IMPORTANT]
> Cette commande échoue si elle est utilisée sur un disque en lecture seule.

Pour obtenir des instructions concernant l’utilisation de cette commande, consultez [réactiver un disque absent ou hors connexion dynamique](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Syntaxe

```
online disk [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Lorsqu’il est utilisé sans paramètres dans Windows Vista, cette commande fonctionne sur un groupe de disques. Pour les disques de base, il est jamais plus d’un disque par groupe. Pour les disques dynamiques, le groupe inclut tous les disques dynamiques non étranger.
-   Pour les disques de base, cette commande tente de mettre en ligne le disque sélectionné et tous les volumes sur ce disque.
-   Pour les disques dynamiques, cette commande tente de mettre en ligne tous les disques qui ne sont pas marqués comme étrangère sur l’ordinateur local. Elle tentera également de mettre en ligne tous les volumes sur l’ensemble de disques dynamiques.
-   Si un disque dynamique dans un groupe de disques est mis en ligne, et il est le seul disque dans le groupe, puis le groupe d’origine est recréé et le disque est déplacé vers ce groupe. S’il existe des autres disques dans le groupe et elles sont en ligne, le disque est simplement ajouté dans le groupe.
-   Si le groupe d’un disque sélectionné contient des volumes en miroir ou RAID-5, cette commande resynchronise également ces volumes.
-   Un disque doit être sélectionné pour cette commande réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour mettre le disque en ligne, tapez :
```
online disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

