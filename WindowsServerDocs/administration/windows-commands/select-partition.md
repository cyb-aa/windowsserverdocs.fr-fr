---
title: Sélectionnez la partition
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7d5675aa6c33ddbe1e5e873e1a7cf7a2e8f8017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824960"
---
# <a name="select-partition"></a>Sélectionnez la partition

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sélectionne la partition spécifiée et déplace le focus à ce dernier. Cette commande peut également être utilisée pour afficher la partition qui a actuellement le focus dans le disque sélectionné.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|partition\=<n>|Le numéro de la partition à recevoir le focus. Vous pouvez afficher les nombres pour toutes les partitions sur le disque actuellement sélectionné à l’aide de la **liste partition** commande DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Avant de pouvoir sélectionner une partition vous devez d’abord sélectionner un disque à l’aide de la **sélectionnez disque** commande.  
  
-   Si aucun numéro de partition n’est spécifiée, cette commande affiche la partition qui a actuellement le focus dans le disque sélectionné.  
  
-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.  
  
-   Si une partition est sélectionnée par un volume correspondant, le volume est automatiquement sélectionné.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour déplacer le focus vers la partition 3, tapez :  
  
```  
select partitition=3  
```  
  
Pour afficher la partition qui a actuellement le focus dans le disque sélectionné, tapez :  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

