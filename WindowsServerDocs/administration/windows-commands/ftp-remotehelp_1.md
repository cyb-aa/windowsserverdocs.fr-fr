---
title: remotehelp_1 FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bac6fbe4a55c3fed4caab4e30ba848ec9ea68e21
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376028"
---
# <a name="ftp-remotehelp_1"></a>FTP : remotehelp_1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche l’aide relative aux commandes distantes.   
## <a name="syntax"></a>Syntaxe  
```  
remotehelp [<Command>]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|[<Command>]|Spécifie le nom de la commande sur laquelle vous souhaitez obtenir de l’aide. Si la *commande* n’est pas spécifiée, **FTP** affiche une liste de toutes les commandes distantes.|  
## <a name="remarks"></a>Notes  
Vous pouvez exécuter des commandes distantes à l’aide d’un **guillemet** ou d’un **littéral**.  
## <a name="BKMK_Examples"></a>Illustre  
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
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
