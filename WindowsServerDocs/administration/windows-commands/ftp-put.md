---
title: put FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 15c1734322d3642ebc85891b71c6ad68100d514d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376069"
---
# <a name="ftp-put"></a>FTP : put

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie un fichier local sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Paramètres  

|   Paramètre    |                    Description                    |
|----------------|---------------------------------------------------|
|  <LocalFile>   |         Spécifie le fichier local à copier.         |
| [<remoteFile>] | Spécifie le nom à utiliser sur l’ordinateur distant. |

## <a name="remarks"></a>Notes  
- La commande **put** est identique à la commande **Send** .  
- Si *remoteFile* n’est pas spécifié, le nom du *fichier_local* est attribué au fichier.  
  ## <a name="BKMK_Examples"></a>Illustre  
  Copiez le fichier local **test. txt** et nommez-le **Test1. txt** sur l’ordinateur distant.  
  ```  
  put test.txt test1.txt  
  ```  
  Copiez le fichier **Program. exe** local sur l’ordinateur distant.  
  ```  
  put program.exe  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [FTP : ASCII](ftp-ascii.md)  
- [FTP : binaire](ftp-binary.md)  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
