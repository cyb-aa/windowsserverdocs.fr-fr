---
title: copie de reg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11cbfce39433ea18632bec16c524da191def2a07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867560"
---
# <a name="reg-copy"></a>copie de reg



Copie une entrée de Registre dans un emplacement spécifié sur l’ordinateur local ou distant.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName1>|Spécifie le chemin d’accès complet de la sous-clé à copier. Pour spécifier un ordinateur distant, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|\<KeyName2>|Spécifie le chemin d’accès complet de la sous-clé destination. Pour spécifier un ordinateur distant, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|/s|Copie toutes les sous-clés et entrées sous la sous-clé spécifiée.|
|/f|Copie la sous-clé sans demander confirmation.|
|/?|Affiche l’aide de **reg** copie à l’invite de commandes.|

## <a name="remarks"></a>Notes

-   Reg ne demande pas de confirmation lors de la copie d’une sous-clé.
-   Le tableau suivant répertorie les valeurs de retour pour la **reg copie** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour copier toutes les sous-clés et valeurs sous la clé MyApp vers la clé EnregistrerMonApp, tapez :
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Pour copier toutes les valeurs sous la clé MaClé sur l’ordinateur nommé ZODIAQUE vers la clé Maclé1 sur l’ordinateur actuel, tapez :
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)