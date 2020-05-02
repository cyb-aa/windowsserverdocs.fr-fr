---
title: créer une bande de volume
description: Rubrique de référence pour Create Volume Stripe, qui crée un volume agrégé par bandes à l’aide de deux disques dynamiques spécifiés ou plus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dce735-5f7c-4f83-a580-d087e2913a00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98ca692d27135aa8ba4da0ff85ecd3074cdd73c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716877"
---
# <a name="create-volume-stripe"></a>créer une bande de volume

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume agrégé par bandes à l’aide de deux disques dynamiques spécifiés ou plus.  
  
> [!IMPORTANT]  
> pour Windows Vista, cette commande DiskPart est uniquement disponible dans les éditions Windows Vista Ultimate, Windows Vista entreprise et Windows Vista professionnel.

## <a name="syntax"></a>Syntaxe  
  
```  
create volume stripe [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|         Paramètre         |                                                                                                                            Description                                                                                                                            |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         corps\=<n>         |             Quantité d’espace disque, en mégaoctets ( \(Mo\)), que le volume occupera sur chaque disque. Si aucune taille n’est donnée, le nouveau volume occupe l’espace libre restant sur le disque le plus petit et une quantité égale d’espace sur chaque disque suivant.             |
| disque\=<n><n>,\[,<n>,...\] |                                  Disques dynamiques sur lesquels le volume agrégé par bandes est créé. Vous avez besoin d’au moins deux disques dynamiques pour créer un volume agrégé par bandes. Une quantité d’espace égale à **la\= taille** est allouée sur chaque disque.                                   |
|        droite\=<n>         | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec les groupes de LUN \(\) du numéro d’unité logique RAID matériel pour améliorer les performances. *n* est le nombre de kilo- \(octets\) (Ko) à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|           noerr           |                               à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                |
  
## <a name="remarks"></a>Notes   
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="examples"></a>Exemples  
Pour créer un volume agrégé par bandes de 1000 mégaoctets de taille, sur les disques 1 et 2, tapez :  
  
```  
create volume stripe size=1000 disk=1,2  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

