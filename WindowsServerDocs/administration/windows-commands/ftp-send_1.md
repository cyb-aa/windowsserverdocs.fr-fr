---
title: send_1 FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 000aa80a-60a0-4b51-815f-3237a4f3e0f4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df6ea9eda6fced91babca37639cd6efe981ba7de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843002"
---
# <a name="ftp-send_1"></a>FTP : send_1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
  ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
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
