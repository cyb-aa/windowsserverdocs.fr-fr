---
title: à la fois FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 666025c92b6fb1a612cbe7b83833557a8a7d5017
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376299"
---
# <a name="ftp-mget"></a>FTP : mget

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie les fichiers distants sur l’ordinateur local à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |                        Description                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Spécifie les fichiers distants à copier sur l’ordinateur local. |

## <a name="BKMK_Examples"></a>Illustre  
copier les fichiers distants **a. exe** et **b. exe** sur l’ordinateur local à l’aide du type de transfert de fichier actuel.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [FTP : binaire](ftp-binary.md)  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
