---
title: copie reg
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2acfdd3c0ad66d93313a11f8025b690ea0157c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836532"
---
# <a name="reg-copy"></a>copie reg



Copie une entrée de Registre à un emplacement spécifié sur l’ordinateur local ou distant.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName1 >|Spécifie le chemin d’accès complet de la sous-clé à copier. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur (au format \\\\ComputerName\) dans le *nom*de l’ordinateur. Si vous omettez \\\\ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|\<KeyName2 >|Spécifie le chemin d’accès complet de la destination de la sous-clé. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur (au format \\\\ComputerName\) dans le *nom*de l’ordinateur. Si vous omettez \\\\ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|/s|Copie toutes les sous-clés et les entrées sous la sous-clé spécifiée.|
|/f|Copie la sous-clé sans demander confirmation.|
|/?|Affiche l’aide pour **reg** Copy à l’invite de commandes.|

## <a name="remarks"></a>Notes

-   Reg ne demande pas de confirmation lors de la copie d’une sous-clé.
-   Le tableau suivant répertorie les valeurs renvoyées pour l’opération de **copie reg** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour copier toutes les sous-clés et les valeurs sous la clé MyApp vers la clé SaveMyApp, tapez :
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Pour copier toutes les valeurs sous la clé MyCo sur l’ordinateur nommé ZODIAC vers la clé MyCo1 sur l’ordinateur actuel, tapez :
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)