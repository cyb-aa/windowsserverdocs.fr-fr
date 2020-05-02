---
title: send_1 FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 04617246c05edde127db01ce1a0fe692eb0aceb1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725108"
---
# <a name="ftp-send_1"></a>FTP : send_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier local sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
send <LocalFile> [<remoteFile>]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                    Description                    |
|--------------|---------------------------------------------------|
| <LocalFile>  |         Spécifie le fichier local à copier.         |
| <remoteFile> | Spécifie le nom à utiliser sur l’ordinateur distant. |

## <a name="remarks"></a>Notes   
- La commande **Send** est identique à la commande **put** .  
- Si *remoteFile* n’est pas spécifié, le nom du *fichier_local* est attribué au fichier.  
  ## <a name="examples"></a>Exemples  
  Copiez le fichier local **test. txt** et nommez-le **Test1. txt** sur l’ordinateur distant.  
  ```  
  send test.txt test1.txt  
  ```  
  Copiez le fichier **Program. exe** local sur l’ordinateur distant.  
  ```  
  send program.exe  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
