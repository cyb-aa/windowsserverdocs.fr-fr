---
title: créer un volume simple
description: Rubrique de référence pour Create volume simple, qui crée un volume simple sur le disque dynamique spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f92bd9cf92dea258c6a49dd4cf75c4aae357c88e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716925"
---
# <a name="create-volume-simple"></a>créer un volume simple

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume simple sur le disque dynamique spécifié.  
  
> [!IMPORTANT]  
> pour Windows Vista, cette commande DiskPart est uniquement disponible dans les éditions Windows Vista Ultimate, Windows Vista entreprise et Windows Vista professionnel.
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                                                            Description                                                                                                                            |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| corps\=<n>  |                                                                  Taille du volume en mégaoctets ( \(Mo\)). Si aucune taille n’est indiquée, le nouveau volume occupe l’espace libre restant sur le disque.                                                                   |
| libérer\=<n>  |                                                                                Disque dynamique sur lequel le volume est créé. Si aucun disque n’est spécifié, le disque actuel est utilisé.                                                                                |
| droite\=<n> | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec les groupes de LUN \(\) du numéro d’unité logique RAID matériel pour améliorer les performances. *n* est le nombre de kilo- \(octets\) (Ko) à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|   noerr    |                               à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                |
  
## <a name="remarks"></a>Notes   
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="examples"></a>Exemples  
Pour créer un volume de 1000 mégaoctets de taille, sur le disque 1, tapez :  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

