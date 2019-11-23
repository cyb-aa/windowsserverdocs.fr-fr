---
title: 'Ksetup : setcomputerpassword'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d1d3742476385eb770c9cb5c798c1f6ab27c74f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374941"
---
# <a name="ksetupsetcomputerpassword"></a>Ksetup : setcomputerpassword



Définit le mot de passe de l’ordinateur local. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setcomputerpassword <Password>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Mot de passe \<>|Utilise le mot de passe fourni pour définir le compte d’ordinateur sur l’ordinateur local.</br>Le mot de passe ne peut être défini qu’à l’aide d’un compte disposant de privilèges d’administrateur. Le mot de passe peut être compris entre 1 et 156 caractères alphanumériques ou spéciaux.|

## <a name="remarks"></a>Notes

Cette commande affecte uniquement le compte d’ordinateur.

Vous devez redémarrer l’ordinateur pour que la modification du mot de passe prenne effet.

Le mot de passe du compte d’ordinateur n’est pas affiché dans le registre ou en tant que sortie de la commande **Ksetup** .

## <a name="BKMK_Examples"></a>Illustre

Remplacez le mot de passe du compte d’ordinateur sur l’ordinateur local IPops897 par IPop $897 !.
```
ksetup /setcomputerpassword IPop$897!
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)