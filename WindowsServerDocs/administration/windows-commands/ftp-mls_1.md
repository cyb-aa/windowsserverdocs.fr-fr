---
title: mls_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a84a0f8f3121ea19876744e9ef04bebf5f9fcb08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376259"
---
# <a name="ftp-mls_1"></a>FTP : mls_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche une liste abrégée de fichiers et de sous-répertoires dans un répertoire distant.   
## <a name="syntax"></a>Syntaxe  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |                       Description                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Spécifie le fichier pour lequel vous souhaitez afficher une liste. |
| <LocalFile>  |  Spécifie un fichier local dans lequel stocker la liste.  |

## <a name="remarks"></a>Notes  
- Spécification de *remoteFiles*  
  tapez un trait d’Union ( **-** ) pour utiliser le répertoire de travail actuel sur l’ordinateur distant.  
- Spécification du *fichier_local*  
  tapez un trait d’Union ( **-** ) pour afficher la liste à l’écran.  
  ## <a name="BKMK_Examples"></a>Illustre  
  Affichez une liste abrégée de fichiers et de sous-répertoires pour **dir1** et **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Enregistrer une liste abrégée de fichiers et de sous-répertoires pour **dir1** et **dir2** dans le fichier local **DirList. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
