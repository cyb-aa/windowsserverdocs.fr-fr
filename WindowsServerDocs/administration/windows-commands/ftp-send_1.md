---
title: Send_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9f658b4fbfa5f6c9fa9a58fb0c524ad53627bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375999"
---
# <a name="ftp-send_1"></a>FTP : Send_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie un fichier local sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
send <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |                    Description                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Spécifie le fichier local à copier.         |
| <remoteFile> | Spécifie le nom à utiliser sur l’ordinateur distant. |

## <a name="remarks"></a>Notes  
- La commande **Send** est identique à la commande **put** .  
- Si *remoteFile* n’est pas spécifié, le nom du *fichier_local* est attribué au fichier.  
  ## <a name="BKMK_Examples"></a>Illustre  
  Copiez le fichier local **test. txt** et nommez-le **Test1. txt** sur l’ordinateur distant.  
  ```  
  send test.txt test1.txt  
  ```  
  Copiez le fichier **Program. exe** local sur l’ordinateur distant.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
