---
title: extend
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2414e21d-fc0b-40e8-9e33-3e072f8ad76b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c5dfeeaec966bfab3c1de2bb91bf79c9d870401
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725662"
---
# <a name="extend"></a>extend

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

étend le volume ou la partition qui a le focus et son système \(de fichiers\) pour libérer de l’espace non alloué sur un disque.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
extend [size=<n>] [disk=<n>] [noerr]  
extend filesystem [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                             Description                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| corps\=<n>  |      Spécifie la quantité d’espace en \(mégaoctets\) (Mo) à ajouter au volume ou à la partition en cours. Si aucune taille n’est donnée, tout l’espace libre contigu disponible sur le disque est utilisé.       |
| libérer\=<n>  |                          Spécifie le disque sur lequel le volume ou la partition est étendu (e). Si aucun disque n’est spécifié, le volume ou la partition est étendu sur le disque actuel.                          |
| FileSystem |                                   étend le système de fichiers du volume qui a le focus. À utiliser uniquement sur les disques où le système de fichiers n’a pas été étendu avec le volume.                                    |
|   noerr    | à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
  
## <a name="remarks"></a>Notes   
  
-   Sur les disques de base, l’espace libre doit être sur le même disque que le volume ou la partition qui a le focus. Elle doit également suivre immédiatement le volume ou la partition ayant \(le focus, elle doit commencer au décalage\)de secteur suivant.  
  
-   Sur les disques dynamiques avec des volumes simples ou fractionnés, un volume peut être étendu à n’importe quel espace libre sur un disque dynamique. À l’aide de cette commande, vous pouvez convertir un volume dynamique simple en volume dynamique fractionné. En miroir, les\-volumes RAID 5 et agrégés par bandes ne peuvent pas être étendus.  
  
-   Si la partition a été précédemment formatée avec le système de fichiers NTFS, le système de fichiers est automatiquement étendu pour remplir la plus grande partition et aucune perte de données ne se produit.  
  
-   Si la partition a été précédemment formatée avec un système de fichiers autre que NTFS, la commande échoue et aucune modification n’est apportée à la partition.  
  
-   Si la partition n’a pas été formatée précédemment avec un système de fichiers, la partition sera toujours étendue.  
  
-   La partition doit avoir un volume associé pour pouvoir être étendue.  
  
## <a name="examples"></a>Exemples  
Pour étendre le volume ou la partition avec le focus de 500 mégaoctets, sur le disque 3, tapez :  
  
```  
extend size=500 disk=3  
```  
  
Pour étendre le système de fichiers d’un volume après son extension, tapez :  
  
```  
extend filesystem  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

