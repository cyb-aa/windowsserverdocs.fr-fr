---
title: volume en ligne
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bacba1e204f1eee2e3d4772ff9024aedbfc4fed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882000"
---
# <a name="online-volume"></a>volume en ligne



Apporte des volumes qui sont actuellement hors connexion à un état en ligne

> [!IMPORTANT]
> Cette commande n’est pas disponible dans n’importe quelle édition de Windows Vista.

> [!IMPORTANT]
> Cette commande échoue si elle est utilisée sur un volume en lecture seule.

## <a name="syntax"></a>Syntaxe

```
online volume [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur les volumes qui ont échoué, échouent ou sont en état d’échec de la redondance.
-   Un volume doit être sélectionné pour cette commande réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un volume et déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour mettre le volume en ligne, tapez :
```
online volume
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

