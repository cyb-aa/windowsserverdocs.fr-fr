---
title: forfiles
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 196da88dfd4ebe2be5a5c673e5afee3224432cb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844482"
---
# <a name="forfiles"></a>forfiles



Sélectionne et exécute une commande sur un fichier ou un ensemble de fichiers. Cette commande est utile pour le traitement par lots.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
forfiles [/p <Path>] [/m <SearchMask>] [/s] [/c <Command>] [/d [{+|-}][{<Date>|<Days>}]]
```


### <a name="parameters"></a>Paramètres

|                     Paramètre                      |                                                                                                                                                                                                                                                                                                    Description                                                                                                                                                                                                                                                                                                     |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /p \<chemin d’accès >                     |                                                                                                                                                                                                                                                 Spécifie le chemin d’accès à partir duquel commencer la recherche. Par défaut, la recherche commence dans le répertoire de travail actuel.                                                                                                                                                                                                                                                  |
|                  /m \<SearchMask >                  |                                                                                                                                                                                                                                                           Recherche les fichiers en fonction du masque de recherche spécifié. Le masque de recherche par défaut est **\*.\\** \*.                                                                                                                                                                                                                                                           |
|                         /s                         |                                                                                                                                                                                                                                                                   Indique à la commande **Forfiles** d’effectuer une recherche dans les sous-répertoires de manière récursive.                                                                                                                                                                                                                                                                    |
|                  Commande/c \<>                   |                                                                                                                                                                                                                                  Exécute la commande spécifiée sur chaque fichier. Les chaînes de commande doivent être mises entre guillemets. La commande par défaut est **cmd/c echo @file** .                                                                                                                                                                                                                                   |
| /d&nbsp;[{+\|-}]&#8288;[{\<Date > &#8288;\|\<jours >}] | Sélectionne les fichiers dont la date de dernière modification est comprise dans le laps de temps spécifié.</br>-Sélectionne les fichiers dont la date de dernière modification est ultérieure ou égale à ( **+** ) ou antérieure ou égale à ( **-** ) la date spécifiée, où la *Date* est au format mm/jj/aaaa.</br>-Sélectionne les fichiers dont la date de dernière modification est ultérieure ou égale à ( **+** ) la date actuelle plus le nombre de jours spécifié, ou antérieur ou égal à ( **-** ) la date actuelle moins le nombre de jours spécifié.</br>-Les valeurs valides pour les *jours* incluent un nombre compris entre 0 et 32768. Si aucun signe n’est spécifié, **+** est utilisé par défaut. |
|                         /?                         |                                                                                                                                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                        |

## <a name="remarks"></a>Notes

-   **Forfiles** est le plus souvent utilisé dans les fichiers de commandes.
-   **Forfiles/s** est similaire à **dir/s.**
-   Vous pouvez utiliser les variables suivantes dans la chaîne de commande, comme spécifié par l’option de ligne de commande **/c** .  

|Variable|Description|
|--------|-----------|
|@FILE|Nom du fichier.|
|@FNAME|Nom de fichier sans extension.|
|@EXT|Extension de nom de fichier.|
|@PATH|Chemin d’accès complet du fichier.|
|@RELPATH|Chemin d’accès relatif du fichier.|
|@ISDIR|Prend la valeur TRUE si un type de fichier est un répertoire. Dans le cas contraire, cette variable prend la valeur FALSe.|
|@FSIZE|Taille de fichier, en octets.|
|@FDATE|Date de la dernière modification apportée au fichier.|
|@FTIME|Date et heure de la dernière modification du fichier.|

-   Avec **Forfiles**, vous pouvez exécuter une commande sur ou passer des arguments à plusieurs fichiers. Par exemple, vous pouvez exécuter la commande **type** sur tous les fichiers d’une arborescence avec l’extension de nom de fichier. txt. Vous pouvez aussi exécuter chaque fichier de commandes (*. bat) sur le lecteur C, avec le nom de fichier MyInput. txt comme premier argument.
-   Avec **Forfiles**, vous pouvez effectuer l’une des opérations suivantes :  
    -   Sélectionnez les fichiers en fonction d’une date absolue ou d’une date relative à l’aide du paramètre **/d** .
    -   Créer une arborescence d’archive de fichiers à l’aide de variables telles que @FSIZE et @FDATE.
    -   Différenciez les fichiers des répertoires à l’aide de la variable @ISDIR.
    -   Incluez des caractères spéciaux dans la ligne de commande en utilisant le code hexadécimal du caractère, au format 0x*hh* (par exemple, 0x09 pour un onglet).
-   **Forfiles** fonctionne en implémentant l’indicateur de sous **-répertoires recurse** sur les outils conçus pour traiter un seul fichier.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour répertorier tous les fichiers de commandes sur le lecteur C, tapez :
```
forfiles /p c:\ /s /m *.bat /c cmd /c echo @file is a batch file
```
Pour répertorier tous les répertoires sur le lecteur C, tapez :
```
forfiles /p c:\ /s /m *.* /c cmd /c if @isdir==TRUE echo @file is a directory
```
Pour répertorier tous les fichiers du répertoire actif qui ont au moins un an, tapez :
```
forfiles /s /m *.* /d -365 /c cmd /c echo @file is at least one year old.
```
Pour afficher le *fichier* texte est obsolète pour chacun des fichiers du répertoire actif antérieurs au 1er janvier 2007, tapez :
```
forfiles /s /m *.* /d -01/01/2007 /c cmd /c echo @file is outdated. 
```
Pour répertorier les extensions de nom de fichier de tous les fichiers du répertoire actif au format de colonne, puis ajouter un onglet avant l’extension, tapez :
```
forfiles /s /m *.* /c cmd /c echo The extension of @file is 0x09@ext 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
