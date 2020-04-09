---
title: remotehelp_1 FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3c4a4ffec01fce5cde8b2aa9dd1fa0704f3a85ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843052"
---
# <a name="ftp-remotehelp_1"></a>FTP : remotehelp_1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche l’aide relative aux commandes distantes.   
## <a name="syntax"></a>Syntaxe  
```  
remotehelp [<Command>]  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<Command>]|Spécifie le nom de la commande sur laquelle vous souhaitez obtenir de l’aide. Si la *commande* n’est pas spécifiée, **FTP** affiche une liste de toutes les commandes distantes.|  
## <a name="remarks"></a>Notes  
Vous pouvez exécuter des commandes distantes à l’aide d’un **guillemet** ou d’un **littéral**.  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Affiche la liste des commandes distantes.  
```  
remotehelp  
```  
Affiche la syntaxe de la commande distante **fonc** .  
```  
remotehelp feat  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : citation](ftp-quote.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
