---
title: créer un volume RAID
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5a3c13cb5b78ae3e771b461a35a7130a48e7ec01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378864"
---
# <a name="create-volume-raid"></a>créer un volume RAID

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

crée un volume RAID @ no__t-05 à l’aide de trois disques dynamiques spécifiés ou plus.  
  
> [!IMPORTANT]  
> Cette commande DiskPart n’est pas disponible dans les éditions de Windows Vista.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|           Paramètre           |                                                                                                                                                                                                                                              Description                                                                                                                                                                                                                                              |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           taille @ no__t-0 @ no__t-1           | Quantité d’espace disque, en mégaoctets \(MB @ no__t-1, que le volume occupera sur chaque disque. Si aucune taille n’est donnée, le plus grand volume RAID @ no__t-05 possible sera créé. Le disque avec le plus petit espace libre contigu disponible détermine la taille du volume RAID @ no__t-05 et la même quantité d’espace est allouée à partir de chaque disque. La quantité réelle d’espace disque utilisable dans le volume RAID @ no__t-05 est inférieure à la quantité combinée d’espace disque, car une partie de l’espace disque est nécessaire pour la parité. |
| disque @ no__t-0 @ no__t-1, <n>, <n> @ no__t-4, <n>,... \] |                                                                                                                                               Disques dynamiques sur lesquels créer le volume RAID @ no__t-05. Vous devez disposer d’au moins trois disques dynamiques afin de créer un volume RAID @ no__t-05. Une quantité d’espace égale à la **taille @ no__t-1 @ no__t-2** est allouée sur chaque disque.                                                                                                                                                |
|          aligner @ no__t-0 @ no__t-1           |                                                                                                                   Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec le numéro d’unité logique RAID matériel @no__t 0LUN @ no__t-1 pour améliorer les performances. *n* est le nombre de kilo-octets \( Ko @ no__t-2 à partir du début du disque jusqu’à la limite d’alignement la plus proche.                                                                                                                   |
|             noerr             |                                                                                                                                                 À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                                                  |
  
## <a name="remarks"></a>Notes  
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer un volume RAID @ no__t-05 de 1000 mégaoctets de taille, à l’aide des disques 1, 2 et 3, tapez :  
  
```  
create volume raid size=1000 disk=1,2,3  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

