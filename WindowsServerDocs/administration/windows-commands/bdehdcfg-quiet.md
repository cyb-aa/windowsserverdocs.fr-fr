---
title: bdehdcfg silencieux
description: Rubrique de commandes de Windows pour le mode silencieux bdehdcfg - indique bdehdcfg pour ne pas afficher toutes les actions et erreurs.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0f0d98f6ae76e9bf6357689c97e091766b9645c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865750"
---
# <a name="bdehdcfg-quiet"></a>bdehdcfg : silencieux



Informe l’outil de ligne de commande Bdehdcfg que toutes les actions et les erreurs ne doivent ne pas être affiché dans l’interface de ligne de commande. Pour obtenir un exemple d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

### <a name="parameters"></a>Paramètres

Cette commande n’accepte aucuns paramètres supplémentaires.

## <a name="remarks"></a>Notes

Si des invites Oui/Non (O/N) avaient été affichées pendant la préparation de lecteur, une réponse « Oui » est supposée. Pour afficher toute erreur qui s'est produite pendant la préparation du lecteur, examinez le journal des événements système sous le fournisseur d'événements **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant illustre l’utilisation de la **silencieux** commande.
```
bdehdcfg -target default -quiet
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)