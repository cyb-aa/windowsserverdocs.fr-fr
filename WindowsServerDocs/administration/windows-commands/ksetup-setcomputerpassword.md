---
title: ksetup:setcomputerpassword
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0679bb9ee429e05c7679411c5493bd21b530ef8e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831540"
---
# <a name="ksetupsetcomputerpassword"></a>ksetup:setcomputerpassword



Définit le mot de passe pour l’ordinateur local. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Mot de passe >|Utilise le mot de passe fourni pour définir le compte d’ordinateur sur l’ordinateur local.</br>Le mot de passe peut uniquement être définie à l’aide d’un compte avec des privilèges d’administrateur. Le mot de passe permettre comprendre entre 1 et des caractères alphanumériques ou spéciaux 156.|

## <a name="remarks"></a>Notes

Cette commande affecte uniquement le compte d’ordinateur.

Vous devez redémarrer l’ordinateur pour que la modification de mot de passe en vigueur.

Le mot de passe de compte d’ordinateur ne figure pas dans le Registre ou en tant que sortie à partir de la **ksetup** commande.

## <a name="BKMK_Examples"></a>Exemples

Modifier le mot de passe du compte ordinateur sur l’ordinateur local à partir de IPops897 à IPop$ 897 !.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)