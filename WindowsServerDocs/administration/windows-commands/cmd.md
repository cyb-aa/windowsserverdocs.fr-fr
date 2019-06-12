---
title: Cmd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 581e9a3bad8323c79839a4487b7da045e9cfec21
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811238"
---
# <a name="cmd"></a>Cmd

Démarre une nouvelle instance de l’interpréteur de commandes Cmd.exe. Si utilisée sans paramètres, **cmd** affiche les informations de copyright et la version du système d’exploitation.

## <a name="syntax"></a>Syntaxe

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/c|Exécute la commande spécifiée par *chaîne* puis s’arrête.|
|/k|Exécute la commande spécifiée par *chaîne* et continue.|
|/s|Modifie le traitement de *chaîne* après **/c** ou **/k**.|
|/q|Désactive l’écho.|
|/d|Désactive l’exécution des commandes d’exécution automatique.|
|/a|Formate la sortie de commande interne vers un canal ou un fichier en tant que ANSI American National Standards Institute ().|
|/u|Formate la sortie de commande interne vers un canal ou un fichier au format Unicode.|
|/ t: {\<B\>\<F\>\|\<F\>}|Définit l’arrière-plan (*B*) et de premier plan (*F*) couleurs.|
|/e:on|Active les extensions de commande.|
|/e:off|Désactive les extensions de commandes.|
|/f:on|Permet la saisie semi-automatique du nom de fichier et répertoire.|
|/f:off|Désactive la saisie semi-automatique du nom de fichier et répertoire.|
|/v:on|Active retardée expansion de variables d’environnement.|
|/v:off|Désactive retardée expansion de variables d’environnement.|
|\<chaîne >|Spécifie la commande que vous souhaitez exécuter.|
|/?|Affiche l'aide à l'invite de commandes.|

Le tableau suivant répertorie les chiffres hexadécimaux valides que vous pouvez utiliser comme valeurs pour \<B\> et \<F\>

|Value|Color|
|-----|-----|
|0|Noir|
|1|Bleu|
|2|Vert|
|3|Aqua|
|4|Rouge|
|5|Violet|
|6|Jaune|
|7|Blanc|
|8|Gris|
|9|Bleu clair|
|a|Vert clair|
|b|Bleu-vert clair|
|c|Rouge clair|
|d|Violet clair|
|e|Jaune clair|
|f|Blanc brillant|

## <a name="remarks"></a>Notes

-   À l’aide de plusieurs commandes

    Pour utiliser plusieurs commandes pour \<chaîne >, séparez-les par le séparateur de commandes **&&** et placez-les entre guillemets. Exemple :

    ```
    "<Command>&&<Command>&&<Command>"
    ``` 
 
-   Traitement des guillemets

    Si vous spécifiez **/c** ou **/k**, **cmd** traite le reste de *chaîne,* et les guillemets sont conservés uniquement si tous les des opérations suivantes conditions sont remplies :  
    -   Vous n’utilisez pas **/s**.
    -   Vous utilisez un seul jeu de guillemets.
    -   Vous n’utilisez pas de caractères spéciaux entre guillemets (par exemple : & () < > @ ^ |).
    -   Vous utilisez un ou plusieurs espaces entre guillemets.
    -   Le *chaîne* entre guillemets est le nom d’un fichier exécutable.

    Si les conditions précédentes ne sont pas remplies, *chaîne* est traitée en examinant le premier caractère pour vérifier s’il s’agit d’un guillemet ouvrant. Si le premier caractère est un guillemet ouvrant, il est supprimé, ainsi que le guillemet fermant. Texte situé après le guillemet anglais fermant est conservé.
-   Sous-clés de Registre en cours d’exécution

    Si vous ne spécifiez pas **/d** dans *chaîne*, Cmd.exe recherche les sous-clés de Registre suivantes :

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    Si un ou les deux sous-clés de Registre sont présents, ils sont exécutés avant toutes les autres variables.

> [!CAUTION]
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

-   Activation et désactivation des extensions de commande

    Les extensions de commande sont activées par défaut dans Windows XP. Vous pouvez les désactiver pour un processus particulier à l’aide de **/e : off**. Vous pouvez activer ou désactiver les extensions pour tous les **cmd** des options de ligne de commande sur une session d’ordinateur ou l’utilisateur en définissant les éléments suivants **REG_DWORD** valeurs :

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    Définir le **REG_DWORD** valeur soit **0 × 1** (activé) ou **0 × 0** (désactivé) dans le Registre à l’aide de Regedit.exe. Paramètres spécifiés par l’utilisateur sont prioritaires sur les paramètres de l’ordinateur, et les options de ligne de commande sont prioritaires sur les paramètres du Registre.

