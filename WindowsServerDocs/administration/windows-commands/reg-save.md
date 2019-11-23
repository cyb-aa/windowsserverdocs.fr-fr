---
title: enregistrement reg
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6ae07cd3c90c51e7bd494bc6c35919680cde912a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371701"
---
# <a name="reg-save"></a>enregistrement reg



Enregistre une copie des sous-clés, entrées et valeurs de Registre spécifiées dans un fichier spécifié.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg save <KeyName> <FileName> [/y]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName >|Spécifie le chemin d’accès complet de la sous-clé. Pour spécifier des ordinateurs distants, incluez le nom de l’ordinateur (au format \\\\ComputerName\) dans le *nom*de l’ordinateur. Si vous omettez \\\\ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|Nom de fichier \<>|Spécifie le nom et le chemin d’accès du fichier créé. Si aucun chemin d’accès n’est spécifié, le chemin d’accès actuel est utilisé.|
|/y|Remplace un fichier existant par *le nom de fichier sans demander* confirmation.|
|/?|Affiche l’aide pour **reg save** à l’invite de commandes.|

## <a name="remarks-optional-section"></a>Remarques \<section facultative >

-   Le tableau suivant répertorie les valeurs renvoyées pour l’opération de **sauvegarde de Reg** .

|Valeur|Description|
|-----|-----------|
|0|Réussite|
|1|Échec|
-   Avant de modifier des entrées de Registre, enregistrez la sous-clé parente avec l’opération **reg save** . Si la modification échoue, restaurez la sous-clé d’origine avec l’opération **reg Restore** .

## <a name="BKMK_examples"></a>Illustre

Pour enregistrer la ruche MonApp dans le dossier actuel sous la forme d’un fichier nommé AppBkUp. HIV, tapez :
```
REG SAVE HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)