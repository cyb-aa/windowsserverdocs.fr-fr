---
title: mput_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6308f7b47d58bc25d964944f96fbc83a26350962
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376206"
---
# <a name="ftp-mput_1"></a>FTP : mput_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Copie les fichiers locaux sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre  |                       Description                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Spécifie le fichier local à copier sur l’ordinateur distant. |

## <a name="BKMK_Examples"></a>Illustre  
Copiez **Program1. exe** et **Program2. exe** sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [FTP : binaire](ftp-binary.md)  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
