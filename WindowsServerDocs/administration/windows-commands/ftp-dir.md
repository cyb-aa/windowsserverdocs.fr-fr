---
title: FTP dir
description: Rubrique de commandes de Windows pour ftp dir
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78ac8ac5e9fc4894f55401bb234aa98de981adf7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835250"
---
# <a name="ftp-dir"></a>FTP : dir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche une liste des fichiers du répertoire et des sous-répertoires sur un ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
dir [<remotedirectory>] [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<remotedirectory>]|Spécifie le répertoire pour lequel vous souhaitez afficher une liste. Si aucun répertoire n’est spécifié, le répertoire de travail actuel sur l’ordinateur distant est utilisé.|  
|[<LocalFile>]|Spécifie un fichier local dans lequel stocker la liste des répertoires. Si un fichier local n’est pas spécifié, les résultats sont affichés sur l’écran.|  
## <a name="BKMK_Examples"></a>Exemples  
Afficher une liste des répertoires de **dir1** sur l’ordinateur distant.  
```  
dir dir1  
```  
Enregistrer une liste du répertoire actif sur l’ordinateur distant dans le fichier local **ListRép.txt**.  
```  
dir . dirlist.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
