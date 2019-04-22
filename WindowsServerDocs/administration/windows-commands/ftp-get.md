---
title: FTP get
description: Obtenir de la rubrique de commandes de Windows pour ftp
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 798317f3921cd0e5ff12b69b972e2ea423fa6b3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816730"
---
# <a name="ftp-get"></a>FTP : obtenir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier distant sur l’ordinateur local en utilisant le mode de transfert de fichiers en cours.   
## <a name="syntax"></a>Syntaxe  
```  
get <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<remoteFile>|Spécifie le fichier distant à copier.|  
|[<LocalFile>]|Spécifie le nom du fichier à utiliser sur l’ordinateur local. Si *LocalFile* n’est pas spécifié, le fichier porte le *Fichier_distant* nom.|  
## <a name="remarks"></a>Notes  
Le **obtenir** commande est identique à la **recv** commande.  
## <a name="BKMK_Examples"></a>Exemples  
copie **test.txt** sur l’ordinateur local en utilisant le mode de transfert de fichiers en cours.  
```  
get test.txt  
```  
copie **test.txt** sur l’ordinateur local en tant que **test1.txt** type de transfert à l’aide du fichier actif.  
```  
Get test.txt test1.txt  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
