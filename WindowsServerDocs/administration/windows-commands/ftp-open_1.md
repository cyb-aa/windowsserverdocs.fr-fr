---
title: FTP open_1
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45de8b3c210fe0925ac3cc43c41d3e092d5dfe16
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438507"
---
# <a name="ftp-open1"></a>FTP : open_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se connecte au serveur ftp spécifié.   
## <a name="syntax"></a>Syntaxe  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre  |                                           Description                                            |
|------------|--------------------------------------------------------------------------------------------------|
| <computer> |                Spécifie l’ordinateur distant auquel vous essayez de vous connecter.                 |
|  [<Port>]  | Spécifie un numéro de port TCP à utiliser pour se connecter à un serveur ftp. Par défaut, le port TCP 21 est utilisé. |

## <a name="remarks"></a>Notes  
Vous pouvez utiliser un nom d’ordinateur ou adresse IP (auquel cas un serveur DNS ou le fichier Hosts doit être disponible) pour spécifier **ordinateur**.  
## <a name="BKMK_Examples"></a>Exemples  
Se connecter au serveur ftp sur **FTP.Microsoft.com**.  
```  
Open ftp.microsoft.com  
```  
Se connecter au serveur ftp sur **FTP.Microsoft.com** qui est à l’écoute sur le port TCP 755.  
```  
open ftp.microsoft.com 755  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
