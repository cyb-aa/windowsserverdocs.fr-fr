---
title: exit
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 105bf572c1ebeb37ea59ff8bc5c04121d2442341
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377348"
---
# <a name="exit"></a>exit

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

quitte le programme cmd. exe (l’interpréteur de commandes) ou le script de traitement par lots actuel.  
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).  
## <a name="syntax"></a>Syntaxe  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Paramètres  

| Paramètre  |                                                                                         Description                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      quitte le script de traitement par lots en cours au lieu de quitter cmd. exe. S’il est exécuté en dehors d’un script de commandes, quitte cmd. exe.                                      |
| <exitCode> | Spécifie un nombre numérique. Si **/b** est spécifié, la variable d’environnement ERRORLEVEL est définie sur ce nombre. Si vous quittez **cmd. exe**, le code de sortie du processus est défini sur ce nombre. |
|     /?     |                                                                             Affiche l'aide à l'invite de commandes.                                                                             |

## <a name="BKMK_examples"></a>Illustre  
Pour fermer l’interpréteur de commandes, cmd. exe, tapez :  
```  
exit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  

