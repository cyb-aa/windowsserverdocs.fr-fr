---
title: délai d'expiration
description: Rubrique de référence pour Timeout, qui suspend le processeur de commandes pendant le nombre de secondes spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e26b4a84-0e30-46e1-aa10-0667b7d3cb4c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed66342c4f0bbe22e9d2dc6440d291941c769cd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721347"
---
# <a name="timeout"></a>délai d'expiration

Suspend le processeur de commandes pendant le nombre de secondes spécifié.



## <a name="syntax"></a>Syntaxe

```
timeout /t <TimeoutInSeconds> [/nobreak] 
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/t \<TimeoutInSeconds>|Spécifie le nombre de secondes (entre-1 et 99999) à attendre avant que le processeur de commande continue le traitement. La valeur-1 provoque l’attente indéfinie de l’ordinateur pour une séquence de touches.|
|/nobreak|Spécifie d’ignorer les frappes de touches utilisateur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   La commande **timeout** est généralement utilisée dans les fichiers de commandes.
-   Une séquence de touches utilisateur reprend immédiatement l’exécution du processeur de commandes, même si le délai d’expiration n’a pas expiré.
-   Lorsqu’il est utilisé conjointement avec la commande **Sleep** , le **délai d’attente** est semblable à celui de la commande **Pause** .

## <a name="examples"></a>Exemples

Pour suspendre le processeur de commandes pendant dix secondes, tapez :
```
timeout /t 10
```
Pour suspendre l’interpréteur de commandes pendant 100 secondes et ignorer les séquences de touches, tapez :
```
timeout /t 100 /nobreak
```
Pour suspendre indéfiniment le processeur de commandes jusqu’à ce qu’une touche soit enfoncée, tapez :
```
timeout /t -1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
