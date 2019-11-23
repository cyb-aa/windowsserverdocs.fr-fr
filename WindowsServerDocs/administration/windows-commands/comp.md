---
title: comp
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84604cea36b0b4c9543a7169002551c0da4f0493
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379257"
---
# <a name="comp"></a>comp



Compare le contenu de deux fichiers ou ensembles de fichiers octet par octet. S’il est utilisé sans paramètres, **COMP** vous invite à entrer les fichiers à comparer.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<données1 >|Spécifie l’emplacement et le nom du premier fichier ou ensemble de fichiers que vous souhaitez comparer. Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) pour spécifier plusieurs fichiers.|
|\<données2 >|Spécifie l’emplacement et le nom du deuxième fichier ou ensemble de fichiers que vous souhaitez comparer. Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) pour spécifier plusieurs fichiers.|
|/d|Affiche les différences au format décimal. (Le format par défaut est hexadécimal.)|
|/a|Affiche les différences sous forme de caractères.|
|/l|Affiche le numéro de la ligne où une différence se produit, au lieu d’afficher l’offset d’octet.|
|/n = nombre\<>|Compare uniquement le nombre de lignes spécifiées pour chaque fichier, même si les fichiers ont des tailles différentes.|
|/c|Effectue une comparaison qui ne respecte pas la casse.|
|/OFF [ligne]|Traite les fichiers avec l’attribut hors connexion défini.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Comment la commande **COMP** identifie les informations incompatibles

    Au cours de la comparaison, **COMP** affiche des messages qui identifient les emplacements d’informations inégales entre les fichiers. Chaque message indique l’adresse mémoire de décalage des octets inégaux et le contenu des octets (en notation hexadécimale sauf si le paramètre de ligne de commande **/a** ou **/d** est spécifié). Les messages s’affichent au format suivant :

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Après dix comparaisons inégales, **COMP** cesse de comparer les fichiers et affiche le message suivant :

    `10 Mismatches - ending compare`
-   Gestion des cas spéciaux pour *données1* et *data2*  
    -   Si vous omettez les composants nécessaires de *Data1* ou de *données2* ou si vous omettez *données2*, **COMP** vous invite à entrer les informations manquantes.
    -   Si *Data1* contient uniquement une lettre de lecteur ou un nom de répertoire sans nom de fichier, **COMP** compare tous les fichiers du répertoire spécifié au fichier spécifié dans *données1*.
    -   Si *données2* contient uniquement une lettre de lecteur ou un nom de répertoire, le nom de fichier par défaut pour *données2* est le même que dans *données1*.
    -   Si **COMP** ne trouve pas le ou les fichiers que vous spécifiez, il vous invite à entrer un message pour déterminer si vous souhaitez comparer d’autres fichiers.
-   Comparaison de fichiers dans différents emplacements

    **COMP** peut comparer des fichiers sur le même lecteur ou sur des lecteurs différents, et dans le même répertoire ou dans des répertoires différents. Quand **COMP** compare les fichiers, il affiche leurs emplacements et leurs noms de fichiers.
-   Comparaison des fichiers portant le même nom

    Les fichiers que vous comparez peuvent avoir le même nom de fichier, à condition qu’ils se trouvent dans des répertoires différents ou sur des lecteurs différents. Si vous ne spécifiez pas de nom de fichier pour *données2*, le nom de fichier par défaut pour *données2* est le même que le nom de fichier dans *données1*. Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) pour spécifier des noms de fichiers.
-   Comparaison de fichiers de différentes tailles

    Vous devez spécifier **/n** pour comparer des fichiers de tailles différentes. Si les tailles de fichier sont différentes et que **/n** n’est pas spécifié, **COMP** affiche le message suivant :

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Pour comparer ces fichiers, appuyez sur N pour arrêter la commande **COMP** . Ensuite, réexécutez la commande **COMP** avec l’option **/n** pour comparer uniquement la première partie de chaque fichier.
-   Comparaison de fichiers séquentiellement

    Si vous utilisez des caractères génériques **&#42;** (et **?** ) pour spécifier plusieurs fichiers, **COMP** trouve le premier fichier qui correspond à *données1* et le compare au fichier correspondant dans *données2*, s’il existe. La commande **COMP** signale les résultats de la comparaison pour chaque fichier correspondant à *données1*. Lorsque vous avez terminé, **COMP** affiche le message suivant :

    `Compare more files (Y/N)?`

    Pour comparer d’autres fichiers, appuyez sur o. La commande **COMP** vous invite à entrer les emplacements et les noms des nouveaux fichiers. Pour arrêter les comparaisons, appuyez sur N. Lorsque vous appuyez sur o, **COMP** vous invite à indiquer les options de ligne de commande à utiliser. Si vous ne spécifiez pas d’options de ligne de commande, **COMP** utilise celles que vous avez spécifiées précédemment.

## <a name="BKMK_examples"></a>Illustre

Pour comparer le contenu du répertoire C:\Reports avec le répertoire de sauvegarde \\\\Sales\Backup\April, tapez :
```
comp c:\reports \\sales\backup\april
```
Pour comparer les dix premières lignes des fichiers texte dans le répertoire \Invoice et afficher le résultat au format décimal, tapez :
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)