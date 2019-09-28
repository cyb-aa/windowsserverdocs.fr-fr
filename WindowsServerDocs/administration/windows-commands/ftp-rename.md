---
title: changement de nom ftp
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 977baa042a6b0d9c23db7cb398bee997c2049227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376023"
---
# <a name="ftp-rename"></a>FTP : renommer

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

renomme les fichiers distants.   
## <a name="syntax"></a>Syntaxe  
```  
rename <FileName> <NewFileName>  
```  
### <a name="parameters"></a>Paramètres  

|   Paramètre   |                 Description                 |
|---------------|---------------------------------------------|
|  <FileName>   | Spécifie le fichier que vous souhaitez renommer. |
| <NewFileName> |        Spécifie le nouveau nom de fichier.         |

## <a name="BKMK_Examples"></a>Illustre  
Renommez le fichier distant **example. txt** en **example1. txt**  
```  
rename example.txt example1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
