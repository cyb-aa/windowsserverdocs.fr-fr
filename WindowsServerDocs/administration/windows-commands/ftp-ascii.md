---
title: ASCII FTP
description: Rubrique de référence pour le code ASCII FTP
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aa33637f43ef8d26635f36b40dbd21cfe2bff42e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725391"
---
# <a name="ftp-ascii"></a>FTP : ASCII

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit le type de transfert de fichier sur ASCII.   
## <a name="syntax"></a>Syntaxe  
```  
ascii  
```  
#### <a name="parameters"></a>Paramètres  
Aucun  
## <a name="remarks"></a>Notes   
- Le type de transfert de fichier par défaut est ASCII.  
- En mode ASCII, les conversions de caractères vers et à partir du jeu de caractères standard du réseau sont effectuées. Par exemple, les caractères de fin de ligne sont convertis en fonction des besoins, selon le système d’exploitation cible.  
- **FTP** prend en charge les types de transfert de fichiers image ASCII et binaires. Utilisez ASCII pour transférer des fichiers texte. Pour plus d’informations sur le transfert de fichiers binaires, consultez **ftp : Binary** dans Références supplémentaires.  
  ## <a name="examples"></a>Exemples  
  Définissez le type de transfert de fichier sur ASCII.  
  ```  
  ascii  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
- [FTP : binaire](ftp-binary.md)  
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
