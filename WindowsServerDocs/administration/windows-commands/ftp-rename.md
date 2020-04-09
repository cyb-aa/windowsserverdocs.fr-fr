---
title: changement de nom ftp
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe159f2833ce52921b46e46881a1d7aed8c5df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843022"
---
# <a name="ftp-rename"></a>FTP : renommer

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

renomme les fichiers distants.   
## <a name="syntax"></a>Syntaxe  
```  
rename <FileName> <NewFileName>  
```  
#### <a name="parameters"></a>Paramètres  

|   Paramètre   |                 Description                 |
|---------------|---------------------------------------------|
|  <FileName>   | Spécifie le fichier que vous souhaitez renommer. |
| <NewFileName> |        Spécifie le nouveau nom de fichier.         |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Renommez le fichier distant **example. txt** en **example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
