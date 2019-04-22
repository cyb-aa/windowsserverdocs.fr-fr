---
title: bitsadmin monitor
description: Rubrique de commandes de Windows pour **bitsadmin moniteur** -surveille les travaux dans la file d’attente de transfert détenus par l’utilisateur actuel.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c4620d5c8e46cb8bfcb6b9c83261d57781abea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814590"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Contrôle les travaux dans la file d’attente de transfert détenus par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ALLUSERS|Facultatif : surveille les travaux pour tous les utilisateurs.|
|Refresh|Facultatif : actualise les données selon un intervalle spécifié par *secondes*. L’intervalle d’actualisation par défaut est de cinq secondes.|

## <a name="remarks"></a>Notes

Vous devez disposer des privilèges d’administrateur pour utiliser le **Allusers** paramètre.

Utilisez CTRL + C pour arrêter l’actualisation.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant surveille la file d’attente de transfert des travaux appartenant à l’utilisateur actuel et actualise les informations de toutes les 60 secondes.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)