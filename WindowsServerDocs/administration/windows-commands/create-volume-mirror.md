---
title: Création d’un volume miroir
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72d80fdf6eca1262a858cbe2a98ed8c9c421bff6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434083"
---
# <a name="create-volume-mirror"></a>Création d’un volume miroir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un miroir de volume à l’aide de deux disques dynamiques spécifiés.  
  
> [!NOTE]  
> Cette commande est uniquement disponible dans Windows 7 et Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|         Paramètre         |                                                                                                                                     Description                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         size\=<n>         |                 Spécifie la quantité d’espace disque, en mégaoctets \(Mo\), que le volume occupera sur chaque disque. Si aucune taille n’est spécifiée, le nouveau volume occupe l’espace libre restant sur le plus petit disque et une quantité d’espace égale sur les disques suivants.                 |
| disque\=<n>,<n>\[,<n>,...\] |                       Spécifie les disques dynamiques sur lequel le volume en miroir est créé. Vous avez besoin de deux disques dynamiques pour créer un volume en miroir. La quantité d’espace qui est égale à la taille spécifiée avec le **taille** paramètre est alloué sur chaque disque.                        |
|        align\=<n>         | Aligne toutes les étendues de volume à la limite d’alignement le plus proche. Ce paramètre est généralement utilisé avec le numéro d’unité logique RAID de matériel \(LUN\) tableaux pour améliorer les performances. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche. |
|           NOERR           |                                        Utilisé pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec une erreur de DiskPart.                                         |
  
## <a name="remarks"></a>Notes  
  
-   Après avoir créé le volume, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer un volume en miroir de 1 000 mégaoctets dans la taille, sur le disque 1 et 2, tapez :  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

