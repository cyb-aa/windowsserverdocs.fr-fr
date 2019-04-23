---
title: Cessez d’exposer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fe9cb5dfd8ae6c71fdc72ddc1e8421229f98f5d0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837470"
---
# <a name="unexpose"></a>Cessez d’exposer



unexposes un cliché instantané qui a été exposé à l’aide de la **exposer** commande. Le cliché instantané exposé peut être spécifié par son ID du cliché instantané, une lettre de lecteur, un partage ou un point de montage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowID>|Unexposes le cliché instantané spécifié par l’ID de cliché instantané donné.|
|\<Lecteur : >|Unexposes le cliché instantané associé à la lettre de lecteur spécifiée (par exemple, le lecteur P).|
|\<Partage >|Unexposes le cliché instantané associé au partage spécifié (par exemple, \\ \\ *MachineName*\).|
|\<MountPoint>|Unexposes le cliché instantané associé au point de montage spécifié (par exemple, C:\shadowcopy\).|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **ajouter** sans paramètres pour voir les alias existants.

## <a name="BKMK_examples"></a>Exemples

Pour cessez d’exposer le cliché instantané associé à P de lecteur, tapez :
```
unexpose P:
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)