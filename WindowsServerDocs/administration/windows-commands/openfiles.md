---
title: openfiles
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 38b1d27b86551c6d4cd9e6b1ad87bfc0e8dd221d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372500"
---
# <a name="openfiles"></a>openfiles



Permet à un administrateur d’interroger, d’afficher ou de déconnecter des fichiers et des répertoires qui ont été ouverts sur un système. Active ou désactive également l’indicateur global de la liste gérer les objets.

Cette rubrique contient des informations sur les commandes suivantes :
-   [OPENFILES/Disconnect](#BKMK_disconnect)
-   [openfiles/Query](#BKMK_query)
-   [openfiles/local](#BKMK_local)

## <a name="BKMK_disconnect"></a>OPENFILES/Disconnect

Permet à un administrateur de déconnecter des fichiers et des dossiers qui ont été ouverts à distance par le biais d’un dossier partagé.

### <a name="syntax"></a>Syntaxe

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Paramètres

|            Paramètre             |                                                                                                                                 Description                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<System >           | Spécifie le système distant auquel se connecter (par nom ou adresse IP). N’utilisez pas de barres obliques inverses. Si vous n’utilisez pas l’option **/s** , la commande est exécutée sur l’ordinateur local par défaut. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande. |
|    /u [\<Domain > \] @ no__t-2     |                                                          Exécute la commande en utilisant les autorisations du compte d’utilisateur spécifié. Si vous n’utilisez pas l’option **/u** , les autorisations système sont utilisées par défaut.                                                           |
|         /p [@no__t 0Password >]         |                                               Spécifie le mot de passe du compte d’utilisateur spécifié dans l’option **/u** . Si vous n’utilisez pas l’option **/p** , une invite de mot de passe s’affiche lorsque la commande est exécutée.                                                |
|        /ID \<OpenFileID >         |                                       Déconnecte les fichiers ouverts par l’ID de fichier spécifié. Le caractère générique ( **&#42;** ) peut être utilisé avec ce paramètre.</br>Remarque : Vous pouvez utiliser la commande **openfiles/Query** pour Rechercher l’ID du fichier.                                       |
|         /a \<AccessedBy >         |                                                Déconnecte tous les fichiers ouverts associés au nom d’utilisateur spécifié dans le paramètre *AccessedBy* . Le caractère générique ( **&#42;** ) peut être utilisé avec ce paramètre.                                                 |
| /o {lecture \| écriture \| lecture/écriture} |                                               Déconnecte tous les fichiers ouverts avec la valeur de mode d’ouverture spécifiée. Les valeurs valides sont lecture, écriture ou lecture/écriture. Le caractère générique ( **&#42;** ) peut être utilisé avec ce paramètre.                                                |
|         /op \<OpenFile >          |                                                           Déconnecte toutes les connexions de fichiers ouvertes créées par un nom de fichier ouvert spécifique. Le caractère générique ( **&#42;** ) peut être utilisé avec ce paramètre.                                                           |
|                /?                |                                                                                                                     Affiche l'aide à l'invite de commandes.                                                                                                                     |

### <a name="examples"></a>Exemples

Pour déconnecter tous les fichiers ouverts avec l’ID de fichier 26843578, tapez :
```
openfiles /disconnect /id 26843578
```
Pour déconnecter tous les fichiers et répertoires ouverts accessibles par l’utilisateur « hiropln », tapez :
```
openfiles /disconnect /a hiropln
```
Pour déconnecter tous les fichiers et répertoires ouverts en mode lecture/écriture, tapez :
```
openfiles /disconnect /o read/write
```
Pour déconnecter le répertoire portant le nom de fichier ouvert «C:\TestShare @ no__t-0, quelle que soit la personne qui y accède, tapez :
```
openfiles /disconnect /a * /op "c:\testshare\"
```
Pour déconnecter tous les fichiers ouverts sur l’ordinateur distant « Srvmain » qui sont accessibles par l’utilisateur « hiropln », quel que soit leur ID, tapez :
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles/Query

Interroge et affiche tous les fichiers ouverts.

### <a name="syntax"></a>Syntaxe

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Paramètres

|          Paramètre           |                                                                                                                                 Description                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<System >         | Spécifie le système distant auquel se connecter (par nom ou adresse IP). N’utilisez pas de barres obliques inverses. Si vous n’utilisez pas l’option **/s** , la commande est exécutée sur l’ordinateur local par défaut. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande. |
|  /u [\<Domain > \] @ no__t-2   |                                                          Exécute la commande en utilisant les autorisations du compte d’utilisateur spécifié. Si vous n’utilisez pas l’option **/u** , les autorisations système sont utilisées par défaut.                                                           |
|       /p [@no__t 0Password >]       |                                               Spécifie le mot de passe du compte d’utilisateur spécifié dans l’option **/u** . Si vous n’utilisez pas l’option **/p** , une invite de mot de passe s’affiche lorsque la commande est exécutée.                                                |
| [/FO {TABLE \| LIST \| CSV}] |                             Affiche la sortie dans le format spécifié. Les valeurs valides pour le *format* sont :</br>TABLEAU  Affiche la sortie dans une table.</br>TARIFS Affiche la sortie dans une liste.</br>VIRGULE Affiche la sortie au format de valeurs séparées par des virgules.                              |
|             /NH              |                                                                                Supprime l’en-tête de colonne dans la sortie. Valide uniquement lorsque le paramètre **/FO** est défini sur **table** ou **CSV**.                                                                                 |
|              /v              |                                                                                                       Spécifie que des informations détaillées doivent être affichées dans la sortie.                                                                                                        |
|              /?              |                                                                                                                     Affiche l'aide à l'invite de commandes.                                                                                                                     |

### <a name="examples"></a>Exemples

Pour interroger et afficher tous les fichiers ouverts, tapez :
```
openfiles /query
```
Pour interroger et afficher tous les fichiers ouverts dans un format de table sans en-tête, tapez :
```
openfiles /query /fo table /nh
```
Pour interroger et afficher tous les fichiers ouverts sous forme de liste avec des informations détaillées, tapez :
```
openfiles /query /fo list /v
```
Pour interroger et afficher tous les fichiers ouverts sur le système distant « Srvmain » en utilisant les informations d’identification de l’utilisateur « hiropln » sur le domaine « maindol », tapez :
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> Dans cet exemple, le mot de passe est fourni sur la ligne de commande. Pour empêcher l’affichage du mot de passe, laissez l’option **/p** . Vous serez invité à entrer le mot de passe, qui ne sera pas renvoyé à l’écran.

## <a name="BKMK_local"></a>openfiles/local

Active ou désactive l’indicateur global de la liste gérer les objets. En cas d’utilisation sans paramètre, **openfiles/local** affiche l’état actuel de l’indicateur global maintain Object List.

### <a name="syntax"></a>Syntaxe

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[sur \| OFF]|Active ou désactive l’indicateur global gérer les objets de la liste des objets, qui effectue le suivi des descripteurs de fichiers locaux.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="remarks"></a>Notes

-   L’activation de l’indicateur global gérer la liste d’objets peut ralentir votre système.
-   Les modifications apportées à l’aide de l’option **activé** ou **désactivé** ne prennent effet qu’après le redémarrage du système.

### <a name="examples"></a>Exemples

Pour vérifier l’état actuel de l’indicateur global gérer la liste d’objets, tapez :
```
openfiles /local
```
Par défaut, l’indicateur global gérer la liste des objets est désactivé et la sortie suivante s’affiche :
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
Pour activer l’indicateur global gérer la liste d’objets, tapez :
```
openfiles /local on
```
Le message suivant s’affiche lorsque l’indicateur global est activé :
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
Pour désactiver l’indicateur global gérer la liste d’objets, tapez :
```
openfiles /local off
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)