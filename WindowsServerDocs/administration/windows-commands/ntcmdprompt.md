---
title: ntcmdprompt
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fef1641bf1b48bd1fe4aaf284ed309ab4d4d5f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372670"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Exécute l’interpréteur de commandes **cmd. exe**, au lieu de **Command.com**, après l’exécution d’un TSR (Terminate and Start Resident) ou après le démarrage de l’invite de commandes à partir d’une application ms-dos.
## <a name="syntax"></a>Syntaxe
```
ntcmdprompt
```
### <a name="parameters"></a>Paramètres

| Paramètre |             Description              |
|-----------|--------------------------------------|
|    /?     | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes
- Quand **Command.com** est en cours d’exécution, certaines fonctionnalités de **cmd. exe**, telles que l’affichage **doskey** de l’historique de commande, ne sont pas disponibles. Si vous préférez exécuter l’interpréteur de commandes **cmd. exe** après avoir démarré un TSR (Terminate and Rest Resident) ou démarré l’invite de commandes à partir d’une application basée sur MS-DOS, vous pouvez utiliser la commande **ntcmdprompt** . Toutefois, gardez à l’esprit que le TSR peut ne pas être disponible pour une utilisation lorsque vous exécutez **cmd. exe**. Vous pouvez inclure la commande **ntcmdprompt** dans votre fichier **config. NT** ou le fichier de démarrage personnalisé équivalent dans le fichier d’informations de programme d’une application (PIF).
  ## <a name="examples"></a>Exemples
  Pour inclure **ntcmdprompt** dans votre fichier **config. NT** , ou le fichier de démarrage de configuration spécifié dans le fichier PIF, tapez : **ntcmdprompt**
  ## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

