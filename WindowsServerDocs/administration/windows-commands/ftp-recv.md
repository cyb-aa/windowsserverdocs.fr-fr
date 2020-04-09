---
title: réception FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fec409741e00bb3e6f61808630e5141ce4ec78f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842962"
---
# <a name="ftp-recv"></a>FTP : recv

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier distant sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
recv <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Paramètres  

|   Paramètre   |                   Description                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Spécifie le fichier distant à copier.        |
| [<LocalFile>] | Spécifie le nom à utiliser sur l’ordinateur local. |

## <a name="remarks"></a>Notes  
- La commande **recv** est identique à la commande d' **extraction** .  
- Si *fichier_local* n’est pas spécifié, le fichier reçoit le nom *remoteFile* .  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
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
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
