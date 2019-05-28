---
title: pwlauncher
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8ec9748056b296bb0c74250b36c762fb86fa90ad
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564650"
---
# <a name="pwlauncher"></a>pwlauncher



Active ou désactive le Windows aux Options de démarrage Go (pwlauncher). Le **pwlauncher** outil de ligne de commande vous permet de configurer l’ordinateur pour démarrer automatiquement dans un espace de travail Windows To Go (en supposant qu’il est présent), sans avoir à entrer votre microprogramme ou modifier vos options de démarrage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Pwlauncher {/enable | /disable}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/Enable|Active les options de démarrage Windows To Go afin que l’ordinateur démarre automatiquement à partir d’un périphérique USB lorsqu’il est présent|
|/disable|Désactive les options de démarrage Windows To Go afin que l’ordinateur ne peut pas être démarré à partir d’un périphérique USB, sauf si configurées manuellement dans le microprogramme.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Le plus gros obstacle pour un utilisateur souhaitant utiliser Windows To Go est bien leur ordinateur pour démarrer à partir d’USB. Cela se fait habituellement en entrant le microprogramme et en essayant différentes options de configuration jusqu'à ce que l’ordinateur est configuré correctement. Cela n’est pas un simple pour la plupart des utilisateurs et est extrêmement délicate car le microprogramme contient les options qui peuvent rendre un système inutilisable si utilisée de manière incorrecte. Pour aider à alléger ce problème, le Windows 8and des systèmes d’exploitation ultérieurs incluent une nouvelle fonctionnalité nommée « Windows aux Options de démarrage Go » qui permet à un utilisateur à configurer leur ordinateur pour démarrer à partir d’USB à partir de Windows-sans jamais entrer leur microprogramme, tant que leur microprogramme prend en charge le démarrage à partir d’USB. L’activation d’un système de toujours démarrer à partir d’USB a tout d’abord des conséquences que vous devez prendre en compte. Par exemple, un périphérique USB qui inclut les logiciels malveillants peut être démarré par inadvertance pour compromettre le système, ou plusieurs lecteurs USB peuvent être branchés à provoquer un conflit de démarrage. Pour cette raison, la configuration par défaut a le Windows aux Options de démarrage Go désactivée par défaut. En outre, les privilèges d’administrateur sont requis pour configurer Windows à Options de démarrage Go. Si vous activez les options de démarrage Windows To Go à l’aide de l’outil de ligne de commande pwlauncher ou **modification Windows aux Options de démarrage Go** application tentera de démarrer à partir de n’importe quel périphérique USB qui est insérée dans l’ordinateur avant qu’il soit l’ordinateur démarré.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant montre comment vous pouvez utiliser la **pwlauncher** commande pour activer le démarrage à partir d’USB :
```
Pwlauncher /enable
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)