---
title: créer une partition EFI
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cfc1fca-6515-4a4d-bfae-615fa8045ea9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d97129fd67345f23eee2fc7b300493a1632cc6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379011"
---
# <a name="create-partition-efi"></a>créer une partition EFI

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sur les ordinateurs Itanium\-, crée une partition système Extensible Firmware Interface \(EFI\) sur une table de partition GUID \(disque GPT\).  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                             Description                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  taille\=<n>  |                         Taille de la partition en mégaoctets \(Mo\). Si aucune taille n’est indiquée, la partition se poursuit jusqu’à ce qu’il n’y ait plus d’espace libre dans la région actuelle.                         |
| décalage\=<n> |             Décalage en kilo-octets \(Ko\), à partir duquel la partition est créée. Si aucun décalage n’est spécifié, la partition est placée dans la première étendue de disque qui est suffisamment grande pour la contenir.              |
|    noerr    | à des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
  
## <a name="remarks"></a>Notes  
  
-   Une fois la partition créée, le focus est donné à la nouvelle partition.  
  
-   Pour que cette opération aboutisse, vous devez sélectionner un disque GPT. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour créer une partition EFI de 1000 mégaoctets sur le disque sélectionné, tapez :  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

