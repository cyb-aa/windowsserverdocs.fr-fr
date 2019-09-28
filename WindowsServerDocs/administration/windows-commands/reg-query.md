---
title: Reg Query
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d2f616fb33974df4327c7b2536b3143b75d116be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371721"
---
# <a name="reg-query"></a>Reg Query



Retourne une liste du niveau suivant de sous-clés et d’entrées qui se trouvent sous une sous-clé spécifiée dans le registre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|@no__t 0KeyName >|Spécifie le chemin d’accès complet de la sous-clé. Pour spécifier des ordinateurs distants, incluez le nom de l’ordinateur (au format \\ @ no__t-1ComputerName @ no__t-2 dans le cadre du *keyName*. Si vous omettez \\ @ no__t-1ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|/v \<ValueName >|Spécifie le nom de la valeur de Registre à interroger. En cas d’omission, tous les noms de valeurs pour *keyName* sont retournés. La *valueName* de ce paramètre est facultative si l’option **/f** est également utilisée.|
|/ve|Exécute une requête pour les noms de valeurs qui sont vides.|
|/s|Spécifie d’interroger toutes les sous-clés et les noms de valeurs de manière récursive.|
|/se \<Separator >|Spécifie le séparateur à valeur unique à rechercher dans le nom de valeur type REG_MULTI_SZ. Si *separator* n’est pas spécifié, **\ 0** est utilisé.|
|/f \<Data >|Spécifie les données ou le modèle à rechercher. Utilisez des guillemets doubles si une chaîne contient des espaces. S’il n’est pas spécifié, **&#42;** un caractère générique () est utilisé comme modèle de recherche.|
|/k|Spécifie de rechercher uniquement dans les noms de clés.|
|/d|Spécifie la recherche dans les données uniquement.|
|/c|Spécifie que la requête respecte la casse. Par défaut, les requêtes ne respectent pas la casse.|
|/e|Spécifie de retourner uniquement les correspondances exactes. Par défaut, toutes les correspondances sont retournées.|
|/t \<Type >|Spécifie les types de Registre à rechercher. Les types valides sont : REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. S’il n’est pas spécifié, tous les types sont recherchés.|
|z|Spécifie d’inclure l’équivalent numérique du type de Registre dans les résultats de la recherche.|
|/?|Affiche l’aide de la **requête reg** à l’invite de commandes.|

## <a name="remarks-optional-section"></a>La section Notes \<optional >

Le tableau suivant répertorie les valeurs renvoyées pour l’opération de **requête reg** .

|Value|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="BKMK_examples"></a>Illustre

Pour afficher la valeur de la valeur de nom version dans la clé HKLM\Software\Microsoft\ResKit, tapez :
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Pour afficher toutes les sous-clés et les valeurs sous la clé HKLM\Software\Microsoft\ResKit\Nt\Setup sur un ordinateur distant nommé ABC, tapez :
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Pour afficher toutes les sous-clés et les valeurs du type REG_MULTI_SZ à l’aide de **#** comme séparateur, tapez :
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Pour afficher la clé, la valeur et les données pour les correspondances exactes et sensibles à la casse du système sous la racine HKLM du type de données REG_SZ, tapez :
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Pour afficher la clé, la valeur et les données qui correspondent à **0F** dans les données sous la clé racine HKCU du type de données REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Pour afficher la valeur et les données pour les noms de valeurs de valeur null (par défaut) sous HKLM\SOFTWARE, tapez :
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)