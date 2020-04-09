---
title: suppression FTP
description: Rubrique commandes Windows pour la suppression FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4683e63700a22d8ac8016fb118475a341221e7f6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843562"
---
# <a name="ftp-delete"></a>FTP : supprimer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprime des fichiers sur des ordinateurs distants.   
## <a name="syntax"></a>Syntaxe  
```  
delete <remoteFile>  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |          Description          |
|--------------|-------------------------------|
| <remoteFile> | Spécifie le fichier à supprimer. |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Supprimez le fichier test. txt sur l’ordinateur distant.  
```  
delete test.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
