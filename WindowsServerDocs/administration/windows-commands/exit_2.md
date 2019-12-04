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
ms.openlocfilehash: d6d3a4d0bfc74644f6fda43abe57e0e4e7c1264a
ms.sourcegitcommit: 4a03f263952c993dfdf339dd3491c73719854aba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74791231"
---
# <a name="exit"></a>exit

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quitte le programme cmd. exe (l’interpréteur de commandes) ou le script de traitement par lots actuel.  
Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).  
## <a name="syntax"></a>Syntaxe  
```  
exit [/b] [<exitCode>]  
```  
## <a name="parameters"></a>Paramètres  

| Paramètre  |                                                                                         Description                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      Quitte le script de traitement par lots en cours au lieu de quitter cmd. exe. S’il est exécuté en dehors d’un script de commandes, quitte cmd. exe.                                      |
| `<exitCode>` | Spécifie un nombre numérique. Si **/b** est spécifié, la variable d’environnement ERRORLEVEL est définie sur ce nombre. Si vous quittez **cmd. exe**, le code de sortie du processus est défini sur ce nombre. |
|     /?     |                                                                             Affiche l'aide à l'invite de commandes.                                                                             |

## <a name="BKMK_examples"></a>Illustre  
Pour fermer l’interpréteur de commandes, cmd. exe, tapez :  
```  
exit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  

