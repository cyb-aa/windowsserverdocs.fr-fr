---
title: ftp mput_1
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd19a97246aa6155182cb055deceb4b5a5019f6c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438580"
---
# <a name="ftp-mput1"></a>FTP : mput_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Type de transfert de copie des fichiers locaux à l’ordinateur distant à l’aide du fichier actif.   
## <a name="syntax"></a>Syntaxe  
```  
mput <LocalFile>[ ]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre  |                       Description                        |
|-------------|----------------------------------------------------------|
| <LocalFile> | Spécifie le fichier local à copier sur l’ordinateur distant. |

## <a name="BKMK_Examples"></a>Exemples  
copie **Program1.exe** et **Program2.exe** à l’ordinateur distant en utilisant le mode de transfert de fichiers en cours.  
```  
mput Program1.exe Program2.exe  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [ftp: ascii](ftp-ascii.md)  
-   [ftp: binary](ftp-binary.md)  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
