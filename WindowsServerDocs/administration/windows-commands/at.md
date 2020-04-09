---
title: à
description: La rubrique commandes Windows pour **at**, qui planifie l’exécution de commandes et de programmes sur un ordinateur à une date et une heure spécifiées.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc71af64bdcb1b71afef1e88d42647c0e52612af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851282"
---
# <a name="at"></a>à

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Planifie l’exécution des commandes et des programmes sur un ordinateur à une date et une heure spécifiées. Vous pouvez utiliser uniquement lorsque le **service de planification** est en cours d’exécution. Utilisé sans paramètres, **dans** répertorie les commandes planifiées.

## <a name="syntax"></a>Syntaxe

```
at [\computername] [[id] [/delete] | /delete [/yes]]
at [\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `\<computerName\>` | Spécifie un ordinateur distant. Si vous omettez ce paramètre, **dans** planifie les commandes et les programmes sur l’ordinateur local. |
| `<id>` | Spécifie le numéro d’identification affecté à une commande planifiée. |
| `/delete` | Annule une commande planifiée. Si vous omettez l' *ID*, toutes les commandes planifiées sur l’ordinateur sont annulées. |
|  `/yes` | Répond oui à toutes les requêtes du système lorsque vous supprimez des événements planifiés. |
| `<time>` | Spécifie l’heure à laquelle vous souhaitez exécuter la commande. l’heure est exprimée en heures : minutes, en notation 24 heures (c’est-à-dire 00:00 [minuit] à 23:59). |
| `interactive` | Permet à la *commande* d’interagir avec le Bureau de l’utilisateur qui est connecté au moment de l’exécution de la *commande* . |
| `every:` | Exécute la *commande* à chaque jour spécifié ou tous les jours de la semaine ou du mois (par exemple, tous les jeudis ou le troisième jour de chaque mois). |
|` <date>` | Spécifie la date à laquelle vous souhaitez exécuter la commande. Vous pouvez spécifier un ou plusieurs jours de la semaine (autrement dit, tapez **M**,**T**,**W**,**th**,**F**,**S**,**su**) ou un ou plusieurs jours du mois (autrement dit, tapez 1 à 31). Séparez les entrées de date par des virgules. Si vous omettez la *Date*, **at** utilise le jour actuel du mois. |
| `next:` | Exécute la *commande* à l’occurrence suivante de la journée (par exemple, le jeudi suivant). |
| `<command>`      | Spécifie la commande Windows, le programme (c’est-à-dire le fichier. exe ou. com) ou le programme de traitement par lots (fichier. bat ou. cmd) que vous souhaitez exécuter. Lorsque la commande requiert un chemin d’accès en tant qu’argument, utilisez le chemin d’accès absolu (autrement dit, le chemin d’accès complet commençant par la lettre de lecteur). Si la commande se trouve sur un ordinateur distant, spécifiez la notation UNC (Universal Naming Convention) pour le serveur et le nom de partage, plutôt qu’une lettre de lecteur distant. |
| `/?` | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes

- **schtasks** est un autre outil de planification de ligne de commande que vous pouvez utiliser pour créer et gérer des tâches planifiées. Pour plus d’informations sur **schtasks**, consultez [schtasks](schtasks.md).

- Pour utiliser **dans**, vous devez être membre du groupe Administrateurs local.

- **at** ne charge pas automatiquement cmd. exe, l’interpréteur de commandes, avant d’exécuter les commandes. Si vous n’exécutez pas de fichier exécutable (. exe), vous devez charger cmd. exe de manière explicite au début de la commande comme suit :

    ```
    cmd /c dir > c:\test.out
    ```

- Quand vous utilisez l’option sans des options de **ligne de commande** , les tâches planifiées s’affichent dans un tableau au format suivant :

    ```
    Status  ID   Day        time        Command Line
    OK      1    Each F     4:30 PM     net send group leads status due
    OK      2    Each M     12:00 AM    chkstor > check.file
    OK      3    Each F     11:59 PM    backup2.bat
    ```

- Lorsque vous incluez un numéro d’identification (*ID*) avec **à** une invite de commandes, les informations relatives à une entrée unique s’affichent dans un format semblable à ce qui suit :  

    ```
    Task ID: 1
    Status: OK
    Schedule: Each  F
    Time of Day: 4:30 PM
    Command: net send group leads status due
  ```

    Une fois que vous avez planifié une commande avec **at**, en particulier une commande avec des options de ligne de commande, vérifiez que la syntaxe de la commande est correcte en tapant sans options de **ligne de commande** . Si les informations contenues dans la colonne de ligne de commande sont incorrectes, supprimez la commande et retapez-la. Si elle est toujours incorrecte, retapez la commande avec moins d’options de ligne de commande.

- Commandes planifiées avec **au moment** de l’exécution en tant que processus en arrière-plan. La sortie n’est pas affichée sur l’écran de l’ordinateur. Pour rediriger la sortie vers un fichier, utilisez le symbole de redirection `(>)`. Si vous redirigez la sortie vers un fichier, vous devez utiliser le symbole d’échappement `(^)` avant le symbole de redirection, que vous **utilisiez au niveau** de la ligne de commande ou dans un fichier de commandes. Par exemple, pour rediriger la sortie vers output. Text, tapez :

    `at 14:45 c:\test.bat ^>c:\output.txt`

    Le répertoire actif pour la commande en cours d’exécution est le dossier systemroot.

- Si vous modifiez l’heure système sur un ordinateur après avoir planifié une commande à exécuter avec **à**, synchronisez le **dans** le planificateur avec l’heure système révisée **en tapant sans** options de ligne de commande.

- Les commandes planifiées sont stockées dans le registre. Par conséquent, vous ne perdez pas les tâches planifiées si vous redémarrez le service de planification.

- N’utilisez pas de lecteur Redirigé pour les tâches planifiées qui accèdent au réseau. Le service de planification peut ne pas être en mesure d’accéder au lecteur Redirigé, ou le lecteur Redirigé peut ne pas être présent si un autre utilisateur est connecté au moment de l’exécution de la tâche planifiée. Utilisez plutôt des chemins d’accès UNC pour les tâches planifiées. Par exemple :  

    `at 1:00pm my_backup \\\server\share`  

    N’utilisez pas la syntaxe suivante, où **x :** est une connexion établie par l’utilisateur :  

    `at 1:00pm my_backup x:`  

    Si vous planifiez une commande **at** qui utilise une lettre de lecteur pour se connecter à un répertoire partagé, ajoutez une commande **at** pour déconnecter le lecteur lorsque vous avez terminé d’utiliser le lecteur. Si le lecteur n’est pas déconnecté, la lettre de lecteur affectée n’est pas disponible à l’invite de commandes.

- Par défaut, les tâches planifiées à l’aide de la commande **at** arrêtent après 72 heures. Vous pouvez modifier le registre pour modifier cette valeur par défaut.

> [!Caution]
> Une modification incorrecte du Registre peut sérieusement endommager votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.

    1. Démarrez l’éditeur du Registre (regedit. exe).

    2. Recherchez et cliquez sur la clé suivante dans le registre : **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\schedule**

    3. Dans le menu **Edition** , cliquez sur **Ajouter une valeur**, puis ajoutez les valeurs de Registre suivantes :

        - **Nom de la valeur.** atTaskMaxHours

        - **Type de données.** reg_DWOrd 

        - **Dicaux.** Decimal

        - **Données de la valeur :** entre. La valeur 0 dans le champ de données de la valeur indique l’absence de limite et ne se bloque pas. Les valeurs comprises entre 1 et 99 indiquent le nombre d’heures.

- Vous pouvez utiliser le dossier tâches planifiées pour afficher ou modifier les paramètres d’une tâche qui a été créée à l’aide de la commande **at** . Lorsque vous planifiez une tâche à l’aide de la commande **at** , la tâche est indiquée dans le dossier tâches planifiées, avec un nom tel que le suivant :**at3478**. Toutefois, si vous modifiez une tâche par le biais du dossier tâches planifiées, elle est mise à niveau vers une tâche planifiée normale. La tâche n’est plus visible pour la commande **at** et le paramètre at Account ne s’y applique plus. Vous devez entrer explicitement un compte d’utilisateur et un mot de passe pour la tâche.

## <a name="examples"></a>Exemples

Pour afficher la liste des commandes planifiées sur le serveur marketing, tapez :

`at \\marketing`

Pour en savoir plus sur une commande portant le numéro d’identification 3 sur le serveur Corp, tapez :

`at \\corp 3`

Pour planifier l’exécution d’une commande net share sur le serveur Corp à 8:00 h 00 et rediriger la liste vers le serveur de maintenance, dans le répertoire Shared Reports et le fichier Corp. txt, tapez :

`at \\corp 08:00 cmd /c net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt`

Pour sauvegarder le disque dur du serveur marketing sur un lecteur de bande à minuit tous les cinq jours, créez un programme batch nommé Archive. cmd, qui contient les commandes de sauvegarde, puis planifiez l’exécution du programme batch, tapez :

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Pour annuler toutes les commandes planifiées sur le serveur actuel, effacez les informations **à** la planification comme suit :

`at /delete`

Pour exécuter une commande qui n’est pas un fichier exécutable (autrement dit,. exe), faites précéder la commande de **cmd/c** de charger cmd. exe comme suit :

`cmd /c dir > c:\test.out`

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)