---
title: endlocal
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e516b2bf9e8a45ada910dfbd93e3ed5e7d86c14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862140"
---
# <a name="endlocal"></a>endlocal



Met fin à la localisation des modifications d’environnement dans un fichier de commandes et restaure les variables d’environnement à leurs valeurs avant le correspondantes **setlocal** commande a été exécutée.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
endlocal
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **endlocal** commande n’a aucun effet en dehors d’un fichier de script ou lot.
-   Il est implicite **endlocal** commande à la fin d’un fichier de commandes.
-   Si les extensions de commande sont activées (extensions de commande sont activées par défaut), le **endlocal** commande restaure l’état des extensions de commande (autrement dit, activé ou désactivé) pour qu’il avait avant le correspondantes  **SETLOCAL** commande a été exécutée.

> [!NOTE]
> Pour plus d’informations sur l’activation et désactivation des extensions de commande, consultez [Cmd](cmd.md).

## <a name="BKMK_examples"></a>Exemples

Vous pouvez localiser les variables d’environnement dans un fichier de commandes. Par exemple, le programme suivant démarre le programme de commandes superapp sur le réseau, dirige la sortie vers un fichier et affiche le fichier dans le bloc-notes :
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)