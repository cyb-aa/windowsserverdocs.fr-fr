---
title: répertoire FTP
description: Rubrique commandes Windows pour le répertoire FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64f227ea9806f169c2df1698382cfce6e7ac3257
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843572"
---
# <a name="ftp-dir"></a>FTP : Rép

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche la liste des fichiers de répertoire et des sous-répertoires d’un ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<remotedirectory>]|Spécifie le répertoire dont vous souhaitez afficher la liste. Si aucun répertoire n’est spécifié, le répertoire de travail actuel sur l’ordinateur distant est utilisé.|  
|[<LocalFile>]|Spécifie un fichier local dans lequel stocker la liste des répertoires. Si aucun fichier local n’est spécifié, les résultats s’affichent à l’écran.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Affichez une liste de répertoires pour **dir1** sur l’ordinateur distant.  
```  
dir dir1  
```  
Enregistrez une liste du répertoire actif sur l’ordinateur distant dans le fichier local **DirList. txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
