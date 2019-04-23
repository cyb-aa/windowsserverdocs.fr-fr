---
title: créer la partition efi
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3714cfe52aafd4a602346139552b6712dbbc98c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878220"
---
# <a name="create-partition-efi"></a>créer la partition efi

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sur Itanium\-ordinateurs, crée une Interface micrologicielle Extensible \(EFI\) partition système sur une Table de Partition GUID \(gpt\) disque.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition efi [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|size\=<n>|La taille de la partition en mégaoctets \(Mo\). Si aucune taille n’est donnée, la partition se poursuit jusqu'à ce que l’espace libre dans la région actuelle.|  
|offset\=<n>|Le décalage en kilo-octets \(Ko\), à laquelle la partition est créée. Si aucun décalage n’est fourni, la partition est placée dans la première étendue de disque qui est assez grande pour le contenir.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Une fois que la partition a été créée, le focus est donné à la nouvelle partition.  
  
-   Un disque gpt doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer une partition EFI de 1 000 mégaoctets sur le disque sélectionné, tapez :  
  
```  
create partition efi size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

