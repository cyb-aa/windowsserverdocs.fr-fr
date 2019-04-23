---
title: reg enregistrer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a46dfe081421ed727bd7ffeeab364e6c23dd801
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841080"
---
# <a name="reg-save"></a>reg enregistrer



Enregistre une copie des sous-clés spécifiés, les entrées et les valeurs du Registre dans un fichier spécifié.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé. Pour spécifier les ordinateurs distants, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|\<FileName>|Spécifie le nom et le chemin d’accès du fichier qui est créé. Si aucun chemin n’est spécifié, le chemin d’accès actuel est utilisé.|
|/y|Remplace un fichier existant portant le nom *FileName* sans demander confirmation.|
|/?|Affiche l’aide de **reg enregistrer** à l’invite de commandes.|

## <a name="remarks-optional-section"></a>Remarques \<section facultative >

-   Le tableau suivant répertorie les valeurs de retour pour la **reg enregistrer** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|
-   Avant de modifier les entrées de Registre, enregistrez la sous-clé parente avec la **reg enregistrer** opération. Si la modification échoue, restaurez la sous-clé d’origine avec le **reg restauration** opération.

## <a name="BKMK_examples"></a>Exemples

Pour enregistrer la ruche MyApp dans le dossier actuel dans un fichier nommé AppBkUp.hiv, tapez :
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)