---
title: forfiles
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: d5ac95b795d1c5a59f8917bf851ab08fb4d7c1e7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439182"
---
# <a name="forfiles"></a>forfiles



Sélectionne et exécute une commande sur un fichier ou un ensemble de fichiers. Cette commande est utile pour le traitement par lots.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c "<Command>"] [/d [{+|-}][{<Date>|<Days>}]]
```


## <a name="parameters"></a>Paramètres

|                     Paramètre                      |                                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<Path>                     |                                                                                                                                                                                                                                                 Spécifie le chemin d’accès à partir duquel commencer la recherche. Par défaut, la recherche commence dans le répertoire de travail actuel.                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask>                  |                                                                                                                                                                                                                                                           Recherche des fichiers selon le masque de recherche spécifié. Le masque de recherche par défaut est **\*.\\** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   Indique le **forfiles** commande à rechercher dans les sous-répertoires de manière récursive.                                                                                                                                                                                                                                                                    |
|                  /c «\<commande > »                   |                                                                                                                                                                                                                                  Exécute la commande spécifiée sur chaque fichier. Chaînes de commande doivent être entourés de guillemets. La commande par défaut est **« cmd /c echo @file»** .                                                                                                                                                                                                                                   |
| /d&nbsp;[{+\|-}]&#8288;[{\<Date>\|&#8288;\<Days>}] | Sélectionne les fichiers avec une date de dernière modification dans l’intervalle de temps spécifié.</br>-Sélectionne les fichiers avec une date de dernière modification postérieure ou égale à ( **+** ) ou antérieure ou égale à ( **-** ) la date spécifiée, où *Date* est au format MM/jj/aaaa.</br>-Sélectionne les fichiers avec une date de dernière modification postérieure ou égale à ( **+** ) la date actuelle ainsi que le nombre de jours spécifié, ou antérieure ou égale à ( **-** ) la date actuelle moins le nombre de jours spécifié.</br>-Les valeurs valides pour *jours* inclure un nombre quelconque de la plage 0 – 32 768. Si aucune connexion n’est spécifiée, **+** est utilisé par défaut. |
|                         /?                         |                                                                                                                                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>Notes

-   **Forfiles** est couramment utilisé dans les fichiers de commandes.
-   **Forfiles /s** est similaire à **dir/s.**
-   Vous pouvez utiliser les variables suivantes dans la chaîne de commande tel que spécifié par le **/c** option de ligne de commande.  

|Variable|Description|
|--------|-----------|
|@FILE|Nom de fichier.|
|@FNAME|Nom de fichier sans extension.|
|@EXT|Extension de nom de fichier.|
|@PATH|Chemin d’accès complet du fichier.|
|@RELPATH|Chemin d’accès relatif du fichier.|
|@ISDIR|A la valeur TRUE si un type de fichier est un répertoire. Sinon, cette variable a la valeur FALSE.|
|@FSIZE|Taille de fichier, en octets.|
|@FDATE|Dernière date de modification sur le fichier.|
|@FTIME|Dernier horodateur modifié sur le fichier.|

-   Avec **forfiles**, vous pouvez exécuter une commande sur ou passer des arguments à plusieurs fichiers. Par exemple, vous pouvez exécuter la **type** commande sur tous les fichiers dans une arborescence avec l’extension de nom de fichier .txt. Ou vous pouvez exécuter chaque fichier de commandes (* .bat) sur le lecteur C, avec le fichier de nom « Monentrée.txt » comme premier argument.
-   Avec **forfiles**, vous pouvez effectuer une des opérations suivantes :  
    -   Sélectionnez les fichiers par une date absolue ou une date relative à l’aide de la **/d** paramètre.
    -   Générer une arborescence d’archive de fichiers à l’aide de variables telles que @FSIZE et @FDATE.
    -   Différencier des fichiers des répertoires à l’aide de la @ISDIR variable.
    -   Inclure des caractères spéciaux dans la ligne de commande en utilisant le code hexadécimal du caractère, dans 0 x*HH* format (par exemple, 0 x 09 pour un onglet).
-   **Forfiles** fonctionne en implémentant la **recurse sous-répertoires** indicateur sur les outils qui sont conçues pour traiter un seul fichier.

## <a name="BKMK_examples"></a>Exemples

Pour répertorier tous les fichiers de commandes sur le lecteur C, tapez :
```
forfiles /p c:\ /s /m *.bat /c "cmd /c echo @file is a batch file"
```
Pour répertorier tous les répertoires sur le lecteur C, tapez :
```
forfiles /p c:\ /s /m *.* /c "cmd /c if @isdir==TRUE echo @file is a directory"
```
Pour répertorier tous les fichiers dans le répertoire actif qui ont au moins un ans, tapez :
```
forfiles /s /m *.* /d -365 /c "cmd /c echo @file is at least one year old."
```
Pour afficher le texte «*fichier* est obsolète » pour chacun des fichiers dans le répertoire actif qui datent de plus du 1er janvier 2007, tapez :
```
forfiles /s /m *.* /d -01/01/2007 /c "cmd /c echo @file is outdated." 
```
Pour répertorier les extensions de nom de fichier de tous les fichiers dans le répertoire actif au format de colonne et ajouter un onglet avant l’extension, tapez :
```
forfiles /s /m *.* /c "cmd /c echo The extension of @file is 0x09@ext" 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
