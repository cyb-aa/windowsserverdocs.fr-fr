---
title: sélectionner une partition
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9a186e2678fde64396a8b4b57a2d14e4b0b7bf26
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371069"
---
# <a name="select-partition"></a>sélectionner une partition

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sélectionne la partition spécifiée et lui déplace le focus. Cette commande peut également être utilisée pour afficher la partition qui a actuellement le focus sur le disque sélectionné.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select partition=<n>  
```  
  
## <a name="parameters"></a>Paramètres  
  
|   Paramètre    |                                                                                    Description                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| partition\=<n> | Numéro de la partition qui doit recevoir le focus. Vous pouvez afficher les numéros de toutes les partitions sur le disque actuellement sélectionné à l’aide de la commande **List partition** dans DiskPart. |
  
## <a name="remarks"></a>Notes  
  
-   Avant de pouvoir sélectionner une partition, vous devez d’abord sélectionner un disque à l’aide de la commande **Sélectionner le disque** .  
  
-   Si aucun numéro de partition n’est spécifié, cette commande affiche la partition qui a actuellement le focus sur le disque sélectionné.  
  
-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.  
  
-   Si une partition est sélectionnée avec un volume correspondant, le volume est sélectionné automatiquement.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour déplacer le focus vers la partition 3, tapez :  
  
```  
select partitition=3  
```  
  
Pour afficher la partition qui a actuellement le focus sur le disque sélectionné, tapez :  
  
```  
select partition  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

