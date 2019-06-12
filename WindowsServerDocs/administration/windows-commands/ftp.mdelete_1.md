---
title: ftp mdelete_1
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7b342f9d728e91085d5edf2f8e1ece00b48bec8d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438318"
---
# <a name="ftp-mdelete1"></a>FTP : mdelete_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime les fichiers sur l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
mdelete <remoteFile>[ ]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |             Description              |
|--------------|--------------------------------------|
| <remoteFile> | Spécifie le fichier distant à supprimer. |

## <a name="BKMK_Examples"></a>Exemples  
supprimer des fichiers distants **a.exe** et **b.exe**.  
```  
mdelete a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
