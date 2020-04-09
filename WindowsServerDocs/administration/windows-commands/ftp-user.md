---
title: utilisateur FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29081bd8c5d537e1f060e4c848b720a60b4c8aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842852"
---
# <a name="ftp-user"></a>FTP : utilisateur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Spécifie un utilisateur sur l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
user <UserName> [<Password>] [<Account>]  
```  
#### <a name="parameters"></a>Paramètres  

|  Paramètre   |                                                                      Description                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Spécifie un nom d’utilisateur avec lequel se connecter à l’ordinateur distant.                                           |
| [<Password>] |               Spécifie le mot de passe pour le *nom d’utilisateur*. Si aucun mot de passe n’est spécifié, mais qu’il est requis, **FTP** vous invite à entrer le mot de passe.               |
| [<Account>]  | Spécifie un compte avec lequel se connecter à l’ordinateur distant. Si un *compte* n’est pas spécifié mais est requis, **FTP** vous invite à entrer le compte. |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Spécifiez User1 avec le mot de passe Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
