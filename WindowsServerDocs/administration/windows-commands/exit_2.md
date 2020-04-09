---
title: exit
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23d1044b-f5c1-4180-ae6d-f553b48da4d9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13cf7a7658394e59ce6cc7e66c3083cd3d359574
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844882"
---
# <a name="exit"></a>exit

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quitte le programme cmd. exe (l’interpréteur de commandes) ou le script de traitement par lots actuel.  
Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).  
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
Pour fermer l’interpréteur de commandes, cmd. exe, tapez :  
```  
exit  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  

