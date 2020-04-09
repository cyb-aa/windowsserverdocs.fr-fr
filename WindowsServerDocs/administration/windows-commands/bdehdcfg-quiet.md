---
title: BdeHdCfg quiet
description: La rubrique commandes Windows pour **BdeHdCfg quiet**, qui indique à BdeHdCfg de ne pas afficher toutes les actions et les erreurs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9c24d8861476e6c1578af8245236d699b6ef6db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851042"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg : quiet

Informe l’outil en ligne de commande BdeHdCfg que toutes les actions et toutes les erreurs ne doivent pas être affichées dans l’interface de ligne de commande. Pour obtenir un exemple de la façon dont cette commande peut être utilisée, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

#### <a name="parameters"></a>Paramètres

Cette commande ne prend pas de paramètres supplémentaires.

## <a name="remarks"></a>Notes

Si des invites Oui/Non (O/N) avaient été affichées pendant la préparation de lecteur, une réponse « Oui » est supposée. Pour afficher toute erreur qui s'est produite pendant la préparation du lecteur, examinez le journal des événements système sous le fournisseur d'événements **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="examples"></a><a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre l’utilisation de la commande **Quiet** .

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)