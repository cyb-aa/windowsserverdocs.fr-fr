---
title: put FTP
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3307ba71e7b3c8b4113f9ed29ab06660dafa5f6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868740"
---
# <a name="ftp-put"></a>FTP : put

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier local à l’ordinateur distant en utilisant le mode de transfert de fichiers en cours.   
## <a name="syntax"></a>Syntaxe  
```  
put <LocalFile> [<remoteFile>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<LocalFile>|Spécifie le fichier local à copier.|  
|[<remoteFile>]|Spécifie le nom à utiliser sur l’ordinateur distant.|  
## <a name="remarks"></a>Notes  
-   Le **put** commande est identique à la **envoyer** commande.  
-   Si *Fichier_distant* n’est pas spécifié, le fichier porte le *LocalFile* nom.  
## <a name="BKMK_Examples"></a>Exemples  
Copiez le fichier local **test.txt** et nommez-le **test1.txt** sur l’ordinateur distant.  
```  
put test.txt test1.txt  
```  
Copiez le fichier local **program.exe** à l’ordinateur distant.  
```  
put program.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
