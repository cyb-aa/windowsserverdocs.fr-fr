---
title: utilisateur FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb4f0f47f23bec312c57266479c261c96535e8cc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725059"
---
# <a name="ftp-user"></a>FTP : utilisateur

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a>Exemples  
Spécifiez User1 avec le mot de passe Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
