---
title: créer des agrégats par bandes de volume
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55ed731df4613e215fb4d0954a5b8424035b1166
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434000"
---
# <a name="create-volume-stripe"></a>créer des agrégats par bandes de volume

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume agrégé par bandes à l’aide de deux ou plusieurs disques dynamiques spécifiés.  
  
> [!IMPORTANT]  
> pour Windows Vista, cette commande DiskPart est uniquement disponible dans les éditions de Windows Vista Édition intégrale, Windows Vista Enterprise et Windows Vista Professionnel.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|         Paramètre         |                                                                                                                            Description                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size\=<n>         |             La quantité d’espace disque, en mégaoctets \(Mo\), que le volume occupera sur chaque disque. Si aucune taille n’est spécifiée, le nouveau volume occupe l’espace libre restant sur le plus petit disque et une quantité d’espace égale sur les disques suivants.             |
| disque\=<n>,<n>\[,<n>,...\] |                                  Les disques dynamiques sur lequel le volume est créé. Vous devez au moins deux disques dynamiques pour créer un volume agrégé par bandes. La quantité d’espace égale à **taille\= <n>**  est allouée sur chaque disque.                                   |
|        align\=<n>         | Aligne toutes les étendues de volume à la limite d’alignement le plus proche. Généralement utilisé avec le matériel RAID numéro d’unité logique \(LUN\) tableaux pour améliorer les performances. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche. |
|           NOERR           |                               Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.                                |
  
## <a name="remarks"></a>Notes  
  
-   Après avoir créé le volume, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer un volume agrégé par bandes de 1 000 mégaoctets dans la taille, sur le disque 1 et 2, tapez :  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

