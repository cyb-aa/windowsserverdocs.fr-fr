---
title: volume en ligne
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 06a3c81313180b2880c1e47c3b6c12236fda4245
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372517"
---
# <a name="online-volume"></a>volume en ligne



Remet les volumes actuellement hors connexion à un État en ligne

> [!IMPORTANT]
> Cette commande n’est disponible dans aucune édition de Windows Vista.

> [!IMPORTANT]
> Cette commande échoue si elle est utilisée sur un volume en lecture seule.

## <a name="syntax"></a>Syntaxe

```
online volume [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur les volumes qui ont échoué, qui échouent ou qui sont dans l’état Échec de la redondance.
-   Pour que cette commande aboutisse, vous devez sélectionner un volume. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.

## <a name="BKMK_examples"></a>Illustre

Pour mettre le volume avec le focus en ligne, tapez :
```
online volume
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

