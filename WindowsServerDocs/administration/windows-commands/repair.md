---
title: Réparation
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e4b9cde10e11558aaa95edda94921144dac1f86
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441800"
---
# <a name="repair"></a>Réparation

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répare le RAID\-volume 5 avec le focus en remplaçant la région de disque défaillant par le disque dynamique spécifié.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                             Description                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Disque\=<n>  |                                                                 Spécifie le disque dynamique qui remplacera la région de disque défectueux.                                                                 |
| align\=<n> |          Aligne toutes les étendues de volume ou une partition à la limite d’alignement le plus proche. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque à la limite d’alignement le plus proche.           |
|   NOERR    | Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart. |
  
## <a name="remarks"></a>Notes  
  
-   Le disque dynamique spécifié doit avoir un espace libre supérieur ou égal à la taille totale de la région de disque défectueux dans le RAID\-volume 5.  
  
-   Un volume dans un volume RAID\-tableau 5 doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un volume et déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour remplacer le volume qui a le focus en la remplaçant par un disque dynamique 4, tapez :  
  
```  
repair disk=4  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

