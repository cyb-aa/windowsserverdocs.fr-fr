---
title: literal_1 FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc4f8aff5a22da93330a12a75e5f368285366216
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725258"
---
# <a name="ftp-literal_1"></a>FTP : literal_1

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envoie des arguments textuels au serveur FTP distant. Un seul code de réponse FTP est retourné.   

## <a name="syntax"></a>Syntaxe  
```  
literal <Argument> [ ]  
```  
#### <a name="parameters"></a>Paramètres  

| Paramètre  |                    Description                    |
|------------|---------------------------------------------------|
| <Argument> | Spécifie l’argument à envoyer au serveur FTP. |

## <a name="remarks"></a>Notes   
La commande **littérale** est identique à la commande **quote** .  
## <a name="examples"></a>Exemples  
Envoyez une commande **Quit** au serveur FTP distant.  
```  
literal quit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : citation](ftp-quote.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
