---
title: ntcmdprompt
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0063bdbb-dc2b-41c4-99ee-b011603aaa86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25afab00ced3cb14771c18aa38c7fd8c98aecc0c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838042"
---
# <a name="ntcmdprompt"></a>ntcmdprompt

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exécute l’interpréteur de commandes **cmd. exe**, au lieu de **Command.com**, après l’exécution d’un TSR (Terminate and Start Resident) ou après le démarrage de l’invite de commandes à partir d’une application ms-dos.
## <a name="syntax"></a>Syntaxe
```
ntcmdprompt
```
#### <a name="parameters"></a>Paramètres

| Paramètre |             Description              |
|-----------|--------------------------------------|
|    /?     | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes
- Quand **Command.com** est en cours d’exécution, certaines fonctionnalités de **cmd. exe**, telles que l’affichage **doskey** de l’historique de commande, ne sont pas disponibles. Si vous préférez exécuter l’interpréteur de commandes **cmd. exe** après avoir démarré un TSR (Terminate and Rest Resident) ou démarré l’invite de commandes à partir d’une application basée sur MS-DOS, vous pouvez utiliser la commande **ntcmdprompt** . Toutefois, gardez à l’esprit que le TSR peut ne pas être disponible pour une utilisation lorsque vous exécutez **cmd. exe**. Vous pouvez inclure la commande **ntcmdprompt** dans votre fichier **config. NT** ou le fichier de démarrage personnalisé équivalent dans le fichier d’informations de programme d’une application (PIF).
  ## <a name="examples"></a>Exemples
  Pour inclure **ntcmdprompt** dans votre fichier **config. NT** , ou le fichier de démarrage de configuration spécifié dans le fichier PIF, tapez : **ntcmdprompt**
  ## <a name="additional-references"></a>Références supplémentaires
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

