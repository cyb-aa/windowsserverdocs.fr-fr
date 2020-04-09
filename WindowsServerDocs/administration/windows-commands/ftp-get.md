---
title: récupération FTP
description: Rubrique relative aux commandes Windows pour FTP obtenir
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c0b4dc41ec29edfb94661176a5ccaf651584fc43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843482"
---
# <a name="ftp-get"></a>FTP : obtient

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier distant sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
#### <a name="parameters"></a>Paramètres  

|   Paramètre   |                                                              Description                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Spécifie le fichier distant à copier.                                                   |
| [<LocalFile>] | Spécifie le nom du fichier à utiliser sur l’ordinateur local. Si *fichier_local* n’est pas spécifié, le fichier reçoit le nom *remoteFile* . |

## <a name="remarks"></a>Notes  
La commande d' **extraction** est identique à la commande **recv** .  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Copiez **test. txt** sur l’ordinateur local à l’aide du type de transfert de fichier actuel.  
```  
get test.txt  
```  
Copiez **test. txt** sur l’ordinateur local en tant que **Test1. txt** en utilisant le type de transfert de fichier actuel.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [FTP : binaire](ftp-binary.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
