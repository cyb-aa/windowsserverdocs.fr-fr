---
title: poser
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 819484364e8375c4d58e4d022681eedeaa7084ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377277"
---
# <a name="expose"></a>poser



Expose un cliché instantané persistant sous la forme d’une lettre de lecteur, d’un partage ou d’un point de montage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowID|Spécifie l’ID d’ombre du cliché instantané que vous souhaitez exposer.|
|Lecteur \<: >|Expose le cliché instantané spécifié comme une lettre de lecteur (par exemple, P :).|
|Partage de \<>|Expose le cliché instantané spécifié sur un partage (par exemple, \\\\*MachineName*\).|
|\<le MountPoint >|Expose le cliché instantané spécifié à un point de montage (par exemple, C:\shadowcopy\).|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="BKMK_examples"></a>Illustre

Pour exposer le cliché instantané persistant associé à la variable d’environnement VSS_SHADOW_1 comme lecteur X, tapez :
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)