---
title: Devis FTP
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65660cf7311713295dae8a94c9174229f5ee44be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376081"
---
# <a name="ftp-quote"></a>FTP : citation

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Envoie des arguments textuels au serveur FTP distant. Un seul code de réponse FTP est retourné.   
## <a name="syntax"></a>Syntaxe  
```  
quote <Argument>[ ]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre  |                    Description                    |
|------------|---------------------------------------------------|
| <Argument> | Spécifie l’argument à envoyer au serveur FTP. |

## <a name="remarks"></a>Notes  
La commande **quot** est identique à la commande **littérale** .  
## <a name="BKMK_Examples"></a>Illustre  
Envoyez une commande **Quit** au serveur FTP distant.  
```  
quote quit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : literal_1](ftp-literal_1.md)  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
