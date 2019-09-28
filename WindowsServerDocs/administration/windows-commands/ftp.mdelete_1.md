---
title: mdelete_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ffbb06cff9e177316ea60e281c25d7640a7f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375924"
---
# <a name="ftp-mdelete_1"></a>FTP : mdelete_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime les fichiers sur l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |             Description              |
|--------------|--------------------------------------|
| <remoteFile> | Spécifie le fichier distant à supprimer. |

## <a name="BKMK_Examples"></a>Illustre  
supprimer les fichiers distants **a. exe** et **b. exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
