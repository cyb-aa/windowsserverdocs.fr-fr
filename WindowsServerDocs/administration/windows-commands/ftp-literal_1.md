---
title: literal_1 FTP
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f502bb56c94734870962f56cfb85dcc17ddc3f93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843392"
---
# <a name="ftp-literal_1"></a>FTP : literal_1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 envoie des arguments textuels au serveur FTP distant. Un seul code de réponse FTP est retourné.   

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
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Envoyez une commande **Quit** au serveur FTP distant.  
```  
literal quit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : citation](ftp-quote.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
