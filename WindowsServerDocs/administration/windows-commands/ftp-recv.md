---
title: réception FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec35a2044945e3d39a2a78d39923de3a56eb18d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376127"
---
# <a name="ftp-recv"></a>FTP : recv

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie un fichier distant sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  

|   Paramètre   |                   Description                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Spécifie le fichier distant à copier.        |
| [<LocalFile>] | Spécifie le nom à utiliser sur l’ordinateur local. |

## <a name="remarks"></a>Notes  
- La commande **recv** est identique à la commande d' **extraction** .  
- Si *fichier_local* n’est pas spécifié, le fichier reçoit le nom *remoteFile* .  
  ## <a name="BKMK_Examples"></a>Illustre  
  Copiez **test. txt** sur l’ordinateur local à l’aide du type de transfert de fichier actuel.  
  ```  
  recv test.txt  
  ```  
  Copiez **test. txt** sur l’ordinateur local en tant que **Test1. txt** en utilisant le type de transfert de fichier actuel.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [FTP : ASCII](ftp-ascii.md)  
- [FTP : binaire](ftp-binary.md)  
- [FTP : obtient](ftp-get.md)  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
