---
title: Devis FTP
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1101dd6a5fa163df8d43d182e9d0dfe66e340b60
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725139"
---
# <a name="ftp-quote"></a>FTP : citation

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a>Exemples  
Envoyez une commande **Quit** au serveur FTP distant.  
```  
quote quit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [FTP : literal_1](ftp-literal_1.md)  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
