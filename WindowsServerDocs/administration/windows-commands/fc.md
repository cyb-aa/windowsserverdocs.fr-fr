---
title: fc
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 98fd96c73b962a13c0e715420ebbe6f3cd19a42b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888680"
---
# <a name="fc"></a>fc



Compare deux fichiers ou groupes de fichiers et affiche les différences entre eux.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fc /a [/c] [/l] [/lb<N>] [/n] [/off[line]] [/t] [/u] [/w] [/<NNNN>] [<Drive1>:][<Path1>]<FileName1> [<Drive2>:][<Path2>]<FileName2>
fc /b [<Drive1:>][<Path1>]<FileName1> [<Drive2:>][<Path2>]<FileName2>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/a|Abrège le résultat d’une comparaison ASCII. Au lieu d’afficher toutes les lignes qui sont différents, **fc** n'affiche que la première et la dernière ligne pour chaque ensemble de différences.|
|/b|Compare les deux fichiers en mode binaire, octet par octet et ne tente pas de resynchroniser les fichiers après avoir trouvé une incompatibilité. C’est le mode par défaut pour comparer des fichiers qui ont des extensions de fichier suivantes : .exe, .com, .sys, .obj, .lib ou .bin.|
|/c|Ignore la casse.|
|/l|Compare les fichiers en mode ASCII, ligne par ligne et tente de resynchroniser les fichiers après avoir trouvé une incompatibilité. C’est le mode par défaut pour comparer des fichiers, sauf les fichiers portant les extensions de fichier suivantes : .exe, .com, .sys, .obj, .lib ou .bin.|
|/lb\<N>|Définit le nombre de lignes pour la mémoire tampon interne de ligne à *N*. La longueur de la mémoire tampon de ligne par défaut est 100 lignes. Si les fichiers que vous comparez ont plus de 100 lignes consécutives différentes, **fc** annule la comparaison.|
|/n|Affiche les numéros de ligne pendant une comparaison ASCII.|
|/off[line]|N’ignore pas les fichiers qui ont l’attribut hors connexion.|
|/t|Empêche **fc** de convertir les tabulations en espaces. Le comportement par défaut consiste à traiter les tabulations en tant qu’espaces, avec s’arrête à chaque huitième position de caractère.|
|/u|Compare les fichiers en tant que fichiers texte Unicode.|
|/w|Compresse les espaces blancs (autrement dit, les onglets et les espaces) lors de la comparaison. Si une ligne contient de nombreux espaces consécutifs ou des tabulations, **/w** traite ces caractères comme un espace unique. Lorsqu’il est utilisé avec **/w**, **fc** ignore l’espace blanc au début et à la fin d’une ligne.|
|/\<NNNN>|Spécifie le nombre de lignes consécutives qui doit correspondre à la suite une incompatibilité, avant **fc** considère que les fichiers doit être resynchronisée. Si le nombre de lignes correspondantes dans les fichiers est inférieur à *NNNN*, **fc** affiche ces lignes en tant que de différences. La valeur par défaut est 2.|
|[\<Drive1>:][<Path1>]<FileName1>|Spécifie l’emplacement et le nom du premier fichier ou ensemble de fichiers à comparer. *Nom_fichier1* est requis.|
|[\<Drive2>:][<Path2>]<FileName2>|Spécifie l’emplacement et le nom du second fichier ou ensemble de fichiers à comparer. *Nom_fichier2* est requis.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Cette commande est il est implémenté par c:\WINDOWS\fc.exe. Vous pouvez utiliser cette commande PowerShell, mais n’oubliez pas de détailler l’exécutable complète (fc.exe) dans la mesure où « fc » est un alias pour Format-Custom.

-   Différences entre les fichiers pour une comparaison ASCII de création de rapports

    Lorsque vous utilisez **fc** pour une comparaison ASCII, **fc** affiche les différences entre les deux fichiers dans l’ordre suivant :  
    -   Nom du premier fichier
    -   Lignes à partir de *Nom_fichier1* qui diffèrent entre les fichiers
    -   Première ligne à faire correspondre dans les deux fichiers
    -   Nom du deuxième fichier
    -   Lignes à partir de *Nom_fichier2* qui diffèrent
    -   Première ligne correspondante
-   À l’aide de **/b** pour les comparaisons binaires

    **/b** affiche les différences sont trouvées lors d’une comparaison binaire dans la syntaxe suivante :

    `\<XXXXXXXX: YY ZZ>`

    La valeur de *XXXXXXXX* Spécifie l’adresse hexadécimale relative de la paire d’octets, mesurées à partir du début du fichier. Adresses partent de 00000000. Hexadécimal des valeurs *YY* et *ZZ* représentent les octets qui ne correspondent pas à partir de *Nom_fichier1* et *Nom_fichier2*, respectivement.
-   Utilisation de caractères génériques

    Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) dans *Nom_fichier1* et *Nom_fichier2*. Si vous utilisez un caractère générique dans *Nom_fichier1*, **fc** compare tous les fichiers spécifiés dans le fichier ou un ensemble de fichiers spécifiés par *Nom_fichier2*. Si vous utilisez un caractère générique dans *Nom_fichier2*, **fc** utilise la valeur correspondante à partir de *Nom_fichier1*.
-   Utilisation de mémoire

    Lors de la comparaison des fichiers ASCII, **fc** utilise une mémoire tampon interne (assez grande pour contenir 100 lignes) comme espace de stockage. Si les fichiers sont supérieures à la mémoire tampon, **fc** compare ce qu’il peut charger dans la mémoire tampon. Si **fc** ne trouve pas de correspondance dans les portions chargées des fichiers, il s’arrête et affiche le message suivant :

    `Resynch failed. Files are too different.`

    Lors de la comparaison des fichiers binaires qui sont supérieures à la mémoire disponible, **fc** compare les deux fichiers complètement, en superposant les parties en mémoire les portions suivantes à partir du disque. La sortie est identique à celle des fichiers tenir entièrement dans la mémoire.

## <a name="BKMK_examples"></a>Exemples

Pour effectuer une comparaison ASCII de deux fichiers texte, mensuel.rpt et ventes.rpt et afficher les résultats au format abrégé, tapez :
```
fc /a monthly.rpt sales.rpt 
```
Pour effectuer une comparaison binaire de deux fichiers de commandes, Profits.bat et Benefice.bat, tapez :
```
fc /b profits.bat earnings.bat
```
Résultats semblables aux suivants apparaissent :
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
Si les fichiers Profits.bat et Benefice.bat sont identiques, **fc** affiche le message suivant :
```
Comparing files Profits.bat and Earnings.bat
FC: no differences encountered
```
Pour comparer chaque fichier .bat dans le répertoire actif au fichier Nouv.bat, tapez :
```
fc *.bat new.bat
```
Pour comparer le fichier Nouv.bat sur le lecteur C au fichier Nouv.bat du lecteur D, tapez :
```
fc c:new.bat d:*.bat
```
Pour comparer chaque fichier de commandes dans le répertoire racine sur le lecteur C dans le fichier portant le même nom dans le répertoire racine du lecteur D, tapez :
```
fc c:*.bat d:*.bat
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
