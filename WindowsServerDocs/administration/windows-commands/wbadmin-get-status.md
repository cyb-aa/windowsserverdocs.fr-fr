---
title: État de l’extraction de Wbadmin
description: Rubrique de référence relative à l’état de l’opération Wbadmin obtenir, qui indique l’état de la sauvegarde ou de la récupération en cours d’exécution.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e8e5194cd49770f72ce810f4652d9bc98af75e2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720156"
---
# <a name="wbadmin-get-status"></a>État de l’extraction de Wbadmin



Indique l’état de l’opération de sauvegarde ou de récupération en cours d’exécution.

Pour utiliser cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin get status
```

### <a name="parameters"></a>Paramètres

Cette sous-commande n’a aucun paramètre.

## <a name="remarks"></a>Notes 

-   Cette sous-commande ne s’arrête pas tant que l’opération de sauvegarde ou de récupération n’est pas terminée ; la sous-commande continue de s’exécuter même si vous fermez la fenêtre de commande.
-   Si vous souhaitez arrêter l’opération de sauvegarde ou de récupération en cours, utilisez la sous-commande **Wbadmin Stop Job** .

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande WBJob](https://technet.microsoft.com/library/jj902426.aspx)