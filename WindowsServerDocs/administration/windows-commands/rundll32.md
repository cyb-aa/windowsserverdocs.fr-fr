---
title: rundll32
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 391607f6e744fe88a60cb556067cf2699eee25f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835432"
---
# <a name="rundll32"></a>rundll32



Charge et exécute les bibliothèques de liens dynamiques (dll) 32 bits. Il n’existe aucun paramètre configurable pour rundll32. Des informations d’aide sont fournies pour une DLL spécifique que vous exécutez à l’aide de la commande **rundll32** .

Vous devez exécuter la commande **rundll32** à partir d’une invite de commandes avec élévation de privilèges. Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

## <a name="syntax"></a>Syntaxe

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Commands

|Paramètre|Description|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Affiche l’interface utilisateur de l’imprimante|

## <a name="remarks"></a>Notes

Rundll32 peut uniquement appeler des fonctions d’une DLL écrite explicitement pour être appelée par rundll32.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
