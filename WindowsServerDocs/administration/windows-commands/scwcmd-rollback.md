---
title: Scwcmd Rollback
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cd4eeec1113717a40dca43f0320f2db3c4c414e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722130"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> S’applique à : Windows Server 2012 R2, Windows Server 2012

Applique la stratégie de restauration la plus récente disponible, puis supprime cette stratégie de restauration.

## <a name="syntax"></a>Syntaxe

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/m :\<nom_ordinateur>|Spécifie le nom NetBIOS, le nom DNS ou l’adresse IP d’un ordinateur sur lequel l’opération de restauration doit être effectuée.|
|/u :\<nom d’utilisateur>|Spécifie un autre compte d’utilisateur à utiliser lors d’une restauration à distance. La valeur par défaut est l’utilisateur connecté.|
|/PW :\<mot de passe>|Spécifie les autres informations d’identification de l’utilisateur à utiliser lors d’une restauration à distance. La valeur par défaut est l’utilisateur connecté.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="examples"></a>Exemples

Pour restaurer la stratégie de sécurité sur un ordinateur à l’adresse IP 172.16.0.0, tapez :
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)