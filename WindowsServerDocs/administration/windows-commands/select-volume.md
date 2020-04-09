---
title: select volume
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9337d7e4b37adcc22084249e53fb272335bf4f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834702"
---
# <a name="select-volume"></a>select volume

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
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
  

  

