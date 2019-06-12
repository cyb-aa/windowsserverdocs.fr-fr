---
title: at
description: Rubrique de commandes de Windows pour **à** - planifie les commandes et programmes à exécuter sur un ordinateur à une date et l’heure spécifiée.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ff18fd16-9437-4c53-8794-bfc67f5256b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fc9d9f3d008db1bb85bfb6afa0308834c929b5f0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435281"
---
# <a name="at"></a>at

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Planifie les commandes et programmes à exécuter sur un ordinateur à une date et heure spécifiées. Vous pouvez utiliser **à** uniquement lorsque le service de planification est en cours d’exécution. Utilisée sans paramètres, **à** répertorie les commandes planifiées.
## <a name="syntax"></a>Syntaxe
```
at [\\computername] [[id] [/delete] | /delete [/yes]]
at [\\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```
## <a name="parameters"></a>Paramètres

|      Paramètre       |                                                                                                                                                                                                               Description                                                                                                                                                                                                                |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \\\\\<computerName\> |                                                                                                                                                        Spécifie un ordinateur distant. Si vous omettez ce paramètre, **à** planifie les commandes et les programmes sur l’ordinateur local.                                                                                                                                                        |
|        \<id\>        |                                                                                                                                                                                   Spécifie le numéro d’identification affecté à une commande planifiée.                                                                                                                                                                                   |
|       /delete        |                                                                                                                                                                Annule une commande planifiée. Si vous omettez *ID*, toutes les commandes planifiées sur l’ordinateur sont annulées.                                                                                                                                                                |
|         /yes         |                                                                                                                                                                               Réponses Oui à toutes les requêtes à partir du système lorsque vous supprimez des événements planifiés.                                                                                                                                                                               |
|       \<time\>       |                                                                                                                                          Spécifie l’heure lorsque vous souhaitez exécuter la commande. heure est exprimée en heures : Minutes en notation 24 heures (autrement dit, 00:00 [minuit] à 23:59).                                                                                                                                          |
|     /interactive     |                                                                                                                                                                  Permet de *commande* pour interagir avec le bureau de l’utilisateur qui a ouvert une session en temps *commande* s’exécute.                                                                                                                                                                  |
|       / chaque :        |                                                                                                                                                    Exécutions *commande* chaque jour spécifié ou les jours de la semaine ou du mois (par exemple, tous les jeudis ou le troisième jour de chaque mois).                                                                                                                                                    |
|       \<date\>       |                                                  Spécifie la date lorsque vous souhaitez exécuter la commande. Vous pouvez spécifier un ou plusieurs jours de la semaine (autrement dit, tapez **M**,**T**,**W**,**Th**,**F**,**S**,**Su**) ou un ou plusieurs jours du mois (autrement dit, de type 1 à 31). Séparez plusieurs entrées de date par des virgules. Si vous omettez *date*, **à** utilise le jour du mois actuel.                                                  |
|        /next:        |                                                                                                                                                                              Exécutions *commande* à la prochaine occurrence du jour (par exemple, le jeudi suivant).                                                                                                                                                                              |
|     \<command\>      | Spécifie la commande Windows, programme (fichier .exe ou .com), ou un programme de traitement par lots (autrement dit, les fichiers .bat ou .cmd) que vous souhaitez exécuter. Lorsque la commande requiert un chemin d’accès en tant qu’argument, utilisez le chemin d’accès absolu (autrement dit, le chemin d’accès complet à partir la lettre de lecteur). Si la commande se trouve sur un ordinateur distant, spécifiez la notation UNC Universal Naming Convention () pour le serveur et nom de partage, plutôt qu’une lettre de lecteur à distance. |
|          /?          |                                                                                                                                                                                                   Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                   |

## <a name="remarks"></a>Notes
- **SCHTASKS** est un autre outil de planification de ligne de commande que vous pouvez utiliser pour créer et gérer des tâches planifiées. Pour plus d’informations sur **schtasks**, consultez les rubriques connexes.
- À l’aide de **à**  
  Pour utiliser **à**, vous devez être membre du groupe Administrateurs local.
- Chargement de Cmd.exe  
  **à** ne charge pas automatiquement l’interpréteur de commandes Cmd.exe avant d’exécuter des commandes. Si vous n’exécutez pas un fichier exécutable (.exe), vous devez charger explicitement Cmd.exe au début de la commande comme suit : **cmd /c dir > c:\test.out**
- Commandes d’affichage planifiée  
  Lorsque vous utilisez **à** sans options de ligne de commande, les tâches planifiées apparaissent dans une table au format similaires à ce qui suit :
  ```
  Status  ID   Day        time        Command Line
  OK      1    Each F     4:30 PM     net send group leads status due
  OK      2    Each M     12:00 AM    chkstor > check.file
  OK      3    Each F     11:59 PM    backup2.bat
  ```
- Y compris le numéro d’identification (*ID*)  
  Lorsque vous incluez le numéro d’identification (*ID*) avec **à** à une invite de commandes, les informations pour une seule entrée s’affichent dans un format similaire à ce qui suit :  
  ```
  Task ID:      1
  Status:       OK
  Schedule:     Each  F
  time of Day:  4:30 PM
  Command:      net send group leads status due
  ```
  Une fois que vous planifiez une commande avec **à**, en particulier une commande qui a des options de ligne de commande, vérifiez que la syntaxe de commande est correcte en tapant **à** sans options de ligne de commande. Si les informations contenues dans la colonne de la ligne de commande sont incorrects, supprimez la commande et retapez-le. Si elles sont toujours incorrectes, retapez la commande avec moins d’options de ligne de commande.
