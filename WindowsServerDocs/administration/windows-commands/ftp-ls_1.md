---
title: ls_1 FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ff63272fe3c5e59965b04bef258a06fee2da0c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843362"
---
# <a name="ftp-ls_1"></a>FTP : ls_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> 
> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
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
