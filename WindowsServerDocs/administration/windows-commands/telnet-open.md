---
title: ouverture Telnet
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4528b728c89bbdfc99de94c7fefebb18c8e1ad97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383647"
---
# <a name="telnet-open"></a>Telnet : ouvrir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Établit une connexion à un serveur Telnet.    
## <a name="syntax"></a>Syntaxe  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre  |                                        Description                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Spécifie le nom de l’ordinateur ou l’adresse IP.                         |
|  [<Port>]  | Spécifie le port TCP sur lequel le serveur Telnet écoute. La valeur par défaut est le port TCP 23. |

## <a name="BKMK_Examples"></a>Illustre  
Connectez-vous à un serveur Telnet sur telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
