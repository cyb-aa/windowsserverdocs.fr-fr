---
title: répertoire FTP
description: Rubrique commandes Windows pour le répertoire FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c47971c52135d79ce62f935bfed981f6eefcecaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376444"
---
# <a name="ftp-dir"></a>FTP : Rép

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche la liste des fichiers de répertoire et des sous-répertoires d’un ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<remotedirectory>]|Spécifie le répertoire dont vous souhaitez afficher la liste. Si aucun répertoire n’est spécifié, le répertoire de travail actuel sur l’ordinateur distant est utilisé.|  
|[<LocalFile>]|Spécifie un fichier local dans lequel stocker la liste des répertoires. Si aucun fichier local n’est spécifié, les résultats s’affichent à l’écran.|  
## <a name="BKMK_Examples"></a>Illustre  
Affichez une liste de répertoires pour **dir1** sur l’ordinateur distant.  
```  
dir dir1  
```  
Enregistrez une liste du répertoire actif sur l’ordinateur distant dans le fichier local **DirList. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
