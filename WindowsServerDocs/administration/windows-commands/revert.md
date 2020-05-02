---
title: rétablir
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b967904c28661be82909ebcc0aa7cb42c73e418c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722335"
---
# <a name="revert"></a>rétablir



Ramène un volume à un cliché instantané spécifié. Cela est pris en charge uniquement pour les clichés instantanés dans le contexte CLIENTACCESSIBLE. Ces clichés instantanés sont persistants et peuvent être effectués uniquement par le fournisseur système. S’il est utilisé sans paramètres, l’option **rétablir** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowCopyID>|Spécifie l’ID de cliché instantané pour rétablir le volume.|

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)