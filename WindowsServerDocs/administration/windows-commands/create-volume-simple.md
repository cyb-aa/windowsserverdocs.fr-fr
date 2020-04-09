---
title: créer un volume simple
description: La rubrique commandes Windows pour créer un volume simple, qui crée un volume simple sur le disque dynamique spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da0f208d-7fda-471a-9db2-5de5ba5207c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c707a92692c5531a0e33c9537705558f2ac309
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846892"
---
# <a name="create-volume-simple"></a>créer un volume simple

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| taille\=<n>  |                                                                  Taille du volume en mégaoctets \(Mo\). Si aucune taille n’est indiquée, le nouveau volume occupe l’espace libre restant sur le disque.                                                                   |
| disque\=<n>  |                                                                                Disque dynamique sur lequel le volume est créé. Si aucun disque n’est spécifié, le disque actuel est utilisé.                                                                                |
| aligner\=<n> | Aligne toutes les étendues de volume sur la limite d’alignement la plus proche. Généralement utilisé avec le numéro d’unité logique RAID matériel \(les groupes de LUN\) pour améliorer les performances. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque jusqu’à la limite d’alignement la plus proche. |
|   noerr    |                               À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                |
  
## <a name="remarks"></a>Notes  
  
-   Une fois le volume créé, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
Pour créer un volume de 1000 mégaoctets de taille, sur le disque 1, tapez :  
  
```  
create volume simple size=1000 disk=1  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

