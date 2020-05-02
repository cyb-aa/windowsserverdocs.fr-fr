---
title: volume en ligne
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb2ee396e4fa8a2e61001df0d979d85dabe1aa32
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723422"
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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes 

-   Cette commande fonctionne sur les volumes qui ont échoué, qui échouent ou qui sont dans l’état Échec de la redondance.
-   Pour que cette commande aboutisse, vous devez sélectionner un volume. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.

## <a name="examples"></a>Exemples

Pour mettre le volume avec le focus en ligne, tapez :
```
online volume
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

