---
title: copy
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9624d4a1-349a-4693-ad00-1d1d4e59e9ac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d593fbdbffd2a5ee4e4dfb4a817ad4708162160a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853770"
---
# <a name="copy"></a>copy



Copie un ou plusieurs fichiers d’un emplacement vers un autre.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
copy [/d] [/v] [/n] [/y | /-y] [/z] [/a | /b] <Source> [/a | /b] [+<Source> [/a | /b] [+ ...]] [<Destination> [/a | /b]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d|Permet les fichiers chiffrés copiés pour être enregistré en tant que fichiers déchiffrées à l’emplacement de destination.|
|/v|Vérifie que les nouveaux fichiers sont correctement écrites.|
|/n|Utilise un nom de fichier court, s’il est disponible, lors de la copie d’un fichier avec un nom plu de huit caractères, ou avec une extension de nom de fichier plue de trois caractères.|
|/y|Supprime l’invite pour confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/-y|Vous invite à confirmer que vous souhaitez remplacer un fichier de destination existant.|
|/z|Copie les fichiers en réseau en mode de redémarrage.|
|/a|Indique un fichier texte ASCII.|
|/b|Indique un fichier binaire.|
|\<Source>|Obligatoire. Spécifie l’emplacement à partir duquel vous souhaitez copier un fichier ou un ensemble de fichiers. *Source* peut se composer d’une lettre de lecteur et deux-points, un nom de répertoire, un nom de fichier ou une combinaison de ces éléments.|
|\<Destination>|Obligatoire. Spécifie l’emplacement auquel vous souhaitez copier un fichier ou un ensemble de fichiers. *Destination* peut se composer d’une lettre de lecteur et deux-points, un nom de répertoire, un nom de fichier ou une combinaison de ces éléments.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Vous pouvez copier un fichier texte ASCII qui utilise un caractère de fin de fichier (CTRL + Z) pour indiquer la fin du fichier.
-   À l’aide de **/a**

    Lorsque **/a** précède ou suit une liste de fichiers sur la ligne de commande, elle s’applique à tous les fichiers répertoriés jusqu'à ce que **copie** rencontre **/b**. Dans ce cas, **/b** s’applique au fichier précédent **/b**.

    L’effet de **/a** dépend de sa position dans la chaîne de ligne de commande. Lorsque **/a** suit *Source*, **copie** traite le fichier comme un fichier ASCII et copie les données qui précède le premier caractère de fin de fichier (CTRL + Z).

    Lorsque **/a** suit *Destination*, **copie** ajoute un caractère de fin de fichier (CTRL + Z) comme le dernier caractère du fichier.
-   À l’aide de **/b**

    **/b** dirige l’interpréteur de commandes pour lire le nombre d’octets spécifié par la taille du fichier dans le répertoire. **/b** est la valeur par défaut **copie**, sauf si **copie** combine des fichiers.

    Lorsque **/b** précède ou suit une liste de fichiers sur la ligne de commande, elle s’applique à tous les fichiers répertoriés jusqu'à ce que **copie** rencontre **/a**. Dans ce cas, **/a** s’applique au fichier précédent **/a**.

    L’effet de **/b** dépend de sa position dans la chaîne de ligne de commande. Lorsque **/b** suit *Source*, **copie** copie l’intégralité du fichier, y compris tout caractère de fin de fichier (CTRL + Z).

    Lorsque **/b** suit *Destination*, **copie** n’ajoute pas de caractère de fin de fichier (CTRL + Z).
-   À l’aide de **/v**

    Si une opération d’écriture ne peut pas être vérifiée, qu'un message d’erreur s’affiche. Bien que l’enregistrement des erreurs se produisent rarement avec **copie**, vous pouvez utiliser **/v** pour vérifier que les données critiques ont été correctement enregistrées. Le **/v** option de ligne de commande aussi ralentit le **copie** commande, car chaque secteur enregistré sur le disque doit être vérifiée.
-   À l’aide de **/y** et   **/y**

    Si **/y** est prédéfinie dans la variable d’environnement COPYCMD, vous pouvez remplacer ce paramètre à l’aide de **/-y** en ligne de commande. Par défaut, vous êtes invité lorsque vous remplacez ce paramètre, sauf si le **copie** commande est exécutée dans un script de commandes.
-   Ajout de fichiers

    Pour ajouter des fichiers, spécifiez un seul fichier pour *Destination*, mais plusieurs fichiers pour *Source* (utiliser des caractères génériques ou *File1* +  *Fichier2*+*fichier3* format).
-   À l’aide de **/z**

    Si la connexion est perdue pendant la phase de copie (par exemple, si le serveur de mise hors connexion interrompt la connexion), **copier /z** reprend après la connexion est rétablie. **/z** affiche également le pourcentage de l’opération de copie a été effectuée pour chaque fichier.
-   Copier vers et depuis des appareils

    Vous pouvez remplacer un nom de l’appareil pour une ou plusieurs occurrences de *Source* ou *Destination*.
-   L’utilisation ou l’omission **/b** lors de la copie vers un appareil

    Lorsque *Destination* est un appareil (par exemple, Com1 ou Lpt1) **/b** copie les données sur le périphérique en mode binaire. En mode binaire, **copier /b** copie tous les caractères (y compris les caractères spéciaux tels que CTRL + C, CTRL + S, CTRL + Z et entrée) à l’appareil en tant que données. Toutefois, si vous omettez **/b**, données sont copiées vers l’appareil en mode ASCII. En mode ASCII, les caractères spéciaux peuvent entraîner des fichiers à combiner pendant le processus de copie.
-   Utilisation du fichier de destination par défaut

    Si vous ne spécifiez pas un fichier de destination, une copie est créée avec le même nom, date de modification et modifié le temps que le fichier d’origine. La nouvelle copie est stockée dans le répertoire actif sur le lecteur actif. Si le fichier source est sur le lecteur actuel et dans le répertoire actif et que vous ne spécifiez pas un autre lecteur ou un répertoire du fichier de destination, le **copie** commande arrête et affiche le message d’erreur suivant :

    `File cannot be copied onto itself`

    `0 File(s) copied`
-   Combinaison de fichiers

    Si vous spécifiez plusieurs fichiers dans *Source*, **copie** les combine dans un fichier unique en utilisant le nom de fichier spécifié dans *Destination*. **Copie** suppose que les fichiers combinés sont des fichiers ASCII, sauf si vous utilisez le **/b** option.
-   Copie des fichiers de longueur nulle

    **Copie** ne copie pas les fichiers de longueur 0 octet. Utilisez **xcopy** pour copier ces fichiers.
-   Modification de l’heure et la date d’un fichier

    Si vous souhaitez assigner l’heure actuelle et la date dans un fichier sans modifier le fichier, utilisez la syntaxe suivante :  
    ```
    copy /b <Source> +,,
    ```  
    Les virgules indiquent l’omission de la *Destination* paramètre.
-   Copie des fichiers dans les sous-répertoires

    Pour copier tous les fichiers et les sous-répertoires d’un répertoire, utilisez la **xcopy** commande.
-   Le **copie** commande, avec des paramètres différents, est disponible à partir de la Console de récupération.

## <a name="BKMK_examples"></a>Exemples

Pour copier un fichier nommé Memo.doc dans lettre.doc dans le lecteur en cours et vous assurer qu’un caractère de fin de fichier (CTRL + Z) est à la fin du fichier copié, tapez :
```
copy memo.doc letter.doc /a
```
Pour copier un fichier nommé Merle.typ le lecteur actuel et le répertoire dans un répertoire existant nommé oiseaux qui se trouve sur le lecteur C, tapez :
```
copy robin.typ c:\birds
```
Si le répertoire Oiseaux n’existe pas, le fichier Merle.typ est copié dans un fichier nommé oiseaux qui se trouve dans le répertoire racine sur le disque dans le lecteur C.

Pour combiner Mar89.rpt, avr89.rpt et mai89.rpt, qui se trouvent dans le répertoire actif et les placer dans un fichier nommé rapport (également dans le répertoire actif), tapez :
```
copy mar89.rpt + apr89.rpt + may89.rpt Report
```
Lorsque vous combinez des fichiers, **copie** marque le fichier de destination avec la date et heure actuelles. Si vous omettez *Destination*, les fichiers sont combinées et stockés sous le nom du premier fichier dans la liste. Par exemple, pour combiner tous les fichiers de rapport lorsqu’un fichier nommé rapport existe déjà, tapez :
```
copy report + mar89.rpt + apr89.rpt + may89.rpt
```
Pour combiner tous les fichiers dans le répertoire actif ayant une extension de nom de fichier the.txt dans un seul fichier appelé Combined.doc, type :
```
copy *.txt Combined.doc 
```
Si vous souhaitez combiner plusieurs fichiers binaires en un seul fichier à l’aide de caractères génériques, incluez **/b**. Cela empêche Windows de traiter CTRL + Z comme un caractère de fin de fichier. Par exemple, entrez :
```
copy /b *.exe Combined.exe
```

> [!CAUTION]
> Si vous combinez des fichiers binaires, le fichier résultant peut être inutilisable en raison de la mise en forme interne.

Dans l’exemple suivant, **copie** associe chaque fichier ayant une extension .txt avec fichier .ref correspondant. Le résultat est un fichier portant le même nom de fichier, mais avec une extension .doc. **Copie** combine File1.txt avec Fichier1.ref pour former Fichier1.doc, puis **copie** combine File2.txt avec Fichier2.ref pour former Fichier2.doc et ainsi de suite. Par exemple, entrez :
```
copy *.txt + *.ref *.doc
```
Pour combiner tous les fichiers avec l’extension .txt et ensuite combiner tous les fichiers portant l’extension .ref dans un fichier nommé Combined.doc, tapez :
```
copy *.txt + *.ref Combined.doc
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)