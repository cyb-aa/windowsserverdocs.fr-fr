---
title: ouverture Telnet
description: Rubrique de référence pour telnet Open, qui se connecte à un serveur Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c4529670ef934cfa19c9864ac59f5317eb2887a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721510"
---
# <a name="telnet-open"></a>Telnet : ouvrir

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Établit une connexion à un serveur Telnet.    

## <a name="syntax"></a>Syntaxe  
```  
o[pen] <hostname> [<Port>]  
```  
#### <a name="parameters"></a>Paramètres  

| Paramètre  |                                        Description                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Spécifie le nom de l’ordinateur ou l’adresse IP.                         |
|  [<Port>]  | Spécifie le port TCP sur lequel le serveur Telnet écoute. La valeur par défaut est le port TCP 23. |

## <a name="examples"></a>Exemples  
Connectez-vous à un serveur Telnet sur telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
