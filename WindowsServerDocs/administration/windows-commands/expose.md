---
title: poser
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 500dc5cfcd5e2bba4cfbc3cb5ef81a9065ea53cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725677"
---
# <a name="expose"></a>poser



Expose un cliché instantané persistant sous la forme d’une lettre de lecteur, d’un partage ou d’un point de montage.



## <a name="syntax"></a>Syntaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowID|Spécifie l’ID d’ombre du cliché instantané que vous souhaitez exposer.|
|\<Lecteur : >|Expose le cliché instantané spécifié comme une lettre de lecteur (par exemple, P :).|
|\<Partager>|Expose le cliché instantané spécifié sur un partage (par exemple, \\ \\ *MachineName*\).|
|\<> du MountPoint|Expose le cliché instantané spécifié à un point de montage (par exemple,\)C:\shadowcopy.|

## <a name="remarks"></a>Notes 

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="examples"></a>Exemples

Pour exposer le cliché instantané persistant associé à la variable d’environnement VSS_SHADOW_1 comme lecteur X, tapez :
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)