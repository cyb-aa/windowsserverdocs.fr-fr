---
title: ASCII FTP
description: 'Rubrique relative aux commandes Windows pour FTP ASCII '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5ae0064f9c1679bb8b386271f042d589b158c73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376619"
---
# <a name="ftp-ascii"></a>FTP : ASCII

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Définit le type de transfert de fichier sur ASCII.   
## <a name="syntax"></a>Syntaxe  
```  
ascii  
```  
### <a name="parameters"></a>Paramètres  
aucune  
## <a name="remarks"></a>Notes  
- Le type de transfert de fichier par défaut est ASCII.  
- En mode ASCII, les conversions de caractères vers et à partir du jeu de caractères standard du réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en fonction des besoins, selon le système d’exploitation cible.  
- **FTP** prend en charge les types de transfert de fichiers image ASCII et binaires. Utilisez ASCII pour transférer des fichiers texte. Pour plus d’informations sur le transfert de fichiers binaires, consultez **ftp : Binary** dans Références supplémentaires.  
  ## <a name="BKMK_Examples"></a>Illustre  
  Définissez le type de transfert de fichier sur ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [FTP : binaire](ftp-binary.md)  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
