---
title: volume en ligne
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 476dd893e7899a2bd58336546a7881934f415f92
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837842"
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour mettre le volume avec le focus en ligne, tapez :
```
online volume
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

