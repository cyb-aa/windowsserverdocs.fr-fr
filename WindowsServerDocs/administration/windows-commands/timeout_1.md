---
title: timeout
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3997399b732c494050797c83a0a52938574986bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830170"
---
# <a name="timeout"></a>timeout



Interrompt le processeur de commandes pour le nombre de secondes spécifié.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/t \<TimeoutInSeconds>|Spécifie le nombre de secondes (entre -1 et 99 999) d’attente avant que le processeur de commandes continue le traitement. La valeur -1, l’ordinateur d’attendre indéfiniment une frappe au clavier.|
|/NOBREAK.|Indique d’ignorer les frappes de touche d’utilisateur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **délai d’attente** commande est généralement utilisée dans les fichiers de commandes.
-   Une séquence de touches utilisateur reprend l’exécution de processeur de commande immédiatement, même si le délai d’expiration n’a pas expiré.
-   Lorsqu’il est utilisé conjointement avec le **veille** commande, **délai d’attente** est similaire à la **suspendre** commande.

## <a name="BKMK_examples"></a>Exemples

Pour suspendre l’interpréteur de commandes pour les dix secondes, tapez :
```
timeout /t 10
```
Pour mettre en pause le processeur de commandes pendant 100 secondes et ignorer les séquences de touches, tapez :
```
timeout /t 100 /nobreak
```
Pour suspendre l’interpréteur de commandes jusqu'à ce qu’une touche est enfoncée, tapez :
```
timeout /t -1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
