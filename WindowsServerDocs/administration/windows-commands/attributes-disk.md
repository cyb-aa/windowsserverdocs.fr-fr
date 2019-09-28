---
title: disque d’attributs
description: Rubrique relative aux commandes Windows pour les **attributs disque** -affiche, définit ou efface les attributs d’un disque.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 415125208b13d82adeed736107f59fda9489a953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382570"
---
# <a name="attributes-disk"></a>disque d’attributs



Affiche, définit ou efface les attributs d’un disque.

> [!IMPORTANT]
> Ce paramètre n’est pas disponible dans les éditions de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|jeu|Définit l’attribut spécifié du disque avec le focus.|
|clear|Efface l’attribut spécifié du disque qui a le focus.|
|seulement|Spécifie que le disque est en lecture seule.|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Lorsque le **disque des attributs** est utilisé pour afficher les attributs actuels d’un disque, l’attribut disque de démarrage indique le disque utilisé pour démarrer l’ordinateur. Pour un miroir dynamique, il est affiché pour le disque qui contient le plex de démarrage du volume de démarrage.
-   Pour que la commande de disque des **attributs** aboutisse, vous devez sélectionner un disque. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="BKMK_examples"></a>Illustre

Pour afficher les attributs du disque sélectionné, tapez :
```
attributes disk
```
Pour définir le disque sélectionné en lecture seule, tapez :
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

