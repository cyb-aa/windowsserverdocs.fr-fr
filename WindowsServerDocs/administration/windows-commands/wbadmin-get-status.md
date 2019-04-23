---
title: WBADMIN get état
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35fd640aa56bca7c5f5d6f3901fe095d0b8a73cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863410"
---
# <a name="wbadmin-get-status"></a>WBADMIN get état



Signale l’état de l’opération de sauvegarde ou de récupération est en cours d’exécution.

Pour utiliser cette sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin get status
```

## <a name="parameters"></a>Paramètres

La sous-commande n’a aucun paramètre.

## <a name="remarks"></a>Notes

-   La sous-commande n’arrêtera pas jusqu'à ce que la sauvegarde en cours ou l’opération de récupération est terminée, la sous-commande continueront à s’exécuter même si vous fermez la fenêtre de commande.
-   Si vous souhaitez arrêter la sauvegarde en cours ou l’opération de récupération, utilisez la **wbadmin stop travail** sous-commande.

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx) applet de commande