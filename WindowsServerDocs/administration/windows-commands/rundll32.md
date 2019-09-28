---
title: rundll32
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 29a87f9f07c25a0c671e47550e0a054d8308f747
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384419"
---
# <a name="rundll32"></a>rundll32



Charge et exécute les bibliothèques de liens dynamiques (dll) 32 bits. Il n’existe aucun paramètre configurable pour rundll32. Des informations d’aide sont fournies pour une DLL spécifique que vous exécutez à l’aide de la commande **rundll32** .

Vous devez exécuter la commande **rundll32** à partir d’une invite de commandes avec élévation de privilèges. Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

## <a name="syntax"></a>Syntaxe

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Commandes

|Paramètre|Description|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Affiche l’interface utilisateur de l’imprimante|

## <a name="remarks"></a>Notes

Rundll32 peut uniquement appeler des fonctions à partir d’une DLL qui est explicitement écrite pour être appelée par rundll32. Pour plus d’informations sur les conditions requises pour rundll32, consultez [l’article 164787](https://go.microsoft.com/fwlink/?LinkID=165773) de la base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkID=165773).

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)