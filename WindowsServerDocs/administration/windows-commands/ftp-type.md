---
title: type FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5531da30118914599ed0f85bfd10bd02ae89ffcf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725085"
---
# <a name="ftp-type"></a>FTP : type

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit ou affiche le type de transfert de fichier.   
## <a name="syntax"></a>Syntaxe  
```  
type [<typeName>]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |            Description            |
|--------------|-----------------------------------|
| [<typeName>] | Spécifie le type de transfert de fichier. |

## <a name="remarks"></a>Notes   
- Si *TypeName* n’est pas spécifié, le type actuel est affiché.  
- **FTP** prend en charge deux types de transfert de fichiers : ASCII et binaire.  
  Le type de transfert de fichier par défaut est ASCII.  La commande **ASCII** doit être utilisée lors du transfert de fichiers texte. En mode ASCII, les conversions de caractères vers et à partir du jeu de caractères standard du réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en fonction des besoins, selon le système d’exploitation à la destination.  
  La commande **binaire** doit être utilisée lors du transfert des fichiers exécutables. En mode binaire, le fichier est déplacé en unités d’un octet.  
  ## <a name="examples"></a>Exemples  
  Définissez le type de transfert de fichier sur ASCII.  
  ```  
  type ascii  
  ```  
  Définissez le type de fichier de transfert sur binaire.  
  ```  
  type binary  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
