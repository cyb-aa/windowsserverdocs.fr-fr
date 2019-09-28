---
title: ls_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d183f6a014273b78befd14c8d3208508948ffc54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376252"
---
# <a name="ftp-ls_1"></a>FTP : ls_1

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012
> 
> 
> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche une liste abrégée de fichiers et de sous-répertoires de l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
ls [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  

|      Paramètre      |                                                                       Description                                                                        |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| [<remotedirectory>] | Spécifie le répertoire dont vous souhaitez afficher la liste. Si aucun répertoire n’est spécifié, le répertoire de travail actuel sur l’ordinateur distant est utilisé. |
|    [<LocalFile>]    |               Spécifie un fichier local dans lequel stocker la liste. Si aucun fichier local n’est spécifié, les résultats s’affichent à l’écran.               |

## <a name="BKMK_Examples"></a>Illustre  
Affichez une liste abrégée des fichiers et sous-répertoires de l’ordinateur distant.  
```  
ls  
```  
Obtenir une liste abrégée de répertoires de **dir1** sur l’ordinateur distant et l’enregistrer dans un fichier local nommé **DirList. txt**  
```  
ls dir1 dirlist.txt   
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
