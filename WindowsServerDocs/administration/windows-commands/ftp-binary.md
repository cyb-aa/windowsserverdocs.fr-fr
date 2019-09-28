---
title: binaire FTP
description: Rubrique relative aux commandes Windows pour les fichiers binaires FTP
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 48579523f44232dec3357a20e8082050cc5175e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376582"
---
# <a name="ftp-binary"></a>FTP : binaire

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Définit le type de transfert de fichier binaire.   
## <a name="syntax"></a>Syntaxe  
```  
binary  
```  
### <a name="parameters"></a>Paramètres  
aucune  
## <a name="remarks-optional-section"></a>Remarques <optional section>  
**FTP** prend en charge les types de transfert de fichiers image ASCII et binaires. Utilisez le binaire lors du transfert des fichiers exécutables. En mode binaire, les fichiers sont transférés en unités d’un octet. Pour plus d’informations sur le transfert de fichiers ASCII, consultez **ftp : ASCII** dans Références supplémentaires.  
## <a name="BKMK_Examples"></a>Illustre  
Définissez le type de transfert de fichier sur binaire.  
```  
binary  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
