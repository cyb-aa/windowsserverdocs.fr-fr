---
title: extend
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bb54a661bf60b55fd95bf3a686d758d13831a6ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377307"
---
# <a name="extend"></a>extend

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

étend le volume ou la partition qui a le focus et son système de fichiers en libre \(espace\) non alloué sur un disque.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                             Description                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| taille\=<n>  |      Spécifie la quantité d’espace en mégaoctets \(Mo\) à ajouter au volume ou à la partition en cours. Si aucune taille n’est donnée, tout l’espace libre contigu disponible sur le disque est utilisé.       |
| disque\=<n>  |                          Spécifie le disque sur lequel le volume ou la partition est étendu (e). Si aucun disque n’est spécifié, le volume ou la partition est étendu sur le disque actuel.                          |
| FileSystem |                                   étend le système de fichiers du volume qui a le focus. À utiliser uniquement sur les disques où le système de fichiers n’a pas été étendu avec le volume.                                    |
|   noerr    | à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
  
## <a name="remarks"></a>Notes  
  
-   Sur les disques de base, l’espace libre doit être sur le même disque que le volume ou la partition qui a le focus. Il doit également suivre immédiatement le volume ou la partition ayant le focus \(autrement dit, il doit commencer au décalage de secteur suivant\).  
  
-   Sur les disques dynamiques avec des volumes simples ou fractionnés, un volume peut être étendu à n’importe quel espace libre sur un disque dynamique. À l’aide de cette commande, vous pouvez convertir un volume dynamique simple en volume dynamique fractionné. Les volumes en miroir, RAID\-5 et agrégés par bandes ne peuvent pas être étendus.  
  
-   Si la partition a été précédemment formatée avec le système de fichiers NTFS, le système de fichiers est automatiquement étendu pour remplir la plus grande partition et aucune perte de données ne se produit.  
  
-   Si la partition a été précédemment formatée avec un système de fichiers autre que NTFS, la commande échoue et aucune modification n’est apportée à la partition.  
  
-   Si la partition n’a pas été formatée précédemment avec un système de fichiers, la partition sera toujours étendue.  
  
-   La partition doit avoir un volume associé pour pouvoir être étendue.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour étendre le volume ou la partition avec le focus de 500 mégaoctets, sur le disque 3, tapez :  
  
```  
extend size=500 disk=3  
```  
  
Pour étendre le système de fichiers d’un volume après son extension, tapez :  
  
```  
extend filesystem  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

