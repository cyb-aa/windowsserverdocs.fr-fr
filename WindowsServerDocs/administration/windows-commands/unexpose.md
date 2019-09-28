---
title: cessez
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e10126739ef82b060e271e9b804a77658b5ec82
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392265"
---
# <a name="unexpose"></a>cessez



N’expose pas de cliché instantané qui a été exposé à l’aide de la commande **exposer** . Le cliché instantané exposé peut être spécifié par son ID d’ombre, sa lettre de lecteur, son partage ou son point de montage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0ShadowID >|N’expose pas le cliché instantané spécifié par l’ID d’ombre donné.|
|\<Drive : >|N’expose pas le cliché instantané associé à la lettre de lecteur spécifiée (par exemple, le lecteur P).|
|@no__t 0Share >|N’expose pas le cliché instantané associé au partage spécifié (par exemple, \\ @ no__t-1*MachineName*\).|
|@no__t 0MountPoint >|N’expose pas le cliché instantané associé au point de montage spécifié (par exemple, C:\shadowcopy @ no__t-0.|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="BKMK_examples"></a>Illustre

Pour ne pas exposer le cliché instantané associé au lecteur P, tapez :
```
unexpose P:
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)