---
title: rundll32
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0639206b26ea58c4ec8473c0a736fda3c435021
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722240"
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

Rundll32 peut uniquement appeler des fonctions d’une DLL écrite explicitement pour être appelée par rundll32.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
