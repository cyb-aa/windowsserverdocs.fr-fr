---
title: fc
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 485fc3d8-b7c5-496d-87be-0011112f27d5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6c004fcebcf5eb743354d9e0a121ff8598217a4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377251"
---
# <a name="fc"></a>fc



Compare deux fichiers ou ensembles de fichiers et affiche les différences entre eux.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>Paramètres

|            Paramètre             |                                                                                                                                     Description                                                                                                                                      |
|----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                /a                |                                                 Abrégé la sortie d’une comparaison ASCII. Au lieu d’afficher toutes les lignes qui sont différentes, **FC** affiche uniquement la première et la dernière ligne pour chaque ensemble de différences.                                                  |
|                /b                |             Compare les deux fichiers en mode binaire, octet par octet, et ne tente pas de resynchroniser les fichiers après la détection d’une incompatibilité. Il s’agit du mode par défaut pour comparer les fichiers qui ont les extensions de fichier suivantes :. exe,. com,. sys,. obj,. lib ou. bin.              |
|                /c                |                                                                                                                               Ignore la casse.                                                                                                                               |
|                /l                |               Compare les fichiers en mode ASCII, ligne par ligne, et tente de resynchroniser les fichiers après la détection d’une incompatibilité. Il s’agit du mode par défaut pour comparer les fichiers, à l’exception des fichiers portant les extensions de fichier suivantes :. exe,. com,. sys,. obj,. lib ou. bin.                |
|             /lb\<N >              |                         Définit le nombre de lignes de la mémoire tampon interne sur *N*. La longueur par défaut de la mémoire tampon de ligne est de 100 lignes. Si les fichiers que vous comparez ont plus de 100 lignes consécutives différentes, **FC** annule la comparaison.                         |
|                /n                |                                                                                                                Affiche les numéros de ligne pendant une comparaison ASCII.                                                                                                                 |
|            /OFF [ligne]            |                                                                                                               N’ignore pas les fichiers dont l’attribut offline est défini.                                                                                                               |
|                commutateur                |                                                                    Empêche **FC** de convertir les tabulations en espaces. Le comportement par défaut consiste à traiter les tabulations comme des espaces, avec des arrêts à chaque huitième position de caractère.                                                                    |
|                /u.                |                                                                                                                        Compare les fichiers en tant que fichiers texte Unicode.                                                                                                                         |
|                /w                |         Compresse les espaces blancs (c’est-à-dire les tabulations et les espaces) au cours de la comparaison. Si une ligne contient un grand nombre d’espaces ou de tabulations consécutifs, **/w** traite ces caractères comme un seul espace. Lorsqu’il est utilisé avec **/w**, **FC** ignore les espaces blancs au début et à la fin d’une ligne.         |
|             /\<NNNN >             | Spécifie le nombre de lignes consécutives qui doivent correspondre à la suite d’une incompatibilité, avant que **FC** considère que les fichiers doivent être resynchronisés. Si le nombre de lignes correspondantes dans les fichiers est inférieur à *nnnn*, **FC** affiche les lignes correspondantes comme des différences. La valeur par défaut est 2. |
| [\<Lecteur1 >:] [<Path1>]<FileName1> |                                                                                        Spécifie l’emplacement et le nom du premier fichier ou ensemble de fichiers à comparer. *NomFichier1* est obligatoire.                                                                                        |
| [\<lecteur2 >:] [<Path2>]<FileName2> |                                                                                       Spécifie l’emplacement et le nom du deuxième fichier ou ensemble de fichiers à comparer. *NomFichier2* est requis.                                                                                        |
|                /?                |                                                                                                                         Affiche l'aide à l'invite de commandes.                                                                                                                         |

## <a name="remarks"></a>Notes

-   Cette commande est implemeted par c:\WINDOWS\fc.exe. Vous pouvez utiliser cette commande dans PowerShell, mais veillez à épeler l’exécutable complet (FC. exe), car « FC » est un alias pour format-Custom.

-   Rapports de différences entre les fichiers pour une comparaison ASCII

    Lorsque vous utilisez **FC** pour une comparaison ASCII, **FC** affiche les différences entre deux fichiers dans l’ordre suivant :  
    -   Nom du premier fichier
    -   Lignes de *NomFichier1* qui diffèrent entre les fichiers
    -   Première ligne à faire correspondre dans les deux fichiers
    -   Nom du deuxième fichier
    -   Lignes de *NomFichier2* qui diffèrent
    -   Première ligne à faire correspondre
-   Utilisation de **/b** pour les comparaisons binaires

    **/b** affiche des incompatibilités trouvées lors d’une comparaison binaire dans la syntaxe suivante :

    `\<XXXXXXXX: YY ZZ>`

    La valeur de *xxxxxxxx* spécifie l’adresse hexadécimale relative pour la paire d’octets, mesurée à partir du début du fichier. Les adresses commencent à 00000000. Les valeurs hexadécimales pour *YY* et *ZZ* représentent respectivement les octets incompatibles de *NomFichier1* et *NomFichier2*.
-   Utilisation de caractères génériques

    Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) dans *NomFichier1* et *NomFichier2*. Si vous utilisez un caractère générique dans *NomFichier1*, **FC** compare tous les fichiers spécifiés au fichier ou à l’ensemble de fichiers spécifié par *NomFichier2*. Si vous utilisez un caractère générique dans *NomFichier2*, **FC** utilise la valeur correspondante de *NomFichier1*.
-   Utilisation de la mémoire

    Lors de la comparaison de fichiers ASCII, **FC** utilise une mémoire tampon interne (suffisamment grande pour contenir 100 lignes) en tant que stockage. Si les fichiers sont plus volumineux que le tampon, **FC** compare ce qu’il peut charger dans la mémoire tampon. Si **FC** ne trouve pas de correspondance dans les portions chargées des fichiers, il s’arrête et affiche le message suivant :

    `Resynch failed. Files are too different.`

    Lors de la comparaison de fichiers binaires plus grands que la mémoire disponible, **FC** compare complètement les deux fichiers, en superposant les portions de la mémoire avec les portions suivantes du disque. La sortie est la même que pour les fichiers qui tiennent entièrement dans la mémoire.

## <a name="BKMK_examples"></a>Illustre

Pour effectuer une comparaison ASCII de deux fichiers texte, Monthly. rpt et Sales. rpt, et afficher les résultats dans un format abrégé, tapez :
```
fc /a monthly.rpt sales.rpt 
```
Pour effectuer une comparaison binaire de deux fichiers de commandes, profits. bat et Earnings. bat, tapez :
```
fc /b profits.bat earnings.bat
```
Les résultats ressemblent à ce qui suit :
```
00000002: 72 43
00000004: 65 3A
0000000E: 56 92
...
...
...
000005E8: 00 6E
FC: Earnings.bat longer than Profits.bat
```
Si les fichiers profits. bat et Earners. bat sont identiques, **FC** affiche le message suivant :
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
Pour comparer chaque fichier. bat dans le répertoire actif avec le fichier New. bat, tapez :
```
fc *.bat new.bat
```
Pour comparer le fichier New. bat sur le lecteur C avec le fichier New. bat sur le lecteur D, tapez :
```
fc c:new.bat d:*.bat
```
Pour comparer chaque fichier de commandes du répertoire racine du lecteur C au fichier portant le même nom dans le répertoire racine du lecteur D, tapez :
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
