---
title: créer une partition EFI
description: Rubrique de référence pour Create partition EFI, qui crée une partition système Extensible Firmware Interface (EFI) sur un disque GPT (GUID partition table) sur des ordinateurs Itanium.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc505dcbd94cf616b4ea160501ed1eadc1456a18
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719271"
---
# <a name="create-partition-efi"></a>créer une partition EFI

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sur les ordinateurs Itanium, crée une partition système Extensible Firmware Interface (EFI) sur un disque GPT (GUID partition table).

## <a name="syntax"></a>Syntaxe  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                             Description                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  corps\=<n>  |                         Taille de la partition en mégaoctets ( \(Mo\)). Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la région actuelle.                         |
| décalage\=<n> |             Décalage en kilo-octets \((\)Ko), à partir duquel la partition est créée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir.              |
|    noerr    | à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
  
## <a name="remarks"></a>Notes   
  
-   Une fois la partition créée, le focus est donné à la nouvelle partition.  
  
-   Pour que cette opération aboutisse, vous devez sélectionner un disque GPT. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.  
  
## <a name="examples"></a>Exemples  
Pour créer une partition EFI de 1000 mégaoctets sur le disque sélectionné, tapez :  
  
```  
create partition efi size=1000  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

