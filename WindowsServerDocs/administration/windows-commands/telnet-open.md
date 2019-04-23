---
title: Telnet ouvert
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a87c4bac000a63af806705e9371a79d7370a34c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838240"
---
# <a name="telnet-open"></a>Telnet : ouvrir

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se connecte à un serveur telnet.    
## <a name="syntax"></a>Syntaxe  
```  
o[pen] <hostname> [<Port>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<hostname>|Spécifie le nom de l’ordinateur ou l’adresse IP.|  
|[<Port>]|Spécifie le port TCP qui écoute sur le serveur telnet. La valeur par défaut est le port TCP 23.|  
## <a name="BKMK_Examples"></a>Exemples  
Se connecter à un serveur telnet à telnet.microsoft.com.  
```  
o telnet.microsoft.com  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
