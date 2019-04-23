---
title: rundll32
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5b1f288d21a1dcac25ecc00f685ea179d8a6542f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835030"
---
# <a name="rundll32"></a>rundll32



Charge et exécute 32 bits des bibliothèques de liens dynamiques (DLL). Il n’existe aucun paramètre configurable pour Rundll32. Informations d’aide sont fournies pour une DLL spécifique que vous exécutez avec le **rundll32** commande.

Vous devez exécuter le **rundll32** commande à partir d’une invite de commandes avec élévation de privilèges. Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

## <a name="syntax"></a>Syntaxe

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Commandes

|Paramètre|Description|
|---------|-----------|
|[Rundll32 printui.dll,PrintUIEntry](rundll32-printui.md)|Affiche l’interface utilisateur imprimante|

## <a name="remarks"></a>Notes

Rundll32 peut uniquement appeler des fonctions à partir d’une DLL qui sont explicitement écrits pour être appelée par Rundll32. Pour plus d’informations sur Rundll32 exigences consultez [article 164787](https://go.microsoft.com/fwlink/?LinkID=165773) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkID=165773).

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)