---
title: Comparaison de Reg
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bfccc1f64b0113967a52e3ac0516d800cfea3532
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384718"
---
# <a name="reg-compare"></a>Comparaison de Reg



Compare les sous-clés ou les entrées du Registre spécifiées.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                                                                                                                                                                                                          Description                                                                                                                                                                                                                                                                                           |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   @no__t 0KeyName1 >   |                                                               Spécifie le chemin d’accès complet de la première sous-clé à comparer. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur (au format \\ @ no__t-1ComputerName @ no__t-2 dans le cadre du *keyName*. Si vous omettez \\ @ no__t-1ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU.                                                                |
|   @no__t 0KeyName2 >   | Spécifie le chemin d’accès complet de la deuxième sous-clé à comparer. Pour spécifier un ordinateur distant, incluez le nom de l’ordinateur (au format \\ @ no__t-1ComputerName @ no__t-2 dans le cadre du *keyName*. Si vous omettez \\ @ no__t-1ComputerName \, l’opération est effectuée par défaut sur l’ordinateur local. Si vous spécifiez uniquement le nom de l’ordinateur dans *KeyName2* , l’opération utilise le chemin d’accès à la sous-clé spécifiée dans *KeyName1*. Le *keyName* doit inclure une clé racine valide. Les clés racines valides pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont les suivantes : HKLM et HKU. |
| /v \<ValueName > |                                                                                                                                                                                                                                                                     Spécifie le nom de la valeur à comparer sous la sous-clé.                                                                                                                                                                                                                                                                      |
|       /ve       |                                                                                                                                                                                                                                                         Spécifie que seules les entrées qui ont un nom de valeur NULL doivent être comparées.                                                                                                                                                                                                                                                         |
|      [{/OA      |                                                                                                                                                                                                                                                                                              /OD                                                                                                                                                                                                                                                                                               |
|       /OA       |                                                                                                                                                                                                                                             Spécifie que toutes les différences et correspondances sont affichées. Par défaut, seules les différences sont répertoriées.                                                                                                                                                                                                                                             |
|       /OD       |                                                                                                                                                                                                                                                          Spécifie que seules les différences sont affichées. Il s’agit du comportement par défaut.                                                                                                                                                                                                                                                          |
|       /OS       |                                                                                                                                                                                                                                                    Spécifie que seules les correspondances sont affichées. Par défaut, seules les différences sont répertoriées.                                                                                                                                                                                                                                                     |
|       /On       |                                                                                                                                                                                                                                                       Spécifie que rien ne s’affiche. Par défaut, seules les différences sont répertoriées.                                                                                                                                                                                                                                                        |
|       /s        |                                                                                                                                                                                                                                                                         Compare toutes les sous-clés et les entrées de manière récursive.                                                                                                                                                                                                                                                                          |
|       /?        |                                                                                                                                                                                                                                                                    Affiche l’aide de la commande **reg compare** à l’invite de commandes.                                                                                                                                                                                                                                                                    |

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour **reg compare**.

|Value|Description|
|-----|-----------|
|0|La comparaison est réussie et le résultat est identique.|
|1|La comparaison a échoué.|
|2|La comparaison a réussi et des différences ont été trouvées.|

Le tableau suivant répertorie les symboles affichés dans les résultats.

|Symbole|Description|
|------|-----------|
|=|Les données *KeyName1* sont égales aux données *KeyName2* .|
|<|Les données *KeyName1* sont inférieures à *KeyName2* .|
|>|Les données *KeyName1* sont supérieures aux données *KeyName2* .|

## <a name="BKMK_examples"></a>Illustre

Pour comparer toutes les valeurs sous la clé **MyApp** avec toutes les valeurs sous la clé **SaveMyApp**, tapez :

REG COMPARe HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

Pour comparer la valeur de la version sous la clé **MyCo** et la valeur de la version sous la clé **MyCo1**, tapez :

REG COMPARe HKLM\Software\MyCo HKLM\Software\MyCo1/v version

Pour comparer toutes les sous-clés et les valeurs sous HKLM\Software\MyCo sur l’ordinateur nommé ZODIAC avec toutes les sous-clés et valeurs sous HKLM\Software\MyCo sur l’ordinateur local, tapez :

REG COMPARe \\ @ no__t-1ZODIAC\HKLM\Software\MyCo \\ @ no__t-3. /s

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)