---
title: open_1 FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8bd3063a52908d65f336afcda6b6982d5bc9bf94
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843182"
---
# <a name="ftp-open_1"></a>FTP : open_1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Établit une connexion au serveur FTP spécifié.   
## <a name="syntax"></a>Syntaxe  
```  
open <computer> [<Port>]  
```  
#### <a name="parameters"></a>Paramètres  

| Paramètre  |                                           Description                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Spécifie l’ordinateur distant auquel vous essayez de vous connecter.                 |
|  [<Port>]  | Spécifie un numéro de port TCP à utiliser pour se connecter à un serveur FTP. Par défaut, le port TCP 21 est utilisé. |

## <a name="remarks"></a>Notes  
Vous pouvez utiliser une adresse IP ou un nom d’ordinateur (dans ce cas, un serveur DNS ou un fichier hôte doit être disponible) pour spécifier l' **ordinateur**.  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Connectez-vous au serveur FTP sur **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Connectez-vous au serveur FTP sur **FTP.Microsoft.com** qui écoute sur le port TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
