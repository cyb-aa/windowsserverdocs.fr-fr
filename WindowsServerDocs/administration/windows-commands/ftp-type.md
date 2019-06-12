---
title: type de FTP
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a261382da47501b416fa83c6d2497deae5711bb1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438356"
---
# <a name="ftp-type"></a>ftp: type

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit ou affiche le type de transfert de fichier.   
## <a name="syntax"></a>Syntaxe  
```  
type [<typeName>]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |            Description            |
|--------------|-----------------------------------|
| [<typeName>] | Spécifie le type de transfert de fichier. |

## <a name="remarks"></a>Notes  
- Si *typeName* n’est pas spécifié, le type actuel est affiché.  
- **FTP** de fichiers prend en charge deux types de transfert ASCII et binaire.  
  Le type de transfert de fichier par défaut est ASCII.  Le **ascii** commande doit être utilisée lors du transfert de fichiers texte. En mode ASCII, les conversions de caractères vers et depuis le jeu de caractères standard de réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en tant que nécessaire, en fonction du système d’exploitation à la destination.  
  Le **binaire** commande doit être utilisée lors du transfert de fichiers exécutables. En mode binaire, le fichier est déplacé en unités de 1 octet.  
  ## <a name="BKMK_Examples"></a>Exemples  
  La valeur du type de transfert de fichier ASCII.  
  ```  
  type ascii  
  ```  
  Définir le transfert de type de fichier binaire.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