> [!CAUTION]
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

    When you enable command extensions, the following commands are affected:  
    -  **assoc**
    -  **call**
    -  **chdir (cd)**
    -  **color**
    -  **del (erase)**
    -  **endlocal**
    -  **for**
    -  **ftype**
    -  **goto**
    -  **if**
    -  **mkdir (md)**
    -  **popd**
    -  **prompt**
    -  **pushd**
    -  **set**
    -  **setlocal**
    -  **shift**
    -  **start** (also includes changes to external command processes)

-   L’activation d’expansion de variables d’environnement retardées

    Si vous activez l’expansion de variables d’environnement retardées, vous pouvez utiliser le caractère point d’exclamation de remplacer la valeur d’une variable d’environnement au moment de l’exécution.
-   L’activation de saisie semi-automatique du nom de fichier et répertoire

    Saisie semi-automatique du nom de fichier et répertoire n’est pas activée par défaut. Vous pouvez activer ou désactiver la saisie semi-automatique du nom de fichier pour un processus particulier de la **cmd** avec **/f:** {**sur**|**hors**}. Vous pouvez activer ou désactiver la saisie semi-automatique du nom fichier et répertoire pour tous les processus de le **cmd** commande sur un ordinateur ou pour une ouverture de session utilisateur en définissant les éléments suivants **REG_DWORD** valeurs :

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    Pour définir le **REG_DWORD** valeur, exécutez Regedit.exe et utilisez la valeur hexadécimale d’un caractère de contrôle pour une fonction particulière (par exemple, **0 × 9** est onglet et **0 × 08** est RETOUR ARRIÈRE). Paramètres spécifiés par l’utilisateur sont prioritaires sur les paramètres de l’ordinateur, et les options de ligne de commande sont prioritaires sur les paramètres du Registre.

> [!CAUTION]
> Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

Si vous activez la saisie semi-automatique du nom de fichier et répertoire à l’aide de **/f : sur**, utilisez CTRL + D pour compléter des noms de répertoire et CTRL + F pour la saisie semi-automatique de nom de fichier. Pour désactiver un caractère de terminaison particulier dans le Registre, utilisez la valeur pour l’espace blanc [**0 × 20**], car il n’est pas un caractère de contrôle valide.

Lorsque vous appuyez sur CTRL + D ou CTRL + F, **cmd** traite la saisie semi-automatique du nom de fichier et répertoire. Ces fonctions de combinaison de touches ajoutent un caractère générique pour *chaîne* (si un n’est pas présent), créez une liste de chemins d’accès qui correspondent et puis afficher le premier chemin. Si aucun des chemins correspondent, la fonction de saisie semi-automatique de nom de fichier et répertoire émet un signal sonore et ne modifie pas l’affichage. Pour vous déplacer dans la liste des chemins d’accès correspondants, appuyez sur CTRL + D ou CTRL + F à plusieurs reprises. Pour vous déplacer vers l’arrière dans la liste, appuyez simultanément sur la touche MAJ et CTRL + D ou CTRL + F. Pour annuler la liste enregistrée de chemins correspondants et générer une nouvelle liste, modifiez *chaîne* et appuyez sur CTRL + D ou CTRL + F. Si vous basculez entre CTRL + D et CTRL + F, la liste enregistrée de chemins correspondants est ignorée et une nouvelle liste est générée. La seule différence entre les combinaisons de touches CTRL + D et CTRL + F est que CTRL + D correspond uniquement aux noms de répertoires et CTRL + F correspond aux noms de fichiers et de répertoires. Si vous utilisez la saisie semi-automatique du nom de fichier et répertoire sur les commandes d’annuaire intégrés (autrement dit, **CD**, **MD**, ou **Bureau à distance**), terminaison de répertoire est supposé.

Saisie semi-automatique du nom de fichier et répertoire traite correctement les noms de fichiers qui contiennent des espaces blancs ou des caractères spéciaux si vous placez des guillemets autour du chemin d’accès correspondant.

Les caractères spéciaux suivants nécessitent des guillemets : & < > [] {} ^ = ; ! '+', ~ [espace blanc].

Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, « nom de l’ordinateur »).

Si vous traitez la terminaison des noms de répertoires et de fichiers à partir *chaîne*, tout dans le cadre de la *chemin d’accès* à droite du curseur est ignorée (au point dans *chaîne* où le Saisie semi-automatique a été traitée).

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
