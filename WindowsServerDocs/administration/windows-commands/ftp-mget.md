---
title: à la fois FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72b45f84622fcd6abf7a743f606fb514def584cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843352"
---
# <a name="ftp-mget"></a>FTP : mget

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les fichiers distants sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
mget <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                        Description                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Spécifie les fichiers distants à copier sur l’ordinateur local. |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
copier les fichiers distants **a. exe** et **b. exe** sur l’ordinateur local à l’aide du type de transfert de fichier actuel.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [FTP : binaire](ftp-binary.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
