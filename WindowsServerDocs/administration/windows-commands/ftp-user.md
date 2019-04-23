---
title: utilisateur FTP
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ef3b943491a90078dab453aaf3a037bd4ccf1825
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887490"
---
# <a name="ftp-user"></a>FTP : utilisateur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Spécifie un utilisateur à l’ordinateur distant.   
## <a name="syntax"></a>Syntaxe  
```  
user <UserName> [<Password>] [<Account>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<UserName>|Spécifie un nom d’utilisateur avec lequel vous vous connectez à l’ordinateur distant.|  
|[<Password>]|Spécifie le mot de passe *nom d’utilisateur*. Si un mot de passe n’est pas spécifié, mais est nécessaire, **ftp** vous invite à entrer le mot de passe.|  
|[<Account>]|Spécifie un compte avec lequel vous vous connectez à l’ordinateur distant. Si un *compte* n’est pas spécifié, mais est nécessaire, **ftp** demande le compte.|  
## <a name="BKMK_Examples"></a>Exemples  
Spécifiez User1 avec le mot de passe Password1.  
```  
user User1 Password1  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
