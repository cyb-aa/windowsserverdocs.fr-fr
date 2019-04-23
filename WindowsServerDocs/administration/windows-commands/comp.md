---
title: comp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d10b86176d97e1afd76085516fbfc00bdc36577f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854680"
---
# <a name="comp"></a>comp



Compare le contenu de deux fichiers ou groupes de fichiers octet par octet. Si utilisée sans paramètres, **comp** vous invite à entrer les fichiers à comparer.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
comp [<Data1>] [<Data2>] [/d] [/a] [/l] [/n=<Number>] [/c]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Data1>|Spécifie l’emplacement et le nom du premier fichier ou ensemble de fichiers que vous souhaitez comparer. Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour spécifier plusieurs fichiers.|
|\<Data2>|Spécifie l’emplacement et le nom du second fichier ou ensemble de fichiers que vous souhaitez comparer. Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour spécifier plusieurs fichiers.|
|/d|Affiche les différences au format décimal. (Le format par défaut est hexadécimal).|
|/a|Affiche les différences sous forme de caractères.|
|/l|Affiche le numéro de la ligne où se trouve une différence, au lieu d’afficher l’offset d’octet.|
|/n=\<Number>|Compare uniquement le nombre de lignes qui sont spécifiées pour chaque fichier, même si les fichiers sont de tailles différentes.|
|/c|Effectue une comparaison qui ne respecte pas la casse.|
|/off[line]|Traite les fichiers avec l’attribut hors connexion.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Comment la **comp** commande identifie les différences d’information

    Lors de la comparaison, **comp** affiche des messages qui identifient les emplacements des informations qui diffèrent entre les fichiers. Chaque message indique l’adresse mémoire de décalage d’octets différents et le contenu des octets (en notation hexadécimale, sauf si le **/a** ou **/d** paramètre de ligne de commande est spécifié). Messages s’affichent dans le format suivant :

    `Compare error at OFFSET xxxxxxxx`

    `file1 = xx`

    `file2 = xx`

    Après dix inégaux, **comp** arrête la comparaison des fichiers et affiche le message suivant :

    `10 Mismatches - ending compare`
-   Gestion des cas spéciaux pour *Data1* et *Data2*  
    -   Si vous omettez les composants nécessaires de soit *Data1* ou *Data2* ou si vous omettez *Data2*, **comp** vous invitent à entrer les informations manquantes.
    -   Si *Data1* contient uniquement une lettre de lecteur ou un nom de répertoire sans nom de fichier, **comp** compare tous les fichiers dans le répertoire spécifié dans le fichier spécifié dans *Data1*.
    -   Si *Data2* contient uniquement une lettre de lecteur ou un nom de répertoire, le nom de fichier par défaut *Data2* est la même que celle dans *Data1*.
    -   Si **comp** ne peut pas trouver les fichiers que vous spécifiez, il vous invite avec un message pour déterminer si vous souhaitez comparer d’autres fichiers.
-   Comparer des fichiers dans différents emplacements

    **Comp** peut comparer des fichiers sur le même lecteur ou sur des lecteurs différents et dans le même répertoire ou dans des répertoires différents. Lorsque **comp** compare les fichiers, il affiche les emplacements et les noms de fichiers.
-   Comparaison des fichiers portant le même nom

    Les fichiers que vous comparez peuvent avoir le même nom, condition qu’ils soient dans des répertoires différents ou sur des lecteurs différents. Si vous ne spécifiez pas un nom de fichier pour *Data2*, le nom de fichier par défaut *Data2* est le même que le nom de fichier dans *Data1*. Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour spécifier les noms de fichiers.
-   Comparaison de fichiers de différentes tailles

    Vous devez spécifier **/n** pour comparer des fichiers de différentes tailles. Si la taille des fichiers est différente et **/n** n’est pas spécifié, **comp** affiche le message suivant :

    `Files are different sizes`

    `Compare more files (Y/N)?`

    Pour comparer ces fichiers, appuyez sur N pour arrêter la **comp** commande. Réexécutez le **comp** commande avec le **/n** option pour comparer uniquement la première partie de chaque fichier.
-   Comparaison des fichiers de manière séquentielle

    Si vous utilisez des caractères génériques (**&#42;** et **?**) pour spécifier plusieurs fichiers, **comp** recherche le premier fichier qui correspond à *Data1* et le compare avec le fichier correspondant dans *Data2*, s’il existe. Le **comp** commande signale les résultats de la comparaison pour chaque fichier correspondant *Data1*. Lorsque vous avez terminé, **comp** affiche le message suivant :

    `Compare more files (Y/N)?`

    Pour comparer plusieurs fichiers, appuyez sur Y. Le **comp** commande vous invite à entrer les emplacements et les noms des nouveaux fichiers. Pour arrêter les comparaisons, appuyez sur N. Lorsque vous appuyez sur O, **comp** vous invite à entrer des options de ligne de commande à utiliser. Si vous ne spécifiez pas d’options de ligne de commande, **comp** utilise ceux que vous avez spécifié avant.

## <a name="BKMK_examples"></a>Exemples

Comparer le contenu du répertoire C:\Reports avec le répertoire de sauvegarde \\ \\Sales\Backup\April, type :
```
comp c:\reports \\sales\backup\april
```
Pour comparer les dix premières lignes des fichiers texte dans le répertoire \Invoice et afficher le résultat en notation décimale, tapez :
```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)