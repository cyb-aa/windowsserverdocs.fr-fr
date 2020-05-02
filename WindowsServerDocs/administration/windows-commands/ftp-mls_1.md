---
title: mls_1 FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4738fd49-0e80-4bdf-a773-0f973db3a710 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27f74d4c1d03cb4d9f665566f69485e80f8eccdc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725206"
---
# <a name="ftp-mls_1"></a>FTP : mls_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste abrégée de fichiers et de sous-répertoires dans un répertoire distant.   
## <a name="syntax"></a>Syntaxe  
```  
mls <remoteFile>[ ] <LocalFile>  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                       Description                       |
|--------------|---------------------------------------------------------|
| <remoteFile> | Spécifie le fichier pour lequel vous souhaitez afficher une liste. |
| <LocalFile>  |  Spécifie un fichier local dans lequel stocker la liste.  |

## <a name="remarks"></a>Notes   
- Spécification de *remoteFiles*  
  tapez un trait d'**-** Union () pour utiliser le répertoire de travail actuel sur l’ordinateur distant.  
- Spécification du *fichier_local*  
  tapez un trait d'**-** Union () pour afficher la liste à l’écran.  
  ## <a name="examples"></a>Exemples  
  Affichez une liste abrégée de fichiers et de sous-répertoires pour **dir1** et **dir2**.  
  ```  
  mls dir1 dir2 -  
  ```  
  Enregistrer une liste abrégée de fichiers et de sous-répertoires pour **dir1** et **dir2** dans le fichier local **DirList. txt**  
  ```  
  mls dir1 dir2 dirlist.txt   
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
