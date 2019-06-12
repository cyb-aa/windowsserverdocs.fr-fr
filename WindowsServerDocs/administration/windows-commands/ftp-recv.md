---
title: FTP reçues
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bfd68dcb745ebf7ef239883aa1c5322241b32df
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438432"
---
# <a name="ftp-recv"></a>ftp: recv

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie un fichier distant sur l’ordinateur local en utilisant le mode de transfert de fichiers en cours.   
## <a name="syntax"></a>Syntaxe  
```  
recv <remoteFile> [<LocalFile>]  
```  
### <a name="parameters"></a>Paramètres  

|   Paramètre   |                   Description                    |
|---------------|--------------------------------------------------|
| <remoteFile>  |        Spécifie le fichier distant à copier.        |
| [<LocalFile>] | Spécifie le nom à utiliser sur l’ordinateur local. |

## <a name="remarks"></a>Notes  
- Le **recv** commande est identique à la **obtenir** commande.  
- Si *LocalFile* n’est pas spécifié, le fichier porte le *Fichier_distant* nom.  
  ## <a name="BKMK_Examples"></a>Exemples  
  copie **test.txt** sur l’ordinateur local en utilisant le mode de transfert de fichiers en cours.  
  ```  
  recv test.txt  
  ```  
  copie **test.txt** sur l’ordinateur local en tant que **test1.txt** type de transfert à l’aide du fichier actif.  
  ```  
  recv test.txt test1.txt  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [ftp: ascii](ftp-ascii.md)  
- [ftp: binary](ftp-binary.md)  
- [ftp: get](ftp-get.md)  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
