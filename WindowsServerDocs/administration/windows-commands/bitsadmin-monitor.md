---
title: bitsadmin monitor
description: Rubrique relative aux commandes Windows pour **Bitsadmin Monitor** -surveille les travaux dans la file d’attente de transfert que l’utilisateur actuel possède.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fe4963349c7e17fc77500b5adfceafc48a20ac5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381220"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Surveille les travaux de la file d’attente de transfert détenus par l’utilisateur actuel.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ALLUSERS|Facultatif : surveille les travaux de tous les utilisateurs.|
|Actualiser|Facultatif : actualise les données à un intervalle spécifié en *secondes*. L’intervalle d’actualisation par défaut est de cinq secondes.|

## <a name="remarks"></a>Notes

Vous devez disposer de privilèges d’administrateur pour utiliser le paramètre **ALLUSERS** .

Utilisez CTRL + C pour arrêter l’actualisation.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant surveille la file d’attente de transfert pour les travaux détenus par l’utilisateur actuel et actualise les informations toutes les 60 secondes.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)