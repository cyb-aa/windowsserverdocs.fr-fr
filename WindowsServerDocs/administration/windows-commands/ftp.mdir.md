---
title: mdir FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb024d063761f1e817f02fdd7301376dd167846
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842712"
---
# <a name="ftp-mdir"></a>FTP : mdir

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste de répertoires de fichiers et de sous-répertoires dans un répertoire distant.   
## <a name="syntax"></a>Syntaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                               Description                                |
|--------------|--------------------------------------------------------------------------|
| <remoteFile> |   Spécifie le répertoire ou le fichier dont vous souhaitez afficher la liste.   |
| <LocalFile>  | Spécifie un fichier local pour stocker la liste. Ce paramètre est obligatoire. |

## <a name="remarks"></a>Notes  
- Vous pouvez utiliser **mdir** pour spécifier plusieurs fichiers.  
- Spécification de *remoteFile*  
  tapez un trait d’Union ( **-** ) pour utiliser le répertoire de travail actuel sur l’ordinateur distant.  
- Spécification d’un *fichier_local*  
  tapez un trait d’Union ( **-** ) pour afficher la liste à l’écran.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
  Afficher une liste de répertoires de **dir1** et **dir2** à l’écran  
  ```  
  mdir dir1 dir2 -  
  ```  
  Enregistrez la liste de répertoires combinée de **dir1** et **dir2** dans un fichier local nommé **DirList. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
