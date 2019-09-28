---
title: open_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5da1c73362c0396300f712b2e45b906d1652604
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376190"
---
# <a name="ftp-open_1"></a>FTP : open_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Établit une connexion au serveur FTP spécifié.   
## <a name="syntax"></a>Syntaxe  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre  |                                           Description                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Spécifie l’ordinateur distant auquel vous essayez de vous connecter.                 |
|  [<Port>]  | Spécifie un numéro de port TCP à utiliser pour se connecter à un serveur FTP. Par défaut, le port TCP 21 est utilisé. |

## <a name="remarks"></a>Notes  
Vous pouvez utiliser une adresse IP ou un nom d’ordinateur (dans ce cas, un serveur DNS ou un fichier hôte doit être disponible) pour spécifier l' **ordinateur**.  
## <a name="BKMK_Examples"></a>Illustre  
Connectez-vous au serveur FTP sur **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Connectez-vous au serveur FTP sur **FTP.Microsoft.com** qui écoute sur le port TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
