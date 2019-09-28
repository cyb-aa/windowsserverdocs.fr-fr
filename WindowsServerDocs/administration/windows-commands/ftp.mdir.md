---
title: mdir FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 08aa5bb216a3d0155c100c761e476bb963e59311
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375846"
---
# <a name="ftp-mdir"></a>FTP : mdir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche une liste de répertoires de fichiers et de sous-répertoires dans un répertoire distant.   
## <a name="syntax"></a>Syntaxe  
```  
mdir <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Paramètres  

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
  ## <a name="BKMK_Examples"></a>Illustre  
  Afficher une liste de répertoires de **dir1** et **dir2** à l’écran  
  ```  
  mdir dir1 dir2 -  
  ```  
  Enregistrez la liste de répertoires combinée de **dir1** et **dir2** dans un fichier local nommé **DirList. txt**  
  ```  
  mdir dir1 dir2 dirlist.txt  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
