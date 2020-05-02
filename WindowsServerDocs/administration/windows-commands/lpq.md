---
title: lpq
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2d7a013ad9481780873cd57be4fa15732fc6196
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724255"
---
# <a name="lpq"></a>lpq

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche l’état d’une file d’attente à l’impression sur un ordinateur exécutant LPD (Line Printer Daemon).  

## <a name="syntax"></a>Syntaxe  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
### <a name="parameters"></a>Paramètres  

|    Paramètre     |                                                                        Description                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S<ServerName>  | Spécifie (par nom ou adresse IP) le périphérique de partage de l’ordinateur ou de l’imprimante qui héberge la file d’attente à l’impression LPD avec l’état que vous souhaitez afficher. Obligatoire. |
| -P<printerName> |                           Spécifie (par nom) l’imprimante pour la file d’attente à l’impression avec l’état que vous souhaitez afficher. Obligatoire.                           |
|        -l        |                                      Spécifie que vous souhaitez afficher des détails sur l’état de la file d’attente à l’impression.                                      |
|        /?        |                                                           Affiche l'aide à l'invite de commandes.                                                            |

## <a name="remarks"></a>Notes   
Les paramètres **-S** et **-P** sont sensibles à la casse et doivent être tapés en majuscules.  
## <a name="examples"></a>Exemples  
Cet exemple montre comment afficher l’état de la file d’attente d’impression Laserprinter1 sur un hôte LPD sur 10.0.0.45 :  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[imprimer la référence des commandes](print-command-reference.md)  
