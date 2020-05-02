---
title: remotehelp_1 FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc4affb3f04eadaa4e0005e5edce0f564156f64a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725132"
---
# <a name="ftp-remotehelp_1"></a>FTP : remotehelp_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a>Exemples  
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
