---
title: poser
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55cc7a292b81977a346f3f078a3b5623243ea46c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844802"
---
# <a name="expose"></a>poser



expose un cliché instantané persistant sous la forme d’une lettre de lecteur, d’un partage ou d’un point de montage.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ShadowID|Spécifie l’ID d’ombre du cliché instantané que vous souhaitez exposer.|
|Lecteur \<: >|Expose le cliché instantané spécifié comme une lettre de lecteur (par exemple, P :).|
|Partage de \<>|Expose le cliché instantané spécifié sur un partage (par exemple, \\\\*MachineName*\).|
|\<le MountPoint >|Expose le cliché instantané spécifié à un point de montage (par exemple, C:\shadowcopy\).|

## <a name="remarks"></a>Notes

-   Vous pouvez utiliser un alias existant ou une variable d’environnement à la place de *ShadowID*. Utilisez **Ajouter** sans paramètres pour afficher les alias existants.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour exposer le cliché instantané persistant associé à la variable d’environnement VSS_SHADOW_1 comme lecteur X, tapez :
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)