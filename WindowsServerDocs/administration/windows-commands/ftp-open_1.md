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
ms.openlocfilehash: da7926914e2cbdbb4909093d90c33025ae5cd695
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882470"
---
# <a name="ftp-open1"></a>FTP : open_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se connecte au serveur ftp spécifié.   
## <a name="syntax"></a>Syntaxe  
```  
open <computer> [<Port>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|<computer>|Spécifie l’ordinateur distant auquel vous essayez de vous connecter.|  
|[<Port>]|Spécifie un numéro de port TCP à utiliser pour se connecter à un serveur ftp. Par défaut, le port TCP 21 est utilisé.|  
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
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
