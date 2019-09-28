---
title: lpq
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a3755c010c9bb4549deed08f26b5a0670fe7318
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374206"
---
# <a name="lpq"></a>lpq

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche l’état d’une file d’attente à l’impression sur un ordinateur exécutant LPD (Line Printer Daemon).  

## <a name="syntax"></a>Syntaxe  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Paramètres  

|    Paramètre     |                                                                        Description                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Spécifie (par nom ou adresse IP) le périphérique de partage de l’ordinateur ou de l’imprimante qui héberge la file d’attente à l’impression LPD avec l’état que vous souhaitez afficher. Obligatoire. |
| -P <printerName> |                           Spécifie (par nom) l’imprimante pour la file d’attente à l’impression avec l’état que vous souhaitez afficher. Obligatoire.                           |
|        -l        |                                      Spécifie que vous souhaitez afficher des détails sur l’état de la file d’attente à l’impression.                                      |
|        /?        |                                                           Affiche l'aide à l'invite de commandes.                                                            |

## <a name="remarks"></a>Notes  
Les paramètres **-S** et **-P** sont sensibles à la casse et doivent être tapés en majuscules.  
## <a name="BKMK_examples"></a>Illustre  
Cet exemple montre comment afficher l’état de la file d’attente d’impression Laserprinter1 sur un hôte LPD sur 10.0.0.45 :  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[imprimer la référence des commandes](print-command-reference.md)  
