---
title: Ajout de FTP
description: 'Ajout de la rubrique de commandes de Windows pour ftp '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9580d725120bb32a9b915d37cdbc173bfb17b859
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438841"
---
# <a name="ftp-append"></a>FTP : ajouter

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute un fichier local à un fichier sur l’ordinateur distant avec le type de fichier actuel.   
## <a name="syntax"></a>Syntaxe  
```  
append <LocalFile> [remoteFile]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |                               Description                                |
|--------------|--------------------------------------------------------------------------|
| <LocalFile>  |                     Spécifie le fichier local à ajouter.                     |
| [remoteFile] | Spécifie le fichier sur l’ordinateur distant auquel <LocalFile> est ajouté. |

## <a name="remarks"></a>Notes  
Si *Fichier_distant* est omis, le *LocalFile* nom est utilisé à la place le nom de fichier distant.  
## <a name="BKMK_Examples"></a>Exemples  
Ajouter file1.txt à file2.txt sur l’ordinateur distant.  
```  
append file1.txt file2.txt  
```  
ajouter le file1.txt local à un fichier nommé file1.txt sur l’ordinateur distant.  
```  
append file1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
