---
title: FTP mget
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e43bf8b6e7067a31b3ec51336b0b43845ab88f63
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438601"
---
# <a name="ftp-mget"></a>FTP : mget

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Type de transfert de copie des fichiers à distance sur l’ordinateur local à l’aide du fichier actif.   
## <a name="syntax"></a>Syntaxe  
```  
mget <remoteFile>[ ]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |                        Description                        |
|--------------|-----------------------------------------------------------|
| <remoteFile> | Spécifie les fichiers distants à copier sur l’ordinateur local. |

## <a name="BKMK_Examples"></a>Exemples  
Copiez les fichiers distants **a.exe** et **b.exe** sur l’ordinateur local en utilisant le mode de transfert de fichiers en cours.  
```  
mget a.exe b.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
