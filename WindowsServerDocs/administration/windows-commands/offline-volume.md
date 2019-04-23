---
title: Volume hors connexion
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd14492a40046472f43f37d79c393c9467fe4a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873740"
---
# <a name="offline-volume"></a>Volume hors connexion



Met le volume en ligne avec le focus à l’état hors connexion.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans n’importe quelle édition de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
offline volume [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Un volume doit être sélectionné pour que cela réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un disque et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour mettre le disque avec le focus hors connexion, tapez :
```
offline volume
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

