---
title: utilisateur FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a77bfeb-27a9-4f2f-a3c4-2fef529fb569 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63281a0ffdd646d3652eb3a442a8edd9acec9cce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375868"
---
# <a name="ftp-user"></a>FTP : utilisateur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Spécifie un utilisateur sur l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Paramètres  

|  Paramètre   |                                                                      Description                                                                      |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <UserName>  |                                          Spécifie un nom d’utilisateur avec lequel se connecter à l’ordinateur distant.                                           |
| [<Password>] |               Spécifie le mot de passe pour le *nom d’utilisateur*. Si aucun mot de passe n’est spécifié, mais qu’il est requis, **FTP** vous invite à entrer le mot de passe.               |
| [<Account>]  | Spécifie un compte avec lequel se connecter à l’ordinateur distant. Si un *compte* n’est pas spécifié mais est requis, **FTP** vous invite à entrer le compte. |

## <a name="BKMK_Examples"></a>Illustre  
Spécifiez User1 avec le mot de passe Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
