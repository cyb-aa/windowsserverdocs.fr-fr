---
title: cmd
description: Rubrique de référence pour la commande cmd, qui démarre une nouvelle instance de l’interpréteur de commandes, cmd. exe.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d381dd56d6648f749cd4a19d71422897e4b9b05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712591"
---
# <a name="cmd"></a>cmd

Démarre une nouvelle instance de l’interpréteur de commandes, cmd. exe. S’il est utilisé sans paramètres, **cmd** affiche la version et les informations de copyright du système d’exploitation.

## <a name="syntax"></a>Syntaxe

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<b><f> | <f>}] [/e:{on | off}] [/f:{on | off}] [/v:{on | off}] [<string>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /C | Exécute la commande spécifiée par *chaîne* , puis s’arrête. |
| /k | Exécute la commande spécifiée par *chaîne* et continue. |
| /s | Modifie le traitement de la *chaîne* après **/c** ou **/k**. |
| /q | Désactive l’écho. |
| /d | Désactive l’exécution des commandes d’exécution automatique. |
| /a | Met en forme la sortie de commande interne vers un canal ou un fichier en tant que American National Standards Institute (ANSI). |
| /U | Met en forme la sortie de commande interne vers un canal ou un fichier au format Unicode. |
| /t : {`<b><f>` | `<f>`} | Définit les couleurs d’arrière-plan (*b*) et de premier plan (*f*). |
| /e : activé | Active les extensions de commande. |
| /e : désactivé | Désactive les extensions de commandes. |
| /f : activé | Active la saisie semi-automatique des noms de fichiers et de répertoires. |
| /f : désactivé | Désactive la saisie semi-automatique des noms de fichiers et de répertoires. |
| /v : activé | Active l’expansion retardée des variables d’environnement. |
| /v : désactivé | Désactive l’expansion retardée des variables d’environnement. |
| `<string>` | Spécifie la commande que vous souhaitez exécuter. |
| /? | Affiche l'aide à l'invite de commandes. |

Le tableau suivant répertorie les chiffres hexadécimaux valides que vous pouvez utiliser comme `<b>` valeurs `<f>`pour et :

| Valeur | Couleur |
| ----- | ----- |
| 0 | Noir |
| 1 | Bleu |
| 2 | Vert |
| 3 | Aqua |
| 4 | Rouge |
| 5 | Violet |
| 6 | Jaune |
| 7 | White |
| 8 | Gris |
| 9 | Bleu clair |
| a | Vert clair |
| b | Cyan clair |
| c | Rouge clair |
| d | Violet clair |
| e | Jaune clair |
| f | Blanc brillant |

## <a name="remarks"></a>Notes 

- Pour utiliser plusieurs commandes pour `<string>`, séparez-les par le **&&** séparateur de commande et placez-les entre guillemets. Par exemple :

    ```
    "<command1>&&<command2>&&<command3>"
    ```

- Si vous spécifiez **/c** ou **/k**, **cmd** Processes, le reste de la *chaîne*, et les guillemets sont conservés uniquement si toutes les conditions suivantes sont remplies :

    - Vous n’utilisez pas également **/s**.

    - Vous utilisez exactement un jeu de guillemets.

    - Vous n’utilisez pas de caractères spéciaux entre guillemets (par exemple :  & < >  () @ ^ |).

    - Vous utilisez un ou plusieurs espaces blancs entre guillemets.

    - La *chaîne* entre guillemets est le nom d’un fichier exécutable.

    Si les conditions précédentes ne sont pas remplies, la *chaîne* est traitée en examinant le premier caractère afin de vérifier s’il s’agit d’un guillemet ouvrant. Si le premier caractère est un guillemet ouvrant, il est supprimé en même temps que le guillemet fermant. Tout texte qui suit les guillemets fermants est conservé.

- Si vous ne spécifiez pas **/d** dans une *chaîne*, cmd. exe recherche les sous-clés de Registre suivantes :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    Si l’une des sous-clés de registre ou les deux sont présentes, elles sont exécutées avant toutes les autres variables.

    > [!CAUTION]
    > Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

- Vous pouvez désactiver les extensions de commande pour un processus particulier à l’aide de l' **option/e : OFF**. Vous pouvez activer ou désactiver les extensions pour toutes les options de ligne de commande **cmd** sur un ordinateur ou une session utilisateur en définissant les valeurs de **REG_DWORD** suivantes :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    Affectez à **REG_DWORD** la valeur **0 × 1** (activé) ou **0 × 0** (désactivé) dans le registre à l’aide de Regedit. exe. Les paramètres spécifiés par l’utilisateur sont prioritaires par rapport aux paramètres de l’ordinateur, et les options de ligne de commande sont prioritaires sur les paramètres du Registre.

    > [!CAUTION]
    > Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

    Lorsque vous activez les extensions de commande, les commandes suivantes sont affectées :  
    - **assoc**

    - **call**

    - **chdir (CD)**

    - **color**

    - **del (effacer)**

    - **endlocal**

    - **for**

    - **ftype**

    - **goto**

    - **if**

    - **mkdir (MD)**

    - **popd**

    - **prompt**

    - **pushd**

    - **set**

    - **setlocal**

    - **shift**

    - **Démarrer** (comprend également les modifications apportées aux processus de commande externes)

- Si vous activez l’expansion retardée des variables d’environnement, vous pouvez utiliser le caractère point d’exclamation pour remplacer la valeur d’une variable d’environnement au moment de l’exécution.

- La saisie semi-automatique des noms de fichiers et de répertoires n’est pas activée par défaut. Vous pouvez activer ou désactiver la saisie semi-automatique des noms de fichiers pour un processus particulier de la commande **cmd** avec **/f :**{**on** | **off**}. Vous pouvez activer ou désactiver la saisie semi-automatique des noms de fichiers et de répertoires pour tous les processus de la commande **cmd** sur un ordinateur ou pour une ouverture de session utilisateur en définissant les valeurs de **REG_DWORD** suivantes :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    Pour définir la valeur **REG_DWORD** , exécutez regedit. exe et utilisez la valeur hexadécimale d’un caractère de contrôle pour une fonction particulière (par exemple, **0 × 9** est Tab et **0 × 08** est retour arrière). Les paramètres spécifiés par l’utilisateur sont prioritaires par rapport aux paramètres de l’ordinateur, et les options de ligne de commande sont prioritaires sur les paramètres du Registre.

    > [!CAUTION]
    > Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

- Si vous activez la saisie semi-automatique des noms de fichiers et de répertoires à l’aide de la touche **/f : on**, utilisez **Ctrl + D** pour terminer le nom de répertoire et **CTRL + f** pour terminer le nom de fichier. Pour désactiver un caractère d’achèvement particulier dans le registre, utilisez la valeur pour les espaces blancs [**0 × 20**], car il ne s’agit pas d’un caractère de contrôle valide.

  - Appuyez sur **Ctrl + D** ou **Ctrl + F**pour traiter la saisie semi-automatique des noms de fichiers et de répertoires. Ces fonctions de combinaison de touches ajoutent un caractère générique à *String* (si aucune n’est présente), génèrent une liste de chemins d’accès qui correspondent, puis affichent le premier chemin d’accès correspondant.<p>Si aucun des chemins d’accès ne correspond, la fonction de saisie semi-automatique des noms de fichiers et de répertoires émet un bip sonore et ne modifie pas l’affichage. Pour parcourir la liste des chemins d’accès correspondants, appuyez sur **Ctrl + D** ou **Ctrl + F** à plusieurs reprises. Pour parcourir la liste vers le haut, appuyez simultanément sur la touche **MAJ** et sur **Ctrl + D** ou **Ctrl + F** . Pour ignorer la liste enregistrée de chemins d’accès correspondants et générer une nouvelle liste, modifiez la *chaîne* , puis appuyez sur **Ctrl + D** ou **Ctrl + F**. Si vous basculez entre **Ctrl + D** et **Ctrl + F**, la liste enregistrée de chemins d’accès correspondants est ignorée et une nouvelle liste est générée. La seule différence entre les combinaisons de touches **Ctrl + d** et **CTRL + f** est que **Ctrl + d** correspond uniquement aux noms de répertoires et que **CTRL + f** correspond à la fois aux noms de fichiers et de répertoires. Si vous utilisez la saisie semi-automatique des noms de fichiers et de répertoires sur l’une des commandes de répertoire intégrées (c’est-à-dire **CD**, **MD**ou **rd**), la saisie semi-automatique des répertoires est utilisée.

  - La saisie semi-automatique des noms de fichiers et de répertoires traite correctement les noms de fichiers qui contiennent des espaces blancs ou des caractères spéciaux si vous placez des guillemets autour du chemin d’accès correspondant.

  - Vous devez utiliser des guillemets autour des caractères spéciaux suivants :  & < >  [] {} ^ =;! ' +, ' ~ [espace blanc].

  - Si les informations que vous fournissez contiennent des espaces, vous devez utiliser des guillemets autour du texte (par exemple, « nom de l’ordinateur »).

  - Si vous traitez la saisie semi-automatique des noms de fichiers et de répertoires à partir d’une *chaîne*, toute partie du *chemin d’accès* à droite du curseur est ignorée (au point de la *chaîne* où la saisie semi-automatique a été traitée).

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
