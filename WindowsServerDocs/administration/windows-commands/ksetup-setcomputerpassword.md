---
title: 'Ksetup : setcomputerpassword'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e65ea6e935d9fde9c23842755c36e418928dec7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841372"
---
# <a name="ksetupsetcomputerpassword"></a>Ksetup : setcomputerpassword



Définit le mot de passe de l’ordinateur local. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /setcomputerpassword <Password>
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Mot de passe \<>|Utilise le mot de passe fourni pour définir le compte d’ordinateur sur l’ordinateur local.</br>Le mot de passe ne peut être défini qu’à l’aide d’un compte disposant de privilèges d’administrateur. Le mot de passe peut être compris entre 1 et 156 caractères alphanumériques ou spéciaux.|

## <a name="remarks"></a>Notes

Cette commande affecte uniquement le compte d’ordinateur.

Vous devez redémarrer l’ordinateur pour que la modification du mot de passe prenne effet.

Le mot de passe du compte d’ordinateur n’est pas affiché dans le registre ou en tant que sortie de la commande **Ksetup** .

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Remplacez le mot de passe du compte d’ordinateur sur l’ordinateur local IPops897 par IPop $897 !.
```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)