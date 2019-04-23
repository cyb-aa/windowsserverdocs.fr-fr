---
title: list
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69b105a1-9710-4a06-8102-38cc9e475ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef9262a04f469f54e43cf3a83efe30fac7ad8580
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854670"
---
# <a name="list"></a>list



Affiche une liste de disques, des partitions dans un disque, des volumes sur un disque ou de disques durs virtuels (VHD).

## <a name="syntax"></a>Syntaxe

```
list { disk | partition | volume | vdisk }
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|disque|Affiche une liste des disques et des informations les concernant, telles que leur taille, la quantité d’espace libre disponible, si le disque est un disque de base ou dynamique, et si le disque utilise l’enregistrement de démarrage principal (MBR) ou le style de partition GUID partition GPT (table).|
|MBR|Affiche les partitions répertoriées dans la table de partition du disque actuelle.|
|volume|Affiche une liste des volumes de base et dynamiques sur tous les disques.|
|vdisk|Affiche une liste de disques durs virtuels qui sont attachés et/ou sélectionnés. Cette commande répertorie les disques durs virtuels détachées si elles ne sont actuellement sélectionnées ; Toutefois, le type de disque est défini sur inconnu jusqu'à ce que le disque dur virtuel est attaché. Le disque dur virtuel marqué d’un astérisque (*) a le focus.</br>Remarque: Cette commande est uniquement disponible pour Windows 7 et Windows Server 2008 R2.|

## <a name="remarks"></a>Notes

-   La liste des partitions sur un disque dynamique, les partitions peut ne pas correspondant aux volumes dynamiques sur le disque. Cette incohérence se produit parce que les disques dynamiques contiennent des entrées dans la table de partition pour le volume système ou le volume de démarrage (s’il est présent sur le disque). Elles contiennent également une partition qui occupe le reste du disque pour réserver l’espace pour des volumes dynamiques.
-   L’objet marqué avec un astérisque (*) a le focus.
-   La liste des disques, si un disque est manquant, son numéro de disque est préfixé avec M. Par exemple, le premier disque manquant est numéroté M0.

## <a name="BKMK_examples"></a>Exemples

```
list disk
list partition
list volume
list vdisk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

