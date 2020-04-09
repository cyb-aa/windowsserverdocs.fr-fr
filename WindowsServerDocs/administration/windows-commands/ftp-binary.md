---
title: binaire FTP
description: Rubrique relative aux commandes Windows pour les fichiers binaires FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ee925b4d-85d2-47b1-b7d6-3832b7ec5505 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20b2f72517826576cfee643eda0c54063b162c94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843722"
---
# <a name="ftp-binary"></a>FTP : binaire

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le type de transfert de fichier binaire.   
## <a name="syntax"></a>Syntaxe  
```  
binary  
```  
#### <a name="parameters"></a>Paramètres  
aucune  
## <a name="remarks-optional-section"></a>Remarques <optional section>  
**FTP** prend en charge les types de transfert de fichiers image ASCII et binaires. Utilisez le binaire lors du transfert des fichiers exécutables. En mode binaire, les fichiers sont transférés en unités d’un octet. Pour plus d’informations sur le transfert de fichiers ASCII, consultez **ftp : ASCII** dans Références supplémentaires.  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Définissez le type de transfert de fichier sur binaire.  
```  
binary  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : ASCII](ftp-ascii.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