- Affichage des résultats  
  Commandes planifiées avec **à** s’exécutent en arrière-plan. Sortie n’est pas affichée à l’écran. Pour rediriger la sortie vers un fichier, utilisez le symbole de redirection (>). Si vous redirigez la sortie vers un fichier, vous devez utiliser le caractère d’échappement (^) avant le symbole de redirection, que vous utilisiez **à** à la ligne de commande ou dans un fichier de commandes. Par exemple, pour rediriger la sortie vers Output.txt, tapez :  
  `at 14:45 c:\test.bat ^>c:\output.txt`  
  Le répertoire actuel de la commande en cours d’exécution est le dossier systemroot.
- Modification de l’heure système  
  Si vous modifiez l’heure système sur un ordinateur une fois que vous planifiez une commande à exécuter avec **à**, synchroniser le **à** planificateur avec la nouvelle heure système en tapant **à** sans options de ligne de commande.
- Stockage des commandes  
  Commandes planifiées sont stockées dans le Registre. Par conséquent, vous ne perdez pas les tâches planifiées si vous redémarrez le service de planification.
- Connexion au réseau des lecteurs  
  N’utilisez pas un lecteur redirigé pour les tâches planifiées qui accèdent au réseau. Le service de planification ne peut pas être en mesure d’accéder au lecteur redirigé, ou le lecteur redirigé peut ne pas être présent si un autre utilisateur est connecté au moment de que la tâche planifiée s’exécute. Au lieu de cela, utilisez des chemins d’accès UNC pour les tâches planifiées. Exemple :  
  `at 1:00pm my_backup \\\server\share`  
  N’utilisez pas la syntaxe suivante, où **x:** est une connexion établie par l’utilisateur :  
  `at 1:00pm my_backup x:`  
  Si vous planifiez une **à** commande par une lettre de lecteur pour se connecter à un répertoire partagé, inclure un **à** commande pour déconnecter le lecteur lorsque vous avez terminé d’utiliser le lecteur. Si le lecteur n’est pas déconnecté, la lettre de lecteur assignée n’est pas disponible à l’invite de commandes.
- Arrêt des tâches après 72 heures  
  Par défaut, les tâches planifiées à l’aide de la **à** commande stop après 72 heures. Vous pouvez modifier le Registre pour modifier cette valeur par défaut.
  1.  Démarrez l’Éditeur du Registre (regedit.exe).
  2.  Recherchez et cliquez sur la clé suivante dans le Registre : **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Schedule**
  3.  Dans le menu Edition, cliquez sur Ajouter la valeur, puis ajoutez la valeur de Registre suivante : Nom de la valeur : atTaskMaxHours type de données : reg_DWOrd Radix : Données de valeur décimale : 0. La valeur 0 dans le champ de données de valeur n’indique aucune limite, n’arrête pas. Valeurs comprises entre 1 et 99 indique le nombre d’heures.
  **Attention**
- Une modification incorrecte du Registre peut endommager gravement votre système. Avant toute modification du registre, il est conseillé de sauvegarder toutes les données importantes de votre ordinateur.
- Planificateur de tâches et les **à** commande  
  Vous pouvez utiliser le dossier Tâches planifiées pour afficher ou modifier les paramètres d’une tâche qui a été créé à l’aide de la **à** commande. Lorsque vous planifiez une tâche à l’aide de la **à** commande, la tâche est répertoriée dans le dossier Tâches planifiées, avec un nom tel que le suivant :**at3478**. Toutefois, si vous modifiez au niveau de la tâche par le biais du dossier Tâches planifiées, il est mis à niveau vers une tâche planifiée normale. La tâche n’est plus visible à la **à** commande et le compte paramètre ne s’applique plus à ce dernier. Vous devez explicitement entrer un compte d’utilisateur et le mot de passe pour la tâche.
  ## <a name="examples"></a>Exemples
  Pour afficher une liste de commandes planifiée sur le serveur Marketing, tapez :

`at \\marketing`

Pour en savoir plus sur une commande portant le numéro d’identification 3 sur le serveur Soc, tapez :

`at \\corp 3`

Pour planifier une commande net share pour s’exécuter sur le serveur Corp à 8 h 00. et rediriger la liste vers le serveur de Maintenance, dans le répertoire partagé de rapports et le fichier Soc.txt, type :

`at \\corp 08:00 cmd /c "net share reports=d:\marketing\reports >> \\maintenance\reports\corp.txt"`

Pour sauvegarder le disque dur du serveur Marketing sur un lecteur de bande tous les cinq jours à minuit, créez un programme de commandes appelé archives.cmd qui contient les commandes de sauvegarde, et ensuite planifier le programme de traitement par lots à exécuter, tapez :

`at \\marketing 00:00 /every:5,10,15,20,25,30 archive`

Pour annuler toutes les commandes programmées sur le serveur actuel, effacez le **à** informations de planification comme suit :

`at /delete`

Pour exécuter une commande qui n’est pas un fichier exécutable (.exe), faites précéder la commande avec **cmd /c** charger Cmd.exe comme suit :

`cmd /c dir > c:\test.out`
