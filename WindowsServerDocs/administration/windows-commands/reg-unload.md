---
title: reg unload
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32df397b597291269dcfb1449d00e86b2f4f5836
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384618"
---
# <a name="reg-unload"></a>reg unload



Supprime une section du Registre qui a été chargée à l’aide de l’opération de **chargement de Reg** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg unload <KeyName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0KeyName >|Spécifie le chemin d’accès complet de la sous-clé à décharger. Pour spécifier des ordinateurs distants, incluez le nom de l’ordinateur (au format \\ @ no__t-1ComputerName @ no__t-2 dans le cadre du *keyName*. Si vous omettez \\ @ no__t-1ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont HKLM et HKU.|
|/?|Affiche l’aide de **reg unload** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’option **reg unload** .

|Value|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="BKMK_examples"></a>Illustre

Pour décharger le TempHive Hive dans le fichier HKLM, tapez :
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Ne modifiez pas le Registre directement, sauf si vous n’avez pas d’autre solution. L’éditeur du Registre contourne les protections standard, en autorisant les paramètres qui peuvent dégrader les performances, endommager votre système, voire vous obliger à réinstaller Windows. Vous pouvez modifier en toute sécurité la plupart des paramètres du Registre à l’aide des programmes du panneau de configuration ou de la console MMC (Microsoft Management Console). Si vous devez modifier le Registre directement, sauvegardez-le en premier.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)