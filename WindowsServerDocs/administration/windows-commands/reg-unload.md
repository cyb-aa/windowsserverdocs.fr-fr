---
title: reg unload
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aaa7d7a9fa82db2968d988e3b7b3fb8275a72337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834980"
---
# <a name="reg-unload"></a>reg unload



Supprime une section du Registre qui a été chargé à l’aide de la **reg charge** opération.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg unload <KeyName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé à être déchargé. Pour spécifier les ordinateurs distants, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont entrées HKLM et HKU.|
|/?|Affiche l’aide de **reg unload** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour la **reg unload** option.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour décharger la ruche RucheTemp dans le fichier HKLM, tapez :
```
REG UNLOAD HKLM\TempHive
```

> [!CAUTION]
> Ne modifiez pas directement le Registre, sauf si vous ne disposez d’aucune alternative. L’Éditeur du Registre ignore les protections standards, autorise des paramètres qui peuvent dégrader les performances, endommager votre système ou même vous obliger à réinstaller Windows. Vous pouvez modifier en toute sécurité de la plupart des paramètres de Registre à l’aide de programmes dans le panneau de configuration ou de la Console MMC (Microsoft Management). Si vous devez modifier le Registre directement, faire en premier.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)