---
title: copy
description: Rubrique de référence pour la commande de copie, qui copie un ou plusieurs fichiers d’un emplacement vers un autre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 909bcdf83c87440dafe2653711c4d7e215f8d66b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719299"
---
# <a name="copy"></a>copy

Copie un ou plusieurs fichiers d’un emplacement à un autre.

> [!NOTE]
> Vous pouvez également utiliser la commande de **copie** , avec des paramètres différents, à partir de la console de récupération. Pour plus d’informations sur la console de récupération, voir [environnement de récupération Windows (Windows RE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Syntaxe

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <source> [/a | /b] [+<source> [/a | /b] [+ ...]] [<destination> [/a | /b]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /d | Permet aux fichiers chiffrés en cours de copie d’être enregistrés en tant que fichiers déchiffrés à la destination. |
| /v | Vérifie que les nouveaux fichiers sont correctement écrits. |
| /n | Utilise un nom de fichier Short, s’il est disponible, lors de la copie d’un fichier avec un nom de plus de huit caractères, ou avec une extension de nom de fichier comportant plus de trois caractères. |
| /y | Supprime l’invite pour confirmer que vous souhaitez remplacer un fichier de destination existant. |
| /-y | Vous invite à confirmer que vous souhaitez remplacer un fichier de destination existant. |
| /z | Copie les fichiers en réseau en mode redémarrable. |
| /a | Indique un fichier texte ASCII. |
| /b | Indique un fichier binaire. |
| `<source>` | Obligatoire. Spécifie l’emplacement à partir duquel vous souhaitez copier un fichier ou un ensemble de fichiers. La *source* peut se composer d’une lettre de lecteur et du signe deux-points, d’un nom de répertoire, d’un nom de fichier ou d’une combinaison de ces deux. |
| `<destination>` | Obligatoire. Spécifie l’emplacement vers lequel vous souhaitez copier un fichier ou un ensemble de fichiers. La *destination* peut se composer d’une lettre de lecteur et du signe deux-points, d’un nom de répertoire, d’un nom de fichier ou d’une combinaison de ces deux. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- Vous pouvez copier un fichier texte ASCII qui utilise un caractère de fin de fichier (CTRL + Z) pour indiquer la fin du fichier.

- Si **/a** précède ou suit une liste de fichiers sur la ligne de commande, il s’applique à tous les fichiers répertoriés jusqu’à ce que la **copie** rencontre la valeur **/b**. Dans ce cas, **/b** s’applique au fichier qui précède le **/b**.

    L’effet de **/a** dépend de sa position dans la chaîne de ligne de commande :
      - Si **/a** suit la *source*, la commande de **copie** traite le fichier comme un fichier ASCII et copie les données qui précèdent le premier caractère de fin de fichier (Ctrl + Z).
      - Si **/a** suit la *destination*, la commande de **copie** ajoute un caractère de fin de fichier (Ctrl + Z) comme dernier caractère du fichier.

- Si **/b** indique à l’interpréteur de commande de lire le nombre d’octets spécifié par la taille du fichier dans le répertoire. **/b** est la valeur par défaut pour **Copy**, sauf si **Copy** combine des fichiers.

- Si **/b** précède ou suit une liste de fichiers sur la ligne de commande, il s’applique à tous les fichiers répertoriés jusqu’à ce que la **copie** rencontre **/a**. Dans ce cas, **/a** s’applique au fichier précédent **/a**.

    L’effet de la fonction **b** dépend de sa position dans la chaîne de ligne de commande :-si **/b** suit la *source*, la commande de **copie** copie l’intégralité du fichier, y compris tout caractère de fin de fichier (Ctrl + Z).
        -Si **/b** suit *destination*, la commande **copier** n’ajoute pas de caractère de fin de fichier (Ctrl + Z).

- Si une opération d’écriture ne peut pas être vérifiée, un message d’erreur s’affiche. Bien que les erreurs d’enregistrement se produisent rarement avec la commande de **copie** , vous pouvez utiliser **/v** pour vérifier que les données critiques ont été correctement enregistrées. L’option de ligne de commande **/v** ralentit également la commande de **copie** , car chaque secteur enregistré sur le disque doit être vérifié.

- Si **/y** est prédéfini dans la variable d’environnement **COPYCMD** , vous pouvez remplacer ce paramètre à l’aide de **/-y** sur la ligne de commande. Par défaut, vous êtes invité à remplacer ce paramètre, sauf si la commande de **copie** est exécutée dans un script de commandes.
  
- Pour ajouter des fichiers, spécifiez un seul fichier à *destination*, mais plusieurs fichiers pour la *source* (utilisez des caractères génériques ou le format *fichier1*+*fichier2*+*fichier3* ).

- Si la connexion est perdue au cours de la phase de copie (par exemple, si le serveur en cours de mise hors connexion interrompt la connexion), vous pouvez utiliser **copier/z** pour reprendre une fois la connexion rétablie. L’option **/z** affiche également le pourcentage d’exécution de l’opération de copie pour chaque fichier.

- Vous pouvez substituer un nom de périphérique à une ou plusieurs occurrences de *source* ou de *destination*.

- Si *destination* est un appareil (par exemple, COM1 ou LPT1), l’option **/b** copie les données sur l’appareil en mode binaire. En mode binaire, **Copy/b** copie tous les caractères (y compris les caractères spéciaux tels que CTRL + C, CTRL + S, Ctrl + Z et ENTER) sur l’appareil, sous forme de données. Toutefois, si vous omettez **/b**, les données sont copiées sur l’appareil en mode ASCII. En mode ASCII, les caractères spéciaux peuvent entraîner la combinaison de fichiers pendant le processus de copie.

- Si vous ne spécifiez pas de fichier de destination, une copie est créée avec le même nom, la même date de modification et l’heure de modification que le fichier d’origine. La nouvelle copie est stockée dans le répertoire actif sur le lecteur actif. Si le fichier source se trouve sur le lecteur actif et dans le répertoire actif et que vous ne spécifiez pas un lecteur ou un répertoire différent pour le fichier de destination, la commande de **copie** s’arrête et affiche le message d’erreur suivant :

    ```
    File cannot be copied onto itself
    0 File(s) copied
    ```

- Si vous spécifiez plusieurs fichiers dans la *source*, la commande de **copie** les combine tous en un seul fichier à l’aide du nom de fichier spécifié dans *destination*. La commande de **copie** part du principe que les fichiers combinés sont des fichiers ASCII, sauf si vous utilisez l’option **/b** .

- Pour copier des fichiers d’une longueur de 0 octet, ou pour copier tous les fichiers et sous-répertoires d’un répertoire, utilisez la [commande xcopy](xcopy.md).

- Pour affecter la date et l’heure actuelles à un fichier sans modifier le fichier, utilisez la syntaxe suivante :  

    ```
    copy /b <source> +,,
    ```

    Où les virgules indiquent que le paramètre de *destination* a été volontairement omis.

## <a name="examples"></a>Exemples

Pour copier un fichier appelé *MEMO. doc* vers *letter. doc* dans le lecteur actif et vous assurer qu’un caractère de fin de fichier (Ctrl + Z) se trouve à la fin du fichier copié, tapez :

```
copy memo.doc letter.doc /a
```

Pour copier un fichier nommé *Robin. Typ* à partir du lecteur et du répertoire actifs vers un répertoire existant nommé *oiseaux* situé sur le lecteur C, tapez :

```
copy robin.typ c:\birds
```

> [!NOTE]
> Si le répertoire *oiseaux* n’existe pas, le fichier *Robin. Typ* est copié dans un fichier nommé *oiseaux* situé dans le répertoire racine du disque du lecteur C.

Pour combiner *Mar89. rpt*, *Apr89. rpt*et *May89. rpt*, qui se trouvent dans le répertoire actif, et les placer dans un fichier nommé *Report* (également dans le répertoire actif), tapez :

```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```

> [!NOTE]
> Si vous combinez des fichiers, la commande de **copie** marque le fichier de destination avec la date et l’heure actuelles. Si vous omettez la *destination*, les fichiers sont combinés et stockés sous le nom du premier fichier de la liste.

Pour combiner tous les fichiers dans le *rapport*, lorsqu’un fichier nommé *rapport* existe déjà, tapez :

```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```

Pour combiner tous les fichiers du répertoire actif ayant l’extension de nom de fichier. txt dans un seul fichier nommé *Combined. doc*, tapez :

```
copy *.txt Combined.doc
```

Pour combiner plusieurs fichiers binaires en un seul fichier à l’aide de caractères génériques, ajoutez **/b**. Cela empêche Windows de traiter CTRL + Z comme un caractère de fin de fichier. Par exemple, entrez :

```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Si vous combinez des fichiers binaires, le fichier résultant peut être inutilisable en raison de la mise en forme interne.

- La combinaison de chaque fichier ayant une extension. txt avec son fichier. Ref correspondant crée un fichier portant le même nom de fichier, mais avec une extension. doc. La commande **Copy** combine *fichier1. txt* avec *fichier1. Ref* pour former *fichier1. doc*, puis la commande combine *fichier2. txt* avec *fichier2. Ref* pour former *fichier2. doc*, et ainsi de suite. Par exemple, entrez :

```
copy *.txt + *.ref *.doc
```

Pour combiner tous les fichiers avec l’extension. txt, puis pour combiner tous les fichiers avec l’extension. ref dans un fichier nommé *Combined. doc*, tapez :

```
copy *.txt + *.ref Combined.doc
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande xcopy](xcopy.md)
