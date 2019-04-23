---
title: pnputil
description: Découvrez comment gérer le magasin de pilotes avec l’utilitaire pnputil.exe.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fab686b8-09d3-4f6c-afa2-630e6036f44c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5bde78d97be8def9f8594572869c34ef213db480
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862540"
---
# <a name="pnputil"></a>pnputil

Pnputil.exe est un utilitaire de ligne de commande que vous pouvez utiliser pour gérer le magasin de pilotes. Vous pouvez utiliser Pnputil pour ajouter des packages de pilotes, de supprimer des packages de pilotes et packages de pilotes de liste qui se trouvent dans le magasin.

## <a name="syntax"></a>Syntaxe

```
pnputil.exe [-f | -i] [ -? | -a | -d | -e ] <INF name>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-a|Spécifie l’ajout du fichier INF identifié.|
|-d|Spécifie pour supprimer le fichier INF identifié.|
|-e|Spécifie pour énumérer tous les fichiers INF de fournisseurs tiers.|
|-f|Spécifie pour forcer la suppression du fichier INF identifié. Ne peut pas être utilisé conjointement avec le **– i** paramètre.|
|-i|Spécifie que le fichier INF identifié. Ne peut pas être utilisé conjointement avec le **-f** paramètre.|
|/?|Affiche l'aide à l'invite de commandes.|


## <a name="examples"></a>Exemples

-   pnputil.exe - un a:\usbcam\USBCAM. INF ajoute le fichier INF qui est spécifié par USBCAM.inf. INF
-   pnputil.exe - un c:\drivers\*.inf ajoute tous les fichiers INF dans c:\drivers\
-   pnputil.exe -i-a:\usbcam\USBCAM. INF ajoute et installe le pilote spécifié.
-   pnputil.exe – e énumère tous les pilotes tiers.
-   oem0.inf -d pnputil.exe supprime le texte spécifié.
-   pnputil.exe -f -d oem0.inf force la suppression du fichier INF spécifié.

## <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Popd](popd.md)