---
title: select volume
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0ebf9896621268c384ea8129d32c985028054d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890730"
---
# <a name="select-volume"></a>select volume

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sélectionne le volume spécifié et lui donne le focus à ce dernier. Cette commande peut également être utilisée pour afficher le volume qui a actuellement le focus dans le disque sélectionné.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|<n>|Le numéro du volume à recevoir le focus. Vous pouvez afficher les nombres pour tous les volumes sur le disque actuellement sélectionné à l’aide de la **liste volume** commande DiskPart.|  
|<d>|La lettre ou le montage point chemin de lecteur du volume à recevoir le focus.|  
  
## <a name="remarks"></a>Notes  
  
-   Si aucun volume n’est spécifié, cette commande affiche le volume qui a actuellement le focus dans le disque sélectionné.  
  
-   Sur un disque de base, en sélectionnant un volume donne également le focus à la partition correspondante.  
  
-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.  
  
-   Si une partition est sélectionnée par un volume correspondant, le volume est automatiquement sélectionné.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour déplacer le focus vers le volume 2, tapez :  
  
```  
select volume=2  
```  
  
Pour déplacer le focus vers le lecteur C, tapez :  
  
```  
select volume=c  
```  
  
Pour déplacer le focus vers le volume monté sur un dossier nommé « mountpath », tapez :  
  
```  
select volume=c:\mountpath  
```  
  
Pour afficher le volume qui a actuellement le focus dans le disque sélectionné, tapez :  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

