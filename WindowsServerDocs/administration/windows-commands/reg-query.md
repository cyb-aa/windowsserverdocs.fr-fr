---
title: reg query
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e239184cc5d118a858d012528fd8135f0b834e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827750"
---
# <a name="reg-query"></a>reg query



Retourne une liste de la couche suivante de sous-clés et entrées qui sont trouvent sous la sous-clé spécifiée dans le Registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé. Pour spécifier les ordinateurs distants, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|/v \<ValueName>|Spécifie le nom de valeur de Registre doit être interrogé. Si omis, toutes les valeurs de noms pour *KeyName* sont retournés. *ValueName* pour ce paramètre est facultatif si le **/f** option est également utilisée.|
|/ve|Exécute une requête pour les noms de valeurs qui sont vides.|
|/s|Spécifie pour interroger toutes les sous-clés et manière récursive les noms de valeur.|
|/se \<séparateur >|Spécifie le séparateur de valeur unique à rechercher dans le type de nom de valeur REG_MULTI_SZ. Si *séparateur* n’est pas spécifié, **\0** est utilisé.|
|/f \<Data>|Spécifie les données ou un modèle à rechercher. Utilisez des guillemets doubles si une chaîne contient des espaces. Si non spécifié, un caractère générique (**&#42;**) est utilisé comme modèle de recherche.|
|/k|Spécifie pour rechercher les noms de clés uniquement.|
|/d|Spécifie pour effectuer une recherche dans les données uniquement.|
|/c|Spécifie que la requête respecte la casse. Par défaut, les requêtes ne sont pas sensible à la casse.|
|/e|Spécifie pour retourner uniquement les correspondances exactes. Par défaut, toutes les correspondances sont retournés.|
|/t \<Type>|Spécifie les types de Registre à rechercher. Les types valides sont : REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Si non spécifié, tous les types sont recherchés.|
|/z|Spécifie d’inclure l’équivalent numérique du type de Registre dans les résultats de la recherche.|
|/?|Affiche l’aide de **reg query** à l’invite de commandes.|

## <a name="remarks-optional-section"></a>Remarques \<section facultative >

Le tableau suivant répertorie les valeurs de retour pour la **reg query** opération.

|Value|Description|
|-----|-----------|
|0|Opération réussie|
|1|Échec|

## <a name="BKMK_examples"></a>Exemples

Pour afficher la valeur de la valeur de nom Version dans la clé HKLM\Software\Microsoft\ResKit, tapez :
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Pour afficher toutes les sous-clés et valeurs sous la clé HKLM\Software\Microsoft\ResKit\Nt\Setup sur un ordinateur distant nommé ABC, tapez :
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Pour afficher toutes les sous-clés et valeurs du type REG_MULTI_SZ avec **#** comme séparateur, tapez :
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Pour afficher la clé, valeur et les données pour exactes et respecte la casse correspond à du système sous la racine HKLM du type de données REG_SZ, tapez :
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Pour afficher la clé, valeur et les données qui correspondent aux **0F** dans les données sous la clé racine HKCU des données de type REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Pour afficher la valeur et les données pour les noms de valeur null (valeur par défaut) sous HKLM\SOFTWARE, tapez :
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)