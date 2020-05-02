---
title: 'Ksetup : setcomputerpassword'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9cb0c2ee36ed85ddfb015a80e86198fe788f8474
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724579"
---
# <a name="ksetupsetcomputerpassword"></a>Ksetup : setcomputerpassword



Définit le mot de passe de l’ordinateur local.

## <a name="syntax"></a>Syntaxe

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Mot de passe>|Utilise le mot de passe fourni pour définir le compte d’ordinateur sur l’ordinateur local.</br>Le mot de passe ne peut être défini qu’à l’aide d’un compte disposant de privilèges d’administrateur. Le mot de passe peut être compris entre 1 et 156 caractères alphanumériques ou spéciaux.|

## <a name="remarks"></a>Notes 

Cette commande affecte uniquement le compte d’ordinateur.

Vous devez redémarrer l’ordinateur pour que la modification du mot de passe prenne effet.

Le mot de passe du compte d’ordinateur n’est pas affiché dans le registre ou en tant que sortie de la commande **Ksetup** .

## <a name="examples"></a>Exemples

Remplacez le mot de passe du compte d’ordinateur sur l’ordinateur local IPops897 par IPop $897 !.
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)