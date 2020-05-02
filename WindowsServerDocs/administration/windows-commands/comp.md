---
title: comp
description: Rubrique de référence pour la commande COMP, qui compare le contenu de deux fichiers ou jeux de fichiers octet par octet.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40319d23-704d-4da1-be93-8259547275d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2939ee2166d961cae8ae0699c130e91117dd8a6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711466"
---
# <a name="comp"></a>comp

Compare le contenu de deux fichiers ou ensembles de fichiers octet par octet. Ces fichiers peuvent être stockés sur le même lecteur ou sur des lecteurs différents, et dans le même répertoire ou dans des répertoires différents. Lorsque cette commande compare des fichiers, elle affiche leur emplacement et leurs noms de fichiers. S’il est utilisé sans paramètres, **COMP** vous invite à entrer les fichiers à comparer.

## <a name="syntax"></a>Syntaxe

```
comp [<data1>] [<data2>] [/d] [/a] [/l] [/n=<number>] [/c]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<data1>` | Spécifie l’emplacement et le nom du premier fichier ou ensemble de fichiers que vous souhaitez comparer. Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour spécifier plusieurs fichiers. |
| `<data2>` | Spécifie l’emplacement et le nom du deuxième fichier ou ensemble de fichiers que vous souhaitez comparer. Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour spécifier plusieurs fichiers. |
| /d | Affiche les différences au format décimal. (Le format par défaut est hexadécimal.) |
| /a | Affiche les différences sous forme de caractères. |
| /l | Affiche le numéro de la ligne où une différence se produit, au lieu d’afficher l’offset d’octet. |
| /n =`<number>` | Compare uniquement le nombre de lignes spécifiées pour chaque fichier, même si les fichiers ont des tailles différentes. |
| /C | Effectue une comparaison qui ne respecte pas la casse. |
| /OFF [ligne] | Traite les fichiers avec l’attribut hors connexion défini. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes 

- Au cours de la comparaison, **COMP** affiche des messages qui identifient les emplacements d’informations inégales entre les fichiers. Chaque message indique l’adresse mémoire de décalage des octets inégaux et le contenu des octets (en notation hexadécimale sauf si le paramètre de ligne de commande **/a** ou **/d** est spécifié). Les messages s’affichent au format suivant :

    ```
    Compare error at OFFSET xxxxxxxx
    file1 = xx
    file2 = xx
    ```

    Après dix comparaisons inégales, **COMP** cesse de comparer les fichiers et affiche le message suivant :

    `10 Mismatches - ending compare`

- Si vous omettez les composants nécessaires de *Data1* ou de *données2*, ou si vous omettez la totalité de *données2* , cette commande vous invite à entrer les informations manquantes.

- Si *Data1* contient uniquement une lettre de lecteur ou un nom de répertoire sans nom de fichier, cette commande compare tous les fichiers du répertoire spécifié au fichier spécifié dans *données1*.

- Si *données2* contient uniquement une lettre de lecteur ou un nom de répertoire, le nom de fichier par défaut pour *données2* devient le même nom que pour *données1*.

- Si la commande **COMP** ne peut pas trouver les fichiers spécifiés, elle vous invitera à fournir un message indiquant si vous souhaitez comparer des fichiers supplémentaires.

- Les fichiers que vous comparez peuvent avoir le même nom de fichier, à condition qu’ils se trouvent dans des répertoires différents ou sur des lecteurs différents. Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour spécifier des noms de fichiers.

- Vous devez spécifier **/n** pour comparer des fichiers de tailles différentes. Si les tailles de fichier sont différentes et que **/n** n’est pas spécifié, le message suivant s’affiche :

    ```
    Files are different sizes
    Compare more files (Y/N)?
    ```

    Pour comparer ces fichiers malgré tout, appuyez sur **N** pour arrêter la commande. Ensuite, exécutez à nouveau la commande **COMP** , en utilisant l’option **/n** pour comparer uniquement la première partie de chaque fichier.

- Si vous utilisez des caractères génériques (**&#42;** et **?**) pour spécifier plusieurs fichiers, **COMP** trouve le premier fichier qui correspond à *données1* et le compare au fichier correspondant dans *données2*, s’il existe. La commande **COMP** signale les résultats de la comparaison pour chaque fichier correspondant à *données1*. Lorsque vous avez terminé, **COMP** affiche le message suivant :

    `Compare more files (Y/N)?`

    Pour comparer d’autres fichiers, appuyez sur **o**. La commande **COMP** vous invite à entrer les emplacements et les noms des nouveaux fichiers. Pour arrêter les comparaisons, appuyez sur **N**. Lorsque vous appuyez sur **o**, vous êtes invité à indiquer les options de ligne de commande à utiliser. Si vous ne spécifiez pas d’options de ligne de commande, **COMP** utilise celles que vous avez spécifiées précédemment.

## <a name="examples"></a>Exemples

Pour comparer le contenu du répertoire *c:\Reports* avec le répertoire `\\sales\backup\april`de sauvegarde, tapez :

```
comp c:\reports \\sales\backup\april
```

Pour comparer les dix premières lignes des fichiers texte dans le répertoire *\invoice* et afficher le résultat au format décimal, tapez :

```
comp \invoice\*.txt \invoice\backup\*.txt /n=10 /d
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)