---
title: créer la partition étendue
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ad7cb66-9c66-4153-b94e-1030a7225070
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa8d556822bc6caf4277812be818a0cf456e75dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818880"
---
# <a name="create-partition-extended"></a>créer la partition étendue

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition étendue sur le disque qui a le focus. Vous pouvez utiliser cette commande uniquement sur Master Boot Record \(MBR\) disques.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition extended [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|size\=<n>|Spécifie la taille de la partition en mégaoctets \(Mo\). Si aucune taille n’est donnée, la partition se poursuit jusqu'à ce que l’espace libre dans la partition étendue.|  
|offset\=<n>|Spécifie le décalage en kilo-octets \(Ko\), à laquelle la partition est créée. Si aucun décalage n’est fourni, la partition démarre au début de l’espace libre sur le disque qui est suffisamment grand pour contenir la nouvelle partition.|  
|align\=<n>|Aligne toutes les étendues de partition à la limite d’alignement le plus proche. Généralement utilisé avec le matériel RAID numéro d’unité logique \(LUN\) tableaux pour améliorer les performances. <n> est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Une fois que la partition a été créée, le focus passe automatiquement à la nouvelle partition.  
  
-   Qu’une seule partition étendue peut être créée par disque.  
  
-   Cette commande échoue si vous essayez de créer une partition étendue au sein d’une autre partition étendue.  
  
-   Vous devez créer une partition étendue avant de pouvoir créer des lecteurs logiques.  
  
-   Un disque MBR de base doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer une partition étendue de 1 000 mégaoctets taille, tapez :  
  
```  
create partition extended size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

