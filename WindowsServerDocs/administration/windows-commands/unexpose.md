---
title: cessez
description: Rubrique de référence pour l’inexposition, qui inpose un cliché instantané qui a été exposé à l’aide de la commande exposer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0caa412e5ff7de149f0a2bd8806f7141c368306
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721190"
---
# <a name="unexpose"></a>cessez

N’expose pas de cliché instantané qui a été exposé à l’aide de la commande **exposer** . Le cliché instantané exposé peut être spécifié par son ID d’ombre, sa lettre de lecteur, son partage ou son point de montage.



## <a name="syntax"></a>Syntaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowID>|N’expose pas le cliché instantané spécifié par l’ID d’ombre donné.|
|\<Lecteur : >|N’expose pas le cliché instantané associé à la lettre de lecteur spécifiée (par exemple, le lecteur P).|
|\<Partager>|N’expose pas le cliché instantané associé au partage spécifié (par exemple, \\ \\ *MachineName*\).|
|\<> du MountPoint|N’expose pas le cliché instantané associé au point de montage spécifié (par exemple,\)C:\shadowcopy.|

## <a name="remarks"></a>Notes 

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="examples"></a>Exemples

Pour ne pas exposer le cliché instantané associé au lecteur P, tapez :
```
unexpose P:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)