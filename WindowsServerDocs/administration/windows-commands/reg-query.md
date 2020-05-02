---
title: Reg Query
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea66a7d96435309a3b30b67f45bc68200f30dd2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722532"
---
# <a name="reg-query"></a>Reg Query



Retourne une liste du niveau suivant de sous-clés et d’entrées qui se trouvent sous une sous-clé spécifiée dans le registre.



## <a name="syntax"></a>Syntaxe

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName>|Spécifie le chemin d’accès complet de la sous-clé. Pour spécifier des ordinateurs distants, incluez le nom de \\ \\l'\) ordinateur (au format ComputerName dans le cadre du *keyName*. \\ \\Si vous omettez ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.|
|/v \<valueName>|Spécifie le nom de la valeur de Registre à interroger. En cas d’omission, tous les noms de valeurs pour *keyName* sont retournés. La *valueName* de ce paramètre est facultative si l’option **/f** est également utilisée.|
|/ve|Exécute une requête pour les noms de valeurs qui sont vides.|
|/s|Spécifie d’interroger toutes les sous-clés et les noms de valeurs de manière récursive.|
|> \<de séparateur/se|Spécifie le séparateur à valeur unique à rechercher dans le nom de la valeur REG_MULTI_SZ. Si *separator* n’est pas spécifié, **\ 0** est utilisé.|
|/f \<Data>|Spécifie les données ou le modèle à rechercher. Utilisez des guillemets doubles si une chaîne contient des espaces. S’il n’est pas spécifié, un caractère générique (**&#42;**) est utilisé comme modèle de recherche.|
|/k|Spécifie de rechercher uniquement dans les noms de clés.|
|/d|Spécifie la recherche dans les données uniquement.|
|/C|Spécifie que la requête respecte la casse. Par défaut, les requêtes ne respectent pas la casse.|
|/e|Spécifie de retourner uniquement les correspondances exactes. Par défaut, toutes les correspondances sont retournées.|
|/t \<> de type|Spécifie les types de Registre à rechercher. Les types valides sont : REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. S’il n’est pas spécifié, tous les types sont recherchés.|
|/z|Spécifie d’inclure l’équivalent numérique du type de Registre dans les résultats de la recherche.|
|/?|Affiche l’aide de la **requête reg** à l’invite de commandes.|

## <a name="remarks-optional-section"></a>Section \<remarques facultatives>

Le tableau suivant répertorie les valeurs renvoyées pour l’opération de **requête reg** .

|Valeur|Description|
|-----|-----------|
|0|Succès|
|1|Échec|

## <a name="examples"></a>Exemples

Pour afficher la valeur de la valeur de nom version dans la clé HKLM\Software\Microsoft\ResKit, tapez :
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Pour afficher toutes les sous-clés et les valeurs sous la clé HKLM\Software\Microsoft\ResKit\Nt\Setup sur un ordinateur distant nommé ABC, tapez :
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Pour afficher toutes les sous-clés et valeurs du type REG_MULTI_SZ à **#** l’aide de comme séparateur, tapez :
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

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)