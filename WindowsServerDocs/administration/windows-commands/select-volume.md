---
title: select volume
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cc981131c8de2dc4534e390645ef45c39a7b02ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371065"
---
# <a name="select-volume"></a>select volume

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

sélectionne le volume spécifié et lui déplace le focus. Cette commande peut également être utilisée pour afficher le volume qui a actuellement le focus sur le disque sélectionné.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select volume={<n>|<d>}  
```  
  
## <a name="parameters"></a>Paramètres  
  
| Paramètre |                                                                               Description                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | Numéro du volume devant recevoir le focus. Vous pouvez afficher les numéros de tous les volumes sur le disque actuellement sélectionné à l’aide de la commande **List volume** dans DiskPart. |
|    <d>    |                                                 Lettre de lecteur ou chemin d’accès du point de montage du volume qui doit recevoir le focus.                                                 |
  
## <a name="remarks"></a>Notes  
  
-   Si aucun volume n’est spécifié, cette commande affiche le volume qui a actuellement le focus sur le disque sélectionné.  
  
-   Sur un disque de base, la sélection d’un volume donne également le focus à la partition correspondante.  
  
-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.  
  
-   Si une partition est sélectionnée avec un volume correspondant, le volume est sélectionné automatiquement.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour déplacer le focus vers le volume 2, tapez :  
  
```  
select volume=2  
```  
  
Pour déplacer le focus sur le lecteur C, tapez :  
  
```  
select volume=c  
```  
  
Pour déplacer le focus sur le volume monté sur un dossier nommé « mountpath », tapez :  
  
```  
select volume=c:\mountpath  
```  
  
Pour afficher le volume qui a actuellement le focus sur le disque sélectionné, tapez :  
  
```  
select volume  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

