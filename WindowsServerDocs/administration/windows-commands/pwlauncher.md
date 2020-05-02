---
title: pwlauncher
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ddda3ab7831643c3c2c096ae87893d97f90155c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722751"
---
# <a name="pwlauncher"></a>pwlauncher



Active ou désactive les options de démarrage de Windows to Go (pwlauncher). L’outil de ligne de commande **pwlauncher** vous permet de configurer l’ordinateur pour qu’il démarre automatiquement dans un espace de travail Windows to Go (en supposant qu’il en existe un), sans que vous ayez besoin d’entrer votre microprogramme ou de modifier vos options de démarrage.



## <a name="syntax"></a>Syntaxe

```
Pwlauncher {/enable | /disable}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/Enable|Active les options de démarrage de Windows to Go afin que l’ordinateur démarre automatiquement à partir d’un périphérique USB lorsqu’il est présent|
|/Disable|Désactive les options de démarrage de Windows to Go afin que l’ordinateur ne puisse pas être démarré à partir d’un périphérique USB, sauf s’il est configuré manuellement dans le microprogramme.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Le plus grand obstacle pour un utilisateur souhaitant utiliser Windows to Go est de faire en sorte que son ordinateur démarre à partir de l’USB. Pour ce faire, il suffit d’entrer le microprogramme et d’essayer différentes options de configuration jusqu’à ce que l’ordinateur soit configuré correctement. Ce n’est pas une simple entreprise pour la plupart des utilisateurs et est extrêmement risqué, car le microprogramme contient des options qui peuvent rendre un système inutilisable s’il est utilisé de manière incorrecte. Pour éviter ce problème, les systèmes d’exploitation Windows 8and ultérieurs incluent une fonctionnalité nommée Windows to Go Startup options qui permet à un utilisateur de configurer son ordinateur pour qu’il démarre à partir d’un périphérique USB depuis Windows, sans jamais entrer dans son microprogramme, à condition que le microprogramme prenne en charge le démarrage à partir de l’USB. L’activation d’un système pour toujours démarrer à partir de USB a d’abord des implications que vous devez prendre en compte. Par exemple, un périphérique USB qui comprend des logiciels malveillants peut être amorcé par inadvertance pour compromettre le système ou plusieurs lecteurs USB peuvent être branchés pour provoquer un conflit de démarrage. Pour cette raison, les options de démarrage de Windows to Go sont désactivées par défaut dans la configuration par défaut. En outre, des privilèges d’administrateur sont nécessaires pour configurer les options de démarrage de Windows to go. Si vous activez les options de démarrage Windows to Go à l’aide de l’outil en ligne de commande pwlauncher ou de l’application **modifier les options de démarrage Windows to Go** , l’ordinateur tente de démarrer à partir de n’importe quel périphérique USB inséré dans l’ordinateur avant son démarrage.

## <a name="examples"></a>Exemples

Pour illustrer la façon dont vous pouvez utiliser la commande **pwlauncher** pour activer le démarrage à partir d’USB :
```
Pwlauncher /enable
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)