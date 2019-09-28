---
title: créer un volume simple
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1afb97c5bdb167eaf6ecfcd34ca3607b7b5a4c71
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378880"
---
# <a name="create-volume-simple"></a>créer un volume simple

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

crée un volume simple sur le disque dynamique spécifié.  
  
> [!IMPORTANT]  
> pour Windows Vista, cette commande DiskPart est uniquement disponible dans les éditions Windows Vista Ultimate, Windows Vista entreprise et Windows Vista professionnel.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                                                            Description                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| taille @ no__t-0 @ no__t-1  |                                                                  Taille du volume en mégaoctets \(MB @ no__t-1. Si aucune taille n’est indiquée, le nouveau volume occupe l’espace libre restant sur le disque.                                                                   |
| disque @ no__t-0 @ no__t-1  |                                                                                Disque dynamique sur lequel le volume est créé. Si aucun disque n’est spécifié, le disque actuel est utilisé.                                                                                |
| aligner @ no__t-0 @ no__t-1 | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec le numéro d’unité logique RAID matériel @no__t 0LUN @ no__t-1 pour améliorer les performances. *n* est le nombre de kilo-octets \( Ko @ no__t-2 à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|   noerr    |                               À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                |
  
## <a name="remarks"></a>Notes  
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer un volume de 1000 mégaoctets de taille, sur le disque 1, tapez :  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

