---
title: binaire FTP
description: Rubrique de référence pour le fichier binaire FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60a3e84bf9256dd5c71dd4444b5939980eecc512
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725369"
---
# <a name="ftp-binary"></a>FTP : binaire

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le type de transfert de fichier binaire.   
## <a name="syntax"></a>Syntaxe  
```  
binary  
```  
#### <a name="parameters"></a>Paramètres  
Aucun  
## <a name="remarks-optional-section"></a>Concernant<optional section>  
**FTP** prend en charge les types de transfert de fichiers image ASCII et binaires. Utilisez le binaire lors du transfert des fichiers exécutables. En mode binaire, les fichiers sont transférés en unités d’un octet. Pour plus d’informations sur le transfert de fichiers ASCII, consultez **ftp : ASCII** dans Références supplémentaires.  
## <a name="examples"></a>Exemples  
Définissez le type de transfert de fichier sur binaire.  
```  
binary  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
