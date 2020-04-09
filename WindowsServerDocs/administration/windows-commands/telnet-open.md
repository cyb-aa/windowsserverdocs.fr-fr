---
title: ouverture Telnet
description: La rubrique commandes Windows pour telnet Open, qui se connecte à un serveur Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b100d2b53a340a083f22d4fd88c42363642d5da5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833332"
---
# <a name="telnet-open"></a>Telnet : ouvrir

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Connectez-vous à un serveur Telnet sur telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
