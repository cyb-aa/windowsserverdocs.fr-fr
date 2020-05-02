---
title: BdeHdCfg quiet
description: Rubrique de référence pour la commande BdeHdCfg quiet, qui indique à BdeHdCfg de ne pas afficher toutes les actions et les erreurs.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb7a73899259b0f3823941ece014ea85568a4ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718636"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg : quiet

Informe l’outil en ligne de commande BdeHdCfg que toutes les actions et toutes les erreurs ne doivent pas être affichées dans l’interface de ligne de commande. Les invites oui/non (Y/N) affichées pendant la préparation du lecteur supposent une réponse « oui ». Pour afficher toute erreur qui s'est produite pendant la préparation du lecteur, examinez le journal des événements système sous le fournisseur d'événements **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="syntax"></a>Syntaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -quiet
```

#### <a name="parameters"></a>Paramètres

Cette commande n’a pas de paramètres supplémentaires.

## <a name="examples"></a>Exemples

Pour utiliser la commande **Quiet** :

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
