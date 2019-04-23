---
title: Rétablir
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bc77b17317f602d642c7a9e025b67be10ad7256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875110"
---
# <a name="revert"></a>Rétablir



Rétablit un volume vers un cliché instantané spécifié. Cela est pris en charge uniquement pour les clichés instantanés dans le contexte CLIENTACCESSIBLE. Ces clichés sont persistantes et peuvent uniquement être effectuées par le fournisseur du système. Si utilisée sans paramètres, **rétablir** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ShadowCopyID>|Spécifie l’ID du cliché instantané pour restaurer le volume.|

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)