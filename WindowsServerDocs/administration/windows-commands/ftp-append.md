---
title: Ajout FTP
description: Rubrique relative aux commandes Windows pour l’ajout FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44ce1d6e7259dc8745da35ed462e6378f0fce8ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843822"
---
# <a name="ftp-append"></a>FTP : ajouter

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un fichier local à un fichier sur l’ordinateur distant en utilisant le paramètre de type de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
append <LocalFile> [remoteFile]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                               Description                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Spécifie le fichier local à ajouter.                     |
| [remoteFile] | Spécifie le fichier sur l’ordinateur distant auquel <LocalFile> est ajouté. |

## <a name="remarks"></a>Notes  
Si *remoteFile* est omis, le nom du *fichier_local* est utilisé à la place du nom du fichier distant.  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Ajoutez fichier1. txt à fichier2. txt sur l’ordinateur distant.  
```  
append file1.txt file2.txt  
```  
Ajoutez le fichier fichier1. txt local à un fichier nommé fichier1. txt sur l’ordinateur distant.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
