---
title: openfiles
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83175d8529d9204c6b6d969a3db2aee2775bd0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852380"
---
# <a name="openfiles"></a>openfiles



Permet à un administrateur interroger, afficher ou déconnecter des fichiers et répertoires qui ont été ouverts sur un système. Également Active ou désactive l’indicateur global de tenir à jour la liste d’objets du système.

Cette rubrique inclut des informations sur les commandes suivantes :
-   [openfiles /disconnect](#BKMK_disconnect)
-   [openfiles /query](#BKMK_query)
-   [openfiles /local](#BKMK_local)

## <a name="BKMK_disconnect"></a>openfiles /disconnect

Permet à un administrateur de déconnecter les fichiers et dossiers qui ont été ouverts à distance via un dossier partagé.

### <a name="syntax"></a>Syntaxe

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<système >|Spécifie le système distant pour se connecter à (par nom ou adresse IP). N’utilisez pas de barres obliques inverses. Si vous n’utilisez pas le **/s** option, la commande est exécutée sur l’ordinateur local par défaut. Ce paramètre s’applique à tous les fichiers et dossiers qui sont spécifiés dans la commande.|
|/u [\<domaine >\]<UserName>|Exécute la commande en utilisant les autorisations du compte d’utilisateur spécifié. Si vous n’utilisez pas le **/u** option, le système d’autorisations sont utilisées par défaut.|
|/p [\<Password>]|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** option. Si vous n’utilisez pas le **/p** option, une invite de mot de passe s’affiche lorsque la commande est exécutée.|
|/id \<OpenFileID>|Déconnecte les fichiers ouverts par l’ID de fichier spécifié. Le caractère générique (**&#42;**) peut être utilisé avec ce paramètre.</br>Remarque: Vous pouvez utiliser la **openfiles /query** commande pour rechercher l’ID de fichier.|
|/a \<AccessedBy>|Déconnecte tous les fichiers ouverts associés avec le nom d’utilisateur spécifié dans le *AccessedBy* paramètre. Le caractère générique (**&#42;**) peut être utilisé avec ce paramètre.|
|/ o {lire \| écrire \| en lecture/écriture}|Déconnecte tous les fichiers ouverts avec la valeur de mode ouvert spécifié. Les valeurs valides sont en lecture, écriture ou lecture/écriture. Le caractère générique (**&#42;**) peut être utilisé avec ce paramètre.|
|/op \<OpenFile>|Déconnecte toutes les connexions de fichiers ouverts sont créées par un nom d’ouvrir un fichier spécifique. Le caractère générique (**&#42;**) peut être utilisé avec ce paramètre.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="examples"></a>Exemples

Pour déconnecter tous les fichiers ouverts avec le fichier ID 26843578, tapez :
```
openfiles /disconnect /id 26843578
```
Pour déconnecter tous les fichiers ouverts et les répertoires sont accédés par l’utilisateur « hiropln », tapez :
```
openfiles /disconnect /a hiropln
```
Pour déconnecter tous les fichiers ouverts et les répertoires avec le mode de lecture/écriture, tapez :
```
openfiles /disconnect /o read/write
```
Pour déconnecter le répertoire portant le nom de fichier ouvert « C:\TestShare\", quelle que soit qui y accède, type :
```
openfiles /disconnect /a * /op "c:\testshare\"
```
Pour déconnecter tous les fichiers ouverts sur l’ordinateur distant « srvmain » qui sont ouverts par l’utilisateur « hiropln », quel que soit leur ID, tapez :
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles /query

Interroge et affiche tous les fichiers ouverts.

### <a name="syntax"></a>Syntaxe

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<système >|Spécifie le système distant pour se connecter à (par nom ou adresse IP). N’utilisez pas de barres obliques inverses. Si vous n’utilisez pas le **/s** option, la commande est exécutée sur l’ordinateur local par défaut. Ce paramètre s’applique à tous les fichiers et dossiers qui sont spécifiés dans la commande.|
|/u [\<domaine >\]<UserName>|Exécute la commande en utilisant les autorisations du compte d’utilisateur spécifié. Si vous n’utilisez pas le **/u** option, le système d’autorisations sont utilisées par défaut.|
|/p [\<Password>]|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** option. Si vous n’utilisez pas le **/p** option, une invite de mot de passe s’affiche lorsque la commande est exécutée.|
|[/fo {TABLE \| liste \| CSV}]|Affiche la sortie au format spécifié. Les valeurs valides pour *Format* sont :</br>TABLE :  Affiche la sortie dans une table.</br>LISTE : Affiche la sortie dans une liste.</br>CSV : Affiche la sortie au format de valeurs séparées par des virgules.|
|/nh|Supprime l’en-tête de colonne dans la sortie. Valide uniquement lorsque le **/fo** paramètre est défini sur **TABLE** ou **CSV**.|
|/v|Spécifie que les informations détaillées affichées dans la sortie.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="examples"></a>Exemples

Pour interroger et afficher tous les fichiers ouverts, tapez :
```
openfiles /query
```
Pour interroger et afficher tous les fichiers ouverts dans un tableau sans en-tête, tapez :
```
openfiles /query /fo table /nh
```
Pour interroger et afficher tous les fichiers ouverts sous forme de liste avec des informations détaillées, tapez :
```
openfiles /query /fo list /v
```
Pour interroger et afficher tous les fichiers ouverts sur le système distant « srvmain » en utilisant les informations d’identification pour l’utilisateur « hiropln » sur le domaine « maindom », tapez :
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> Dans cet exemple, le mot de passe est fourni sur la ligne de commande. Pour empêcher le mot de passe s’affiche, omettez la **/p** option. Vous devez le mot de passe ne s’affichent pas à l’écran.

## <a name="BKMK_local"></a>OPENFILES /local

Active ou désactive l’indicateur global de tenir à jour la liste d’objets du système. Si utilisée sans paramètres, **openfiles /local** affiche l’état actuel de l’indicateur global de tenir à jour la liste d’objets.

### <a name="syntax"></a>Syntaxe

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[sur \| off]|Active ou désactive l’indicateur global de tenir à jour la liste d’objets, qui effectue le suivi des handles de fichiers local du système.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

-   L’activation de l’indicateur global de tenir à jour la liste d’objets peut ralentir votre système.
-   Modifications apportées à l’aide de la **sur** ou **hors** option ne prennent pas effet tant que vous redémarrez le système.

### <a name="examples"></a>Exemples

Pour vérifier l’état actuel de l’indicateur global de tenir à jour la liste d’objets, tapez :
```
openfiles /local
```
Par défaut, l’indicateur global de tenir à jour la liste d’objets est désactivée, et la sortie suivante s’affiche :
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
Pour activer l’indicateur global de tenir à jour la liste d’objets, tapez :
```
openfiles /local on
```
Le message suivant s’affiche quand l’indicateur global est activé :
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
Pour désactiver l’indicateur global de tenir à jour la liste d’objets, tapez :
```
openfiles /local off
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)