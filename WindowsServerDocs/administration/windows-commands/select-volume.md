---
title: select volume
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3885d5d40e35e975ebe1cc28ddc26d4c1e78e24
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721993"
---
# <a name="select-volume"></a>select volume

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sélectionne le volume spécifié et lui déplace le focus. Cette commande peut également être utilisée pour afficher le volume qui a actuellement le focus sur le disque sélectionné.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select volume={<n>|<d>}  
```  
  
### <a name="parameters"></a>Paramètres  
  
| Paramètre |                                                                               Description                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | Numéro du volume devant recevoir le focus. Vous pouvez afficher les numéros de tous les volumes sur le disque actuellement sélectionné à l’aide de la commande **List volume** dans DiskPart. |
|    <d>    |                                                 Lettre de lecteur ou chemin d’accès du point de montage du volume qui doit recevoir le focus.                                                 |
  
## <a name="remarks"></a>Notes   
  
-   Si aucun volume n’est spécifié, cette commande affiche le volume qui a actuellement le focus sur le disque sélectionné.  
  
-   Sur un disque de base, la sélection d’un volume donne également le focus à la partition correspondante.  
  
-   Si un volume est sélectionné avec une partition correspondante, la partition est automatiquement sélectionnée.  
  
-   Si une partition est sélectionnée avec un volume correspondant, le volume est sélectionné automatiquement.  
  
## <a name="examples"></a>Exemples  
Pour déplacer le focus vers le volume 2, tapez :  
  
```  
select volume=2  
```  
  
Pour déplacer le focus sur le lecteur C, tapez :  
  
```  
select volume=c  
```  
  
Pour déplacer le focus sur le volume monté sur un dossier nommé mountpath, tapez :  
  
```  
select volume=c:\mountpath  
```  
  
Pour afficher le volume qui a actuellement le focus sur le disque sélectionné, tapez :  
  
```  
select volume  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

