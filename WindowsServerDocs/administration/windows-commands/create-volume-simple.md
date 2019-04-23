---
title: créer un volume simple
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d754a6b5788656132459a0b4a42954e9084f26bb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836500"
---
# <a name="create-volume-simple"></a>créer un volume simple

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un volume simple sur le disque dynamique spécifié.  
  
> [!IMPORTANT]  
> pour Windows Vista, cette commande DiskPart est uniquement disponible dans les éditions de Windows Vista Édition intégrale, Windows Vista Enterprise et Windows Vista Professionnel.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create volume simple [size=<n>] [disk=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|size\=<n>|La taille du volume en mégaoctets \(Mo\). Si aucune taille n’est spécifiée, le nouveau volume occupe l’espace libre restant sur le disque.|  
|Disque\=<n>|Le disque dynamique sur lequel le volume est créé. Si aucun disque n’est spécifié, le disque actuel est utilisé.|  
|align\=<n>|Aligne toutes les étendues de volume à la limite d’alignement le plus proche. Généralement utilisé avec le matériel RAID numéro d’unité logique \(LUN\) tableaux pour améliorer les performances. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Après avoir créé le volume, le focus se déplace automatiquement vers le nouveau volume.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer un volume de 1 000 mégaoctets dans la taille, sur le disque 1, tapez :  
  
```  
create volume simple size=1000 disk=1  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

