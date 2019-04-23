---
title: setx
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef37482f-f8a8-4765-951a-2518faac3f44
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a650fae246d71d8c1f9822dfa9ff8e96d855b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886570"
---
# <a name="setx"></a>setx



Crée ou modifie des variables d’environnement dans l’environnement de système, ou un utilisateur sans nécessiter de programmation ou de script. Le **Setx** commande également récupère les valeurs de clés de Registre et les écrit dans des fichiers texte.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] <Variable> <Value> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] [<Variable>] /k <Path> [/m]
setx [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <FileName> {[<Variable>] {/a <X>,<Y> | /r <X>,<Y> "<String>"} [/m] | /x} [/d <Delimiters>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur >|Spécifie le nom ou l’adresse IP d’un ordinateur distant. N’utilisez pas de barres obliques inverses. La valeur par défaut est le nom de l’ordinateur local.|
|/u [\<domaine >\]<User name>|Exécute le script avec les informations d’identification du compte d’utilisateur spécifié. La valeur par défaut est les autorisations du système.|
|/p [\<Password>]|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|\<Variable>|Spécifie le nom de la variable d’environnement que vous souhaitez définir.|
|\<valeur >|Spécifie la valeur à laquelle vous souhaitez définir la variable d’environnement.|
|/k \<Path>|Spécifie que la variable est définie en fonction des informations à partir d’une clé de Registre. Le p*hemin* utilise la syntaxe suivante :</br>`\\<HIVE>\<KEY>\...\<Value>`</br>Par exemple, vous pouvez spécifier le chemin d’accès suivant :</br>`HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName`|
|/f \<nom de fichier >|Spécifie le fichier que vous souhaitez utiliser.|
|/a \<X>,<Y>|Spécifie des coordonnées absolues et le décalage en tant que paramètres de recherche.|
|/r \<X >,<Y> «<String>»|Spécifie les coordonnées relatives et le décalage de **chaîne** en tant que paramètres de recherche.|
|/m|Spécifie pour définir la variable dans l’environnement système. Le paramètre par défaut est l’environnement local.|
|/x|Affiche fichier coordonnées, en ignorant le **/a**, **/r**, et **/d** des options de ligne de commande.|
|/d \<délimiteurs >|Spécifie les délimiteurs tels que »**,**» ou « **\**» pour être utilisée en plus les quatre délimiteurs intégrés : espace, tabulation, entrée et saut de ligne. Délimiteurs valides incluent tout caractère ASCII. Le nombre maximal de délimiteurs est 15, y compris les délimiteurs intégrés.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Le **Setx** commande est similaire à l’utilitaire UNIX SETENV.
-   **SETX** permet uniquement de ligne de commande ou par programmation à directement et définitivement les valeurs d’environnement système définis. Variables d’environnement système sont configurables manuellement via **le panneau de configuration** ou via un éditeur du Registre. Le **définir** commande, qui est interne à l’interpréteur de commandes (Cmd.exe), définit les variables d’environnement utilisateur de la fenêtre de console en cours uniquement.
-   Vous pouvez utiliser la **setx** commande pour définir des variables d’environnement pour l’utilisateur et système, les valeurs à partir d’une des trois sources (modes) : Mode à ligne de commande, Mode de Registre ou le Mode de fichier.
-   **SETX** écrit des variables de l’environnement principal dans le Registre. Variables définies avec **setx** variables ne sont disponibles dans les fenêtres de commandes uniquement, pas dans la fenêtre de commande en cours.
-   **HKEY_CURRENT_USER** et **HKEY_LOCAL_MACHINE** sont les seules ruches pris en charge. REG_DWORD, REG_EXPAND_SZ, REG_SZ et REG_MULTI_SZ sont valides **RegKey** types de données.
-   Lorsque vous accédez à **REG_MULTI_SZ** valeurs dans le Registre, seul le premier élément est extrait et utilisé.
-   Vous ne pouvez pas utiliser le **setx** commande pour supprimer les valeurs qui ont été ajoutées à l’environnement local ou système. Vous pouvez utiliser **définir** avec un nom de variable et aucune valeur pour supprimer une valeur correspondante à partir de l’environnement local.
-   Les valeurs de Registre REG_DWORD sont extraites et utilisées en mode hexadécimal.
-   Mode de fichier prend en charge l’analyse du retour chariot et saut de ligne uniquement les fichiers texte (CRLF).

## <a name="BKMK_examples"></a>Exemples

Pour définir la variable d’environnement MACHINE dans l’environnement local à la valeur Brand1, tapez :
```
setx MACHINE Brand1
```
Pour définir la variable d’environnement MACHINE dans l’environnement du système à la valeur Brand1 Computer, tapez :
```
setx MACHINE "Brand1 Computer" /m
```
Pour définir la variable d’environnement MYPATH dans l’environnement local à utiliser le chemin de recherche défini dans la variable d’environnement PATH, tapez :
```
setx MYPATH %PATH%
```
Pour définir la variable d’environnement MYPATH dans l’environnement local à utiliser le chemin de recherche défini dans la variable d’environnement PATH après avoir remplacé **~** avec **%**, type :
```
setx MYPATH ~PATH~ 
```
Pour définir la variable d’environnement MACHINE dans l’environnement local à Brand1 sur un ordinateur distant nommé Computer1, tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MACHINE Brand1
```
Pour définir la variable d’environnement MYPATH dans l’environnement local à utiliser le chemin de recherche défini dans la variable d’environnement PATH sur un ordinateur distant nommé Computer1, tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 MYPATH %PATH%
```
Pour définir la variable d’environnement TZONE dans l’environnement local à la valeur trouvée dans le **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** type de clé de Registre :
```
setx TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Pour définir la variable d’environnement TZONE dans l’environnement local d’un ordinateur distant nommé ordinateur1 à la valeur trouvée dans le **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName** Registre type de clé :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 TZONE /k HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation\StandardName 
```
Pour définir la variable d’environnement BUILD dans l’environnement du système à la valeur trouvée dans le **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber à** type de clé de Registre :
```
setx BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber" /m
```
Pour définir la variable d’environnement BUILD dans l’environnement de système d’un ordinateur distant nommé ordinateur1 à la valeur trouvée dans le **HKEY_LOCAL_MACHINE\Software\Microsoft\WindowsNT\CurrentVersion\CurrentBuildNumber à** clé de Registre , type :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23  BUILD /k "HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\CurrentBuildNumber" /m
```
Pour afficher le contenu d’un fichier nommé Ipconfig.out, ainsi que les coordonnées correspondantes du contenu, tapez :
```
setx /f ipconfig.out /x
```
Pour définir la variable d’environnement IPADDR dans l’environnement local à la valeur trouvée aux coordonnées 5,11 dans le fichier Ipconfig.out, tapez :
```
setx IPADDR /f ipconfig.out /a 5,11
```
Pour définir la variable d’environnement OCTET1 dans l’environnement local à la valeur trouvée aux coordonnées 5,3 dans le fichier Ipconfig.out avec les délimiteurs **« #$\*. »** , type :
```
setx OCTET1 /f ipconfig.out /a 5,3 /d "#$*." 
```
Pour définir la variable d’environnement de passerelle IP dans l’environnement local à la valeur trouvée aux coordonnées 0,7 par rapport à la coordonnée de « Passerelle » dans le fichier Ipconfig.out, tapez :
```
setx IPGATEWAY /f ipconfig.out /r 0,7 Gateway 
```
Pour afficher le contenu d’un fichier nommé Ipconfig.out, ainsi que les coordonnées correspondantes du contenu, sur un ordinateur nommé Computer1, tapez :
```
setx /s computer1 /u maindom\hiropln /p p@ssW23 /f ipconfig.out /x 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)