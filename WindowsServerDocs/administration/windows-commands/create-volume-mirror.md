---
title: créer une mise en miroir du volume
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 72ecc4e0ede163857c47c5b7013aacdd49719ac8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378866"
---
# <a name="create-volume-mirror"></a>créer une mise en miroir du volume

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

crée un miroir de volume à l’aide des deux disques dynamiques spécifiés.  
  
> [!NOTE]  
> Cette commande est disponible uniquement dans Windows 7 et Windows Server 2008 R2.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|         Paramètre         |                                                                                                                                     Description                                                                                                                                     |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         taille\=<n>         |                 Spécifie la quantité d’espace disque, en mégaoctets \(Mo\), que le volume occupera sur chaque disque. Si aucune taille n’est donnée, le nouveau volume occupe l’espace libre restant sur le disque le plus petit et une quantité égale d’espace sur chaque disque suivant.                 |
| disque\=<n>,<n>\[,<n>,...\] |                       Spécifie les disques dynamiques sur lesquels le volume miroir est créé. Vous avez besoin de deux disques dynamiques pour créer un volume miroir. Une quantité d’espace égale à la taille spécifiée avec le paramètre de **taille** est allouée sur chaque disque.                        |
|        aligner\=<n>         | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Ce paramètre est généralement utilisé avec le numéro d’unité logique RAID matériel \(les groupes de LUN\) pour améliorer les performances. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|           noerr           |                                        Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec une erreur.                                         |
  
## <a name="remarks"></a>Notes  
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer un volume en miroir d’une taille de 1000 mégaoctets, sur les disques 1 et 2, tapez :  
  
```  
create volume mirror size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

