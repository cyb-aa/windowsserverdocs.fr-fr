---
title: créer une bande de volume
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 46c1367b5667294a7a9df742861a011090e7a337
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379142"
---
# <a name="create-volume-stripe"></a>créer une bande de volume

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

crée un volume agrégé par bandes à l’aide de deux disques dynamiques spécifiés ou plus.  
  
> [!IMPORTANT]  
> pour Windows Vista, cette commande DiskPart est uniquement disponible dans les éditions Windows Vista Ultimate, Windows Vista entreprise et Windows Vista professionnel.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|         Paramètre         |                                                                                                                            Description                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         taille @ no__t-0 @ no__t-1         |             Quantité d’espace disque, en mégaoctets \(MB @ no__t-1, que le volume occupera sur chaque disque. Si aucune taille n’est donnée, le nouveau volume occupe l’espace libre restant sur le disque le plus petit et une quantité égale d’espace sur chaque disque suivant.             |
| disque @ no__t-0 @ no__t-1, <n> @ no__t-3, <n>,... \] |                                  Disques dynamiques sur lesquels le volume agrégé par bandes est créé. Vous avez besoin d’au moins deux disques dynamiques pour créer un volume agrégé par bandes. Une quantité d’espace égale à la **taille @ no__t-1 @ no__t-2** est allouée sur chaque disque.                                   |
|        aligner @ no__t-0 @ no__t-1         | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec le numéro d’unité logique RAID matériel @no__t 0LUN @ no__t-1 pour améliorer les performances. *n* est le nombre de kilo-octets \( Ko @ no__t-2 à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|           noerr           |                               À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                |
  
## <a name="remarks"></a>Notes  
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer un volume agrégé par bandes de 1000 mégaoctets de taille, sur les disques 1 et 2, tapez :  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

