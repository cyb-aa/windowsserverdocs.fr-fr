---
title: mput_1 FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3fb654d5a2f44b9b63238abdbaee8d6a0294861
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725228"
---
# <a name="ftp-mput_1"></a>FTP : mput_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les fichiers locaux sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
mput <LocalFile>[ ]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre  |                       Description                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Spécifie le fichier local à copier sur l’ordinateur distant. |

## <a name="examples"></a>Exemples  
Copiez **Program1. exe** et **Program2. exe** sur l’ordinateur distant à l’aide du type de transfert de fichier actuel.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [FTP : binaire](ftp-binary.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
