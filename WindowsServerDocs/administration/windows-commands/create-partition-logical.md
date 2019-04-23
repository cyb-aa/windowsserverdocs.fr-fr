---
title: créer une partition logique
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b347581773a086d525bb005edeca2efa31e1848
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886050"
---
# <a name="create-partition-logical"></a>créer une partition logique

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une partition logique dans une partition étendue existante. Vous pouvez uniquement utiliser cette commande sur l’enregistrement de démarrage principal \(MBR\) disques.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|size\=<n>|Spécifie la taille de la partition logique en mégaoctets \(Mo\), qui doit être inférieur à la partition étendue. Si aucune taille n’est donnée, la partition se poursuit jusqu'à ce que l’espace libre dans la partition étendue.|  
|offset\=<n>|Spécifie le décalage en kilo-octets \(Ko\), à laquelle la partition est créée. L’arrondi de manière à remplir complètement la taille de cylindre est utilisé. Si aucun décalage n’est fourni, la partition est placée dans la première étendue de disque qui est assez grande pour le contenir. La partition est au moins aussi longue en octets que le nombre spécifié par **taille\=<n>**. Si vous spécifiez une taille pour la partition logique, il doit être inférieure à la partition étendue.|  
|align\=<n>|Aligne toutes les étendues de volume ou une partition à la limite d’alignement le plus proche. Généralement utilisé avec le matériel RAID numéro d’unité logique \(LUN\) tableaux pour améliorer les performances.  <n> est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Si le **taille** et **décalage** paramètres ne sont pas spécifiés, la partition logique est créée dans l’étendue la plus grande de disque disponible dans la partition étendue.  
  
-   Une fois que la partition a été créée, le focus passe automatiquement à la nouvelle partition logique.  
  
-   Un disque MBR de base doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer une partition logique de 1 000 mégaoctets de la taille, dans la partition étendue du disque sélectionné, tapez :  
  
```  
create partition logical size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

