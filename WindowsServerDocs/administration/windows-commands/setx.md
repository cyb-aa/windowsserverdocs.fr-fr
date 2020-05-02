---
title: setx
description: Rubrique de référence pour setX, qui crée ou modifie des variables d’environnement dans l’environnement de l’utilisateur ou du système, sans nécessiter de programmation ou de script.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63cfb28770f635f97c8f3c7a701d9e959cee4a05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721851"
---
# <a name="setx"></a>setx

Crée ou modifie des variables d’environnement dans l’environnement utilisateur ou système, sans nécessiter de programmation ou de script. La commande **setx** récupère également les valeurs des clés de Registre et les écrit dans des fichiers texte.



## <a name="syntax"></a>Syntaxe

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> <String>} [/m] | /x} [/d <Delimiters>]
```

### <a name="parameters"></a>Paramètres

|         Paramètre          |                                                                                                                                              Description                                                                                                                                              |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<ordinateur>       |                                                                                  Spécifie le nom ou l’adresse IP d’un ordinateur distant. N’utilisez pas de barres obliques inverses. La valeur par défaut est le nom de l’ordinateur local.                                                                                  |
| /u [\<domaine>\]<User name> |                                                                                           Exécute le script avec les informations d’identification du compte d’utilisateur spécifié. La valeur par défaut est les autorisations système.                                                                                            |
|      /p [\<> de mot de passe]      |                                                                                                         Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                         |
|        \<> variable         |                                                                                                                 Spécifie le nom de la variable d’environnement que vous souhaitez définir.                                                                                                                  |
|          \<Valeur>          |                                                                                                                Spécifie la valeur pour laquelle vous souhaitez définir la variable d’environnement.                                                                                                                 |
|         > \<du chemin d’accès/k         | Spécifie que la variable est définie en fonction des informations d’une clé de registre. Le*Hemin* p utilise la syntaxe suivante :</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Par exemple, vous pouvez spécifier le chemin d’accès suivant :</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName` |
|      /f \<nom du fichier>       |                                                                                                                               Spécifie le fichier que vous souhaitez utiliser.                                                                                                                                |
|        /a \<X>,<Y>         |                                                                                                                    Spécifie les coordonnées absolues et le décalage en tant que paramètres de recherche.                                                                                                                    |
|   /r \<X>,<Y><String>   |                                                                                                            Spécifie les coordonnées relatives et le décalage par rapport à la **chaîne** en tant que paramètres de recherche.                                                                                                            |
|             /m             |                                                                                                Spécifie de définir la variable dans l’environnement système. Le paramètre par défaut est l’environnement local.                                                                                                 |
|             /x             |                                                                                                       Affiche les coordonnées des fichiers, en ignorant les options de ligne de commande **/a**, **/r**et **/d** .                                                                                                        |
|      /d \<délimiteurs>      |                    Spécifie des délimiteurs **,** tels que **\\** , ou à utiliser en plus des quatre délimiteurs intégrés (espace, tabulation, entrée et saut de ligne). Les délimiteurs valides incluent n’importe quel caractère ASCII. Le nombre maximal de délimiteurs est 15, y compris les délimiteurs intégrés.                    |
|             /?             |                                                                                                                                 Affiche l'aide à l'invite de commandes.                                                                                                                                  |

## <a name="remarks"></a>Notes 

