---
title: Devis FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bf13704150d602fbfa4e3b1a3fb1774d3bf7363
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843032"
---
# <a name="ftp-quote"></a>FTP : citation

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envoie des arguments textuels au serveur FTP distant. Un seul code de réponse FTP est retourné.   
## <a name="syntax"></a>Syntaxe  
```  
quote <Argument>[ ]  
```  
#### <a name="parameters"></a>Paramètres  

| Paramètre  |                    Description                    |
|------------|---------------------------------------------------|
| <Argument> | Spécifie l’argument à envoyer au serveur FTP. |

## <a name="remarks"></a>Notes  
La commande **quot** est identique à la commande **littérale** .  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Envoyez une commande **Quit** au serveur FTP distant.  
```  
quote quit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : literal_1](ftp-literal_1.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
