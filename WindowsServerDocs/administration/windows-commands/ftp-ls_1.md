---
title: ls_1 FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0dd661742288bdd43299f379f4eb04016b2ac799
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725242"
---
# <a name="ftp-ls_1"></a>FTP : ls_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste abrégée de fichiers et de sous-répertoires de l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Paramètres  

|      Paramètre      |                                                                       Description                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Spécifie le répertoire dont vous souhaitez afficher la liste. Si aucun répertoire n’est spécifié, le répertoire de travail actuel sur l’ordinateur distant est utilisé. |
|    [<LocalFile>]    |               Spécifie un fichier local dans lequel stocker la liste. Si aucun fichier local n’est spécifié, les résultats s’affichent à l’écran.               |

## <a name="examples"></a>Exemples  
Affichez une liste abrégée des fichiers et sous-répertoires de l’ordinateur distant.  
```  
ls  
```  
Obtenir une liste abrégée de répertoires de **dir1** sur l’ordinateur distant et l’enregistrer dans un fichier local nommé **DirList. txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
