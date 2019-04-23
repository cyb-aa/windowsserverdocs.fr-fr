---
title: extend
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84047c690006bf727bc12855576960bbf67d1617
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857930"
---
# <a name="extend"></a>extend

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

étend le volume ou la partition avec le focus et son système de fichiers dans gratuit \(non alloué\) espace sur un disque.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|size\=<n>|Spécifie la quantité d’espace en mégaoctets \(Mo\) à ajouter au volume actuel ou de la partition. Si aucune taille n’est donnée, tout l’espace libre contiguë qui est disponible sur le disque est utilisé.|  
|Disque\=<n>|Spécifie le disque sur lequel le volume ou une partition est étendue. Si aucun disque n’est spécifié, le volume ou la partition est étendue sur le disque en cours.|  
|système de fichiers|étend le système de fichiers du volume qui a le focus. Pour une utilisation uniquement sur des disques où le système de fichiers n’a été pas étendu avec le volume.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Disques de base, l’espace libre doit être sur le même disque que le volume ou la partition qui a le focus. Il doit suivre immédiatement le volume ou la partition qui a le focus \(, autrement dit, il doit commencer à l’offset de secteur suivant\).  
  
-   Sur des disques dynamiques avec des volumes simples ou fractionnés, un volume peut être étendu à n’importe quel espace libre sur tous les disques dynamiques. À l’aide de cette commande, vous pouvez convertir un volume dynamique simple dans un volume dynamique fractionné. Mise en miroir, RAID\-volumes agrégés et 5 ne peuvent pas être étendus.  
  
-   Si la partition a été précédemment formatée avec le système de fichiers NTFS, le système de fichiers est automatiquement étendu pour remplir la plus grande partition et aucune perte de données se produira.  
  
-   Si la partition a été précédemment mis en forme avec un système de fichiers autres que NTFS, la commande échoue et aucune modification à la partition.  
  
-   Si la partition n’était pas précédemment formatée avec un système de fichiers, la partition sera toujours prolongée.  
  
-   La partition doit avoir un volume associé avant de pouvoir être étendu.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour étendre le volume ou la partition qui a le focus à 500 mégaoctets, sur le disque 3, tapez :  
  
```  
extend size=500 disk=3  
```  
  
Pour étendre le système de fichiers d’un volume une fois qu’il a été étendu, tapez :  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

