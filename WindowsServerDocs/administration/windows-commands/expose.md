---
title: exposer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 51cc744bc2b61862ed05ca2e7d0aaa8f70d38692
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886660"
---
# <a name="expose"></a>exposer



expose un cliché instantané persistant comme lettre de lecteur, le partage ou le point de montage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowID|Spécifie l’ID de l’ombre de la copie de clichés instantanés que vous souhaitez exposer.|
|\<Lecteur : >|Expose le cliché instantané spécifié sous la forme d’une lettre de lecteur (par exemple, P:).|
|\<Partage >|Expose le cliché instantané spécifié à un partage (par exemple, \\ \\ *MachineName*\).|
|\<MountPoint>|Expose le cliché instantané spécifié à un point de montage (par exemple, C:\shadowcopy\).|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **ajouter** sans paramètres pour voir les alias existants.

## <a name="BKMK_examples"></a>Exemples

Pour exposer le cliché instantané persistant associé à la variable d’environnement VSS_SHADOW_1 en tant que lecteur X, tapez :
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)