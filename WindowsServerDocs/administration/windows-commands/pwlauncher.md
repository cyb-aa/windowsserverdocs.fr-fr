---
title: pwlauncher
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0917bb7b-408a-40f7-a1c5-20e94c10d38b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4cf65643e0c5a4b28e06619a8156792b6e5456ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371969"
---
# <a name="pwlauncher"></a>pwlauncher



Active ou désactive les options de démarrage de Windows to Go (pwlauncher). L’outil de ligne de commande **pwlauncher** vous permet de configurer l’ordinateur pour qu’il démarre automatiquement dans un espace de travail Windows to Go (en supposant qu’il en existe un), sans que vous ayez besoin d’entrer votre microprogramme ou de modifier vos options de démarrage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/Enable|Active les options de démarrage de Windows to Go afin que l’ordinateur démarre automatiquement à partir d’un périphérique USB lorsqu’il est présent|
|/Disable|Désactive les options de démarrage de Windows to Go afin que l’ordinateur ne puisse pas être démarré à partir d’un périphérique USB, sauf s’il est configuré manuellement dans le microprogramme.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Le plus grand obstacle pour un utilisateur souhaitant utiliser Windows to Go est de faire en sorte que son ordinateur démarre à partir de l’USB. Pour ce faire, il suffit d’entrer le microprogramme et d’essayer différentes options de configuration jusqu’à ce que l’ordinateur soit configuré correctement. Ce n’est pas une simple entreprise pour la plupart des utilisateurs et est extrêmement risqué, car le microprogramme contient des options qui peuvent rendre un système inutilisable s’il est utilisé de manière incorrecte. Pour éviter ce problème, les systèmes d’exploitation Windows 8and ultérieurs incluent une fonctionnalité nommée « options de démarrage Windows to Go » qui permet à un utilisateur de configurer son ordinateur pour qu’il démarre à partir d’un périphérique USB depuis Windows, sans jamais entrer dans son microprogramme, à condition que leur le microprogramme prend en charge le démarrage à partir d’USB. L’activation d’un système pour toujours démarrer à partir de USB a d’abord des implications que vous devez prendre en compte. Par exemple, un périphérique USB qui comprend des logiciels malveillants peut être amorcé par inadvertance pour compromettre le système ou plusieurs lecteurs USB peuvent être branchés pour provoquer un conflit de démarrage. Pour cette raison, les options de démarrage de Windows to Go sont désactivées par défaut dans la configuration par défaut. En outre, des privilèges d’administrateur sont nécessaires pour configurer les options de démarrage de Windows to go. Si vous activez les options de démarrage Windows to Go à l’aide de l’outil en ligne de commande pwlauncher ou de l’application **modifier les options de démarrage Windows to Go** , l’ordinateur tente de démarrer à partir de n’importe quel périphérique USB inséré dans l’ordinateur avant son démarrage.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant montre comment vous pouvez utiliser la commande **pwlauncher** pour activer le démarrage à partir de l’USB :
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)