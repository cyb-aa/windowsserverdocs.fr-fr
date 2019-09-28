---
title: BdeHdCfg quiet
description: La rubrique commandes Windows pour BdeHdCfg quiet-indique à BdeHdCfg de ne pas afficher toutes les actions et les erreurs.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d59a14e34200e3fa8e18e36e166ef62ceca1afe7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382219"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg : quiet



Informe l’outil en ligne de commande BdeHdCfg que toutes les actions et toutes les erreurs ne doivent pas être affichées dans l’interface de ligne de commande. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Paramètres

Cette commande ne prend pas de paramètres supplémentaires.

## <a name="remarks"></a>Notes

Si des invites Oui/Non (O/N) avaient été affichées pendant la préparation de lecteur, une réponse « Oui » est supposée. Pour afficher toute erreur qui s'est produite pendant la préparation du lecteur, examinez le journal des événements système sous le fournisseur d'événements **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **Quiet** .
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)