---
title: reg unload
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5fd1436ed1122a09eea11d358a3711aedddf2c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836272"
---
# <a name="reg-unload"></a>reg unload



Supprime une section du Registre qui a été chargée à l’aide de l’opération de **chargement de Reg** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg unload <KeyName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName >|Spécifie le chemin d’accès complet de la sous-clé à décharger. Pour spécifier des ordinateurs distants, incluez le nom de l’ordinateur (au format \\\\ComputerName\) dans le *nom*de l’ordinateur. Si vous omettez \\\\ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont HKLM et HKU.|
|/?|Affiche l’aide de **reg unload** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs renvoyées pour l’option **reg unload** .

|Valeur|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour décharger le TempHive Hive dans le fichier HKLM, tapez :
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Ne modifiez pas le Registre directement, sauf si vous n’avez pas d’autre solution. L’éditeur du Registre contourne les protections standard, en autorisant les paramètres qui peuvent dégrader les performances, endommager votre système, voire vous obliger à réinstaller Windows. Vous pouvez modifier en toute sécurité la plupart des paramètres du Registre à l’aide des programmes du panneau de configuration ou de la console MMC (Microsoft Management Console). Si vous devez modifier le Registre directement, sauvegardez-le en premier.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)