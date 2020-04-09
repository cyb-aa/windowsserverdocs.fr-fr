---
title: pnputil
description: Découvrez comment gérer le magasin de pilotes avec l’utilitaire PnPutil. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 134e6ce4b1fc44450047de3287b7daac67da4b6a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837512"
---
# <a name="pnputil"></a>pnputil

PnPUtil. exe est un utilitaire de ligne de commande que vous pouvez utiliser pour gérer le magasin de pilotes. Vous pouvez utiliser PNPUtil pour ajouter des packages de pilotes, supprimer des packages de pilotes et répertorier les packages de pilotes qui se trouvent dans le Windows Store.

## <a name="syntax"></a>Syntaxe

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-a|Spécifie d’ajouter le fichier INF identifié.|
|-d|Spécifie de supprimer le fichier INF identifié.|
|-e|Spécifie l’énumération de tous les fichiers INF tiers.|
|-f|Spécifie de forcer la suppression du fichier INF identifié. Ne peut pas être utilisé conjointement avec le paramètre **– i** .|
|-i|Spécifie l’installation du fichier INF identifié. Ne peut pas être utilisé conjointement avec le paramètre **-f** .|
|/?|Affiche l'aide à l'invite de commandes.|


## <a name="examples"></a>Exemples

-   PnPUtil. exe-a a:\usbcam\USBCAM. INF ajoute le fichier INF spécifié par USBCAM. FICHIER
-   PnPUtil. exe-a c:\drivers\*. inf ajoute tous les fichiers INF dans c:\drivers\
-   PnPUtil. exe-i-a a:\usbcam\USBCAM. INF ajoute et installe le pilote spécifié.
-   PnPUtil. exe – e énumère tous les pilotes tiers.
-   PnPUtil. exe-d Oem0. inf supprime le spécifié.
-   PnPUtil. exe-f-d Oem0. inf force la suppression du fichier INF spécifié.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Popd](popd.md)