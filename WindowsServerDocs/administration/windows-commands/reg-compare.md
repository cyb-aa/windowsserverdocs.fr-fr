---
title: comparaison de reg
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9f4a8c51e7add429aab326804af698fed7007999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887220"
---
# <a name="reg-compare"></a>comparaison de reg



Compare spécifié des sous-clés de Registre ou des entrées.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<KeyName1>|Spécifie le chemin d’accès complet de la première sous-clé à comparer. Pour spécifier un ordinateur distant, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|\<KeyName2>|Spécifie le chemin d’accès complet de la deuxième sous-clé à comparer. Pour spécifier un ordinateur distant, incluez le nom d’ordinateur (au format \\ \\ComputerName\) dans le cadre de la *KeyName*. En omettant \\ \\ComputerName\, l’opération par défaut sur l’ordinateur local. En spécifiant uniquement le nom d’ordinateur dans *Nom_de_clé2* , l’opération à utiliser le chemin d’accès à la sous-clé spécifiée dans *Nomclé1*. Le *KeyName* doit inclure une clé racine valide. Les clés racine valide pour l’ordinateur local sont : HKLM, HKCU, HKCR, HKU et HKCC. Si un ordinateur distant est spécifié, les clés racines valides sont : HKLM et HKU.|
|/v \<ValueName>|Spécifie le nom de la valeur à comparer sous la sous-clé.|
|/ve|Spécifie que seules les écritures ayant un nom de valeur NULL doivent être comparées.|
|[{/oa | /od | /os | on}]|Spécifie comment afficher les résultats de l’opération de comparaison. La valeur par défaut est **/od**. Consultez les options ci-dessous.|
|/oa|Spécifie que toutes les différences et les correspondances sont affichées. Par défaut, seules les différences sont répertoriées.|
|/od|Spécifie que les seules différences sont affichées. Il s’agit du comportement par défaut.|
|/os|Indique que seuls les résultats sont affichés. Par défaut, seules les différences sont répertoriées.|
|/On|Spécifie que rien ne s’affiche. Par défaut, seules les différences sont répertoriées.|
|/s|Compare de manière récursive toutes les sous-clés et entrées.|
|/?|Affiche l’aide de **reg compare** à l’invite de commandes.|

## <a name="remarks"></a>Notes

Le tableau suivant répertorie les valeurs de retour pour **reg compare**.

|Value|Description|
|-----|-----------|
|0|La comparaison a réussi et le résultat est identique.|
|1|Échec de la comparaison.|
|2|Réussite de la comparaison et différences ont été trouvées.|

Le tableau suivant répertorie les symboles affichés dans les résultats.

|Symbole|Description|
|------|-----------|
|=|*Nomclé1* données sont égales à *Nom_de_clé2* données.|
|<|*Nomclé1* données sont inférieur à *Nom_de_clé2* données.|
|>|*Nomclé1* données sont supérieures à *Nom_de_clé2* données.|

## <a name="BKMK_examples"></a>Exemples

Pour comparer toutes les valeurs sous la clé **MyApp** avec toutes les valeurs sous la clé **SaveMyApp**, type :

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

Pour comparer la valeur de la Version sous la clé **MyCo** et la valeur de la Version sous la clé **MyCo1**, type :

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version

Pour comparer toutes les sous-clés et valeurs sous HKLM\Software\MaClé sur l’ordinateur nommé ZODIAQUE avec toutes les sous-clés et valeurs sous HKLM\Software\MaClé sur l’ordinateur local, tapez :

COMPARAISON de REG \\ \\ZODIAC\HKLM\Software\MyCo \\ \\. /s

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)