---
title: type FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: eb254d1c9b17ac6baadf6b84702d2812f1117a93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375898"
---
# <a name="ftp-type"></a>FTP : type

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

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
- Si *TypeName* n’est pas spécifié, le type actuel est affiché.  
- **FTP** prend en charge deux types de transfert de fichiers : ASCII et binaire.  
  Le type de transfert de fichier par défaut est ASCII.  La commande **ASCII** doit être utilisée lors du transfert de fichiers texte. En mode ASCII, les conversions de caractères vers et à partir du jeu de caractères standard du réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en fonction des besoins, selon le système d’exploitation à la destination.  
  La commande **binaire** doit être utilisée lors du transfert des fichiers exécutables. En mode binaire, le fichier est déplacé en unités d’un octet.  
  ## <a name="BKMK_Examples"></a>Illustre  
  Définissez le type de transfert de fichier sur ASCII.  
  ```  
  type ascii  
  ```  
  Définissez le type de fichier de transfert sur binaire.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