-   La commande **setx** est semblable à l’utilitaire UNIX setenv.
-   **Setx** fournit la seule ligne de commande ou méthode par programmation pour définir des valeurs d’environnement système directement et de manière permanente. Les variables d’environnement système sont configurables manuellement par **le biais du panneau** de configuration ou d’un éditeur du Registre. La commande **Set** , qui est interne à l’interpréteur de commandes (cmd. exe), définit les variables d’environnement utilisateur uniquement pour la fenêtre de console active.
-   Vous pouvez utiliser la commande **setx** pour définir des valeurs pour les variables d’environnement utilisateur et système à partir de l’une des trois sources (modes) : mode ligne de commande, mode registre ou mode fichier.
-   **Setx** écrit des variables dans l’environnement maître dans le registre. Les variables définies avec des variables **setx** sont disponibles dans les fenêtres de commande ultérieures uniquement, et non dans la fenêtre de commande active.
-   **HKEY_CURRENT_USER** et **HKEY_LOCAL_MACHINE** sont les seules ruches prises en charge. REG_DWORD, REG_EXPAND_SZ, REG_SZ et REG_MULTI_SZ sont les types de données **RegKey** valides.
-   Lorsque vous accédez à **REG_MULTI_SZ** valeurs dans le registre, seul le premier élément est extrait et utilisé.
-   Vous ne pouvez pas utiliser la commande **setx** pour supprimer des valeurs qui ont été ajoutées à des environnements locaux ou système. Vous pouvez utiliser **Set** avec un nom de variable et aucune valeur pour supprimer une valeur correspondante de l’environnement local.
-   REG_DWORD valeurs de Registre sont extraites et utilisées en mode hexadécimal.
-   Le mode de fichier prend en charge l’analyse des fichiers texte de retour chariot et de saut de ligne (CRLF) uniquement.

## <a name="examples"></a>Exemples

Pour définir la variable d’environnement de l’environnement local sur la valeur BRAND1, tapez :
```
setx MACHINE Brand1
```
Pour définir la variable d’environnement d’ordinateur dans l’environnement système sur la valeur BRAND1 Computer, tapez :
```
setx MACHINE Brand1 Computer /m
```
Pour définir la variable d’environnement MYPATH dans l’environnement local afin d’utiliser le chemin de recherche défini dans la variable d’environnement PATH, tapez :
```
setx MYPATH %PATH%
```
Pour définir la variable d’environnement MYPATH dans l’environnement local afin d’utiliser le chemin de recherche défini dans la variable d' **~** environnement **%** Path après le remplacement de par, tapez :
```
setx MYPATH ~PATH~ 
```
Pour définir la variable d’environnement de l’environnement local sur Brand1 sur un ordinateur distant nommé Ordinateur1, tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Pour définir la variable d’environnement MYPATH dans l’environnement local afin d’utiliser le chemin de recherche défini dans la variable d’environnement PATH sur un ordinateur distant nommé Ordinateur1, tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Pour définir la variable d’environnement TZONE dans l’environnement local à la valeur trouvée dans la clé de Registre **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , tapez :
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Pour définir la variable d’environnement TZONE dans l’environnement local d’un ordinateur distant nommé Computer1 sur la valeur trouvée dans la clé de Registre **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\timezoneinformation\standardname** , tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Pour définir la variable d’environnement de génération dans l’environnement système sur la valeur trouvée dans la clé de Registre **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , tapez :
```
setx BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber /m
```
Pour définir la variable d’environnement de génération dans l’environnement système d’un ordinateur distant nommé Computer1 sur la valeur trouvée dans la clé de Registre **HKEY_LOCAL_MACHINE \software\microsoft\windowsnt\currentversion\currentbuildnumber** , tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber /m
```
Pour afficher le contenu d’un fichier nommé ipconfig. out, avec les coordonnées correspondantes, tapez :
```
setx /f ipconfig.out /x
```
Pour définir la variable d’environnement IPADDR dans l’environnement local à la valeur trouvée aux coordonnées 5, 11 dans le fichier ipconfig. out, tapez :
```
setx IPADDR /f ipconfig.out /a 5,11
```
Pour définir la variable d’environnement octet1 dans l’environnement local à la valeur trouvée aux coordonnées 5, 3 dans le fichier ipconfig. out avec des délimiteurs ** #$ \*.**, tapez :
```
setx OCTET1 /f ipconfig.out /a 5,3 /d #$*. 
```
Pour définir la variable d’environnement IPGATEWAY dans l’environnement local à la valeur trouvée à la coordonnée 0, 7 en ce qui concerne la coordonnée de la passerelle dans le fichier ipconfig. out, tapez :
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Pour afficher le contenu d’un fichier nommé ipconfig. out, ainsi que les coordonnées correspondantes, sur un ordinateur nommé Ordinateur1, tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)