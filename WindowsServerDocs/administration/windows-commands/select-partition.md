---
title: sélectionner une partition
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97145d73cbbe1bdc9b27e545b047b78fe89e4984
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834802"
---
# <a name="select-partition"></a>sélectionner une partition

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sélectionne la partition spécifiée et lui déplace le focus. Cette commande peut également être utilisée pour afficher la partition qui a actuellement le focus sur le disque sélectionné.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select partition=<n>  
```  
  
### <a name="parameters"></a>Paramètres  
  
|   Paramètre    |                                                                                    Description                                                                                    |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| partition\=<n> | Numéro de la partition qui doit recevoir le focus. Vous pouvez afficher les numéros de toutes les partitions sur le disque actuellement sélectionné à l’aide de la commande **List partition** dans DiskPart. |
  
## <a name="remarks"></a>Notes  
  
-   Avant de pouvoir sélectionner une partition, vous devez d’abord sélectionner un disque à l’aide de la commande **Sélectionner le disque** .  
  
-   Si aucun numéro de partition n’est spécifié, cette commande affiche la partition qui a actuellement le focus sur le disque sélectionné.  
  
-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.  
  
-   Si une partition est sélectionnée avec un volume correspondant, le volume est sélectionné automatiquement.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
Pour déplacer le focus vers la partition 3, tapez :  
  
```  
select partitition=3  
```  
  
Pour afficher la partition qui a actuellement le focus sur le disque sélectionné, tapez :  
  
```  
select partition  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

