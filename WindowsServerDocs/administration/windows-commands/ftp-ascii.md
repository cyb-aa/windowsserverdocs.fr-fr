---
title: ASCII FTP
description: Rubrique relative aux commandes Windows pour FTP ASCII
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4930d0d726acaa222f802d7aaef59578030db5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843782"
---
# <a name="ftp-ascii"></a>FTP : ASCII

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le type de transfert de fichier sur ASCII.   
## <a name="syntax"></a>Syntaxe  
```  
ascii  
```  
#### <a name="parameters"></a>Paramètres  
aucune  
## <a name="remarks"></a>Notes  
- Le type de transfert de fichier par défaut est ASCII.  
- En mode ASCII, les conversions de caractères vers et à partir du jeu de caractères standard du réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en fonction des besoins, selon le système d’exploitation cible.  
- **FTP** prend en charge les types de transfert de fichiers image ASCII et binaires. Utilisez ASCII pour transférer des fichiers texte. Pour plus d’informations sur le transfert de fichiers binaires, consultez **ftp : Binary** dans Références supplémentaires.  
  ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
  Définissez le type de transfert de fichier sur ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [FTP : binaire](ftp-binary.md)  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
