---
title: mdelete_1 FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b64fa691a9ac890aad0e8d8dc93bd73e53478fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842722"
---
# <a name="ftp-mdelete_1"></a>FTP : mdelete_1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprime les fichiers sur l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
mdelete <remoteFile>[ ]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |             Description              |
|--------------|--------------------------------------|
| <remoteFile> | Spécifie le fichier distant à supprimer. |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
supprimer les fichiers distants **a. exe** et **b. exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
