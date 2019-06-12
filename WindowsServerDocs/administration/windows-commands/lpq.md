---
title: lpq
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 18ff1ff3ecbc2df0a437ec8a465dec9a12123ede
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437513"
---
# <a name="lpq"></a>lpq

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche l’état de la file d’impression sur un ordinateur exécutant le démon LPD (Line printer).  

## <a name="syntax"></a>Syntaxe  
```  
lpq -S <ServerName> -P <printerName> [-l]  
```  
## <a name="parameters"></a>Paramètres  

|    Paramètre     |                                                                        Description                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| -S <ServerName>  | Spécifie (par nom ou adresse IP) de l’ordinateur ou un périphérique qui héberge la file d’attente d’impression LPD dont vous souhaitez afficher l’état de partage d’imprimante. Obligatoire. |
| -P <printerName> |                           Spécifie (par nom) l’imprimante pour la file d’attente à l’impression dont vous souhaitez afficher l’état. Obligatoire.                           |
|        -l        |                                      Spécifie que vous souhaitez afficher des détails sur l’état de la file d’attente.                                      |
|        /?        |                                                           Affiche l'aide à l'invite de commandes.                                                            |

## <a name="remarks"></a>Notes  
Le **-S** et **-P** paramètres respectent la casse et doivent être tapées en majuscules.  
## <a name="BKMK_examples"></a>Exemples  
Cet exemple montre comment afficher l’état de la file d’attente de Laserprinter1 imprimante sur un ordinateur hôte à 10.0.0.45 LPD :  
```  
lpq -S 10.0.0.45 -P Laserprinter1  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[Référence des commandes d’impression](print-command-reference.md)  
