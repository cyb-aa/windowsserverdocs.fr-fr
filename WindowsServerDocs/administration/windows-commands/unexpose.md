---
title: cessez
description: Commande Windows pour l’inexposition, qui inpose un cliché instantané qui a été exposé à l’aide de la commande exposer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2f8bbdb3b810ffbf9332608a016fc3b3e188e9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832352"
---
# <a name="unexpose"></a>cessez

n’expose pas de cliché instantané qui a été exposé à l’aide de la commande **exposer** . Le cliché instantané exposé peut être spécifié par son ID d’ombre, sa lettre de lecteur, son partage ou son point de montage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowID >|N’expose pas le cliché instantané spécifié par l’ID d’ombre donné.|
|Lecteur \<: >|N’expose pas le cliché instantané associé à la lettre de lecteur spécifiée (par exemple, le lecteur P).|
|Partage de \<>|N’expose pas le cliché instantané associé au partage spécifié (par exemple, \\\\*MachineName*\).|
|\<le MountPoint >|N’expose pas le cliché instantané associé au point de montage spécifié (par exemple, C:\shadowcopy\).|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour ne pas exposer le cliché instantané associé au lecteur P, tapez :
```
unexpose P:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)