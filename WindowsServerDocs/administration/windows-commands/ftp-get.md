---
title: récupération FTP
description: Rubrique relative aux commandes Windows pour FTP obtenir
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cc74b56fa849a25ed2f4e4a37d339b1da87c24f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376392"
---
# <a name="ftp-get"></a>FTP : obtient

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie un fichier distant sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  

|   Paramètre   |                                                              Description                                                               |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------|
| <remoteFile>  |                                                   Spécifie le fichier distant à copier.                                                   |
| [<LocalFile>] | Spécifie le nom du fichier à utiliser sur l’ordinateur local. Si *fichier_local* n’est pas spécifié, le fichier reçoit le nom *remoteFile* . |

## <a name="remarks"></a>Notes  
La commande d' **extraction** est identique à la commande **recv** .  
## <a name="BKMK_Examples"></a>Illustre  
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
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
