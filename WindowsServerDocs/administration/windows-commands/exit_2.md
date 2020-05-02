---
title: exit
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e47dfa42f2bacb3fe9f12d1da9163bcf828e9488
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725703"
---
# <a name="exit"></a>exit

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quitte le programme cmd. exe (l’interpréteur de commandes) ou le script de traitement par lots actuel.  
  
## <a name="syntax"></a>Syntaxe  
```  
exit [/b] [<exitCode>]  
```  
### <a name="parameters"></a>Paramètres  

| Paramètre  |                                                                                         Description                                                                                          |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /b     |                                      Quitte le script de traitement par lots en cours au lieu de quitter cmd. exe. S’il est exécuté en dehors d’un script de commandes, quitte cmd. exe.                                      |
| `<exitCode>` | Spécifie un nombre numérique. Si **/b** est spécifié, la variable d’environnement ERRORLEVEL est définie sur ce nombre. Si vous quittez **cmd. exe**, le code de sortie du processus est défini sur ce nombre. |
|     /?     |                                                                             Affiche l'aide à l'invite de commandes.                                                                             |

## <a name="examples"></a>Exemples  
Pour fermer l’interpréteur de commandes, cmd. exe, tapez :  
```  
exit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  

