---
title: Création de volume raid
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca75dd9af441446081cb10743329eb8e42166c0c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434070"
---
# <a name="create-volume-raid"></a>Création de volume raid

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume RAID\-volume 5 à l’aide de trois ou plus spécifié de disques dynamiques.  
  
> [!IMPORTANT]  
> Cette commande DiskPart n’est pas disponible dans n’importe quelle édition de Windows Vista.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|           Paramètre           |                                                                                                                                                                                                                                              Description                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           size\=<n>           | La quantité d’espace disque, en mégaoctets \(Mo\), que le volume occupera sur chaque disque. Si aucune taille n’est spécifié, c’est le plus grand RAID possible\-volume 5 est créé. Le disque avec le plus petit disponible contigu détermine la taille pour le disque RAID\-volume 5 et la même quantité d’espace est alloué à partir de chaque disque. La quantité réelle d’espace disque utilisable dans le RAID\-volume 5 est inférieure à la quantité totale d’espace disque car une partie de l’espace disque est requis pour la parité. |
| disque\=<n>,<n>,<n>\[,<n>,...\] |                                                                                                                                               Les disques dynamiques sur lequel créer le RAID\-volume 5. Vous avez besoin d’au moins trois disques dynamiques pour créer un volume RAID\-volume 5. La quantité d’espace égale à **taille\= <n>**  est allouée sur chaque disque.                                                                                                                                                |
|          align\=<n>           |                                                                                                                   Aligne toutes les étendues de volume à la limite d’alignement le plus proche. Généralement utilisé avec le matériel RAID numéro d’unité logique \(LUN\) tableaux pour améliorer les performances. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche.                                                                                                                   |
|             NOERR             |                                                                                                                                                 Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.                                                                                                                                                  |
  
## <a name="remarks"></a>Notes  
  
-   Après avoir créé le volume, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer un volume RAID\-5 volume de 1 000 mégaoctets, à l’aide de disques de 1, 2 et 3, type :  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

