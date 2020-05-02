---
title: à la fois FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a71fbfb60ae012b5e65af04e6f3e21ec796996a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725237"
---
# <a name="ftp-mget"></a>FTP : mget

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les fichiers distants sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                        Description                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Spécifie les fichiers distants à copier sur l’ordinateur local. |

## <a name="examples"></a>Exemples  
copier les fichiers distants **a. exe** et **b. exe** sur l’ordinateur local à l’aide du type de transfert de fichier actuel.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [FTP : binaire](ftp-binary.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
