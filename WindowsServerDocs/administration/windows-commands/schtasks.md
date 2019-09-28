---
title: schtasks
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e713203-3dd8-491b-b9e1-9423618dc7e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8029bff5907c044e51b0a371265c3bde452e1366
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371269"
---
# <a name="schtasks"></a>schtasks



Planifie l’exécution périodique ou à un moment précis des commandes et des programmes. Ajoute et supprime des tâches de la planification, démarre et arrête les tâches à la demande, puis affiche et modifie les tâches planifiées.

Pour afficher la syntaxe de la commande, cliquez sur l’une des commandes suivantes :
-   [créer une schtasks](#BKMK_create)
-   [modification de schtasks](#BKMK_change)
-   [SCHTASKS-exécuter](#BKMK_run)
-   [fin de schtasks](#BKMK_end)
-   [SCHTASKS supprimer](#BKMK_delete)
-   [requête schtasks](#BKMK_query)

## <a name="remarks"></a>Notes

- **Schtasks. exe** effectue les mêmes opérations que les **tâches planifiées** dans le **panneau de configuration**. Vous pouvez utiliser ces outils ensemble et de façon interchangeable.
- **Schtasks** remplace **at. exe**, un outil inclus dans les versions précédentes de Windows. Bien que le fichier **. exe** soit toujours inclus dans la famille Windows Server 2003, **schtasks** est l’outil de planification des tâches de ligne de commande recommandé.
- Les paramètres d’une commande **schtasks** peuvent apparaître dans n’importe quel ordre. Si vous tapez **schtasks** sans aucun paramètre, une requête est exécutée.
- Autorisations pour **schtasks**  
  -   Vous devez avoir l’autorisation d’exécuter la commande. N’importe quel utilisateur peut planifier une tâche sur l’ordinateur local, et il peut afficher et modifier les tâches qu’il a planifiées. Les membres du groupe administrateurs peuvent planifier, afficher et modifier toutes les tâches sur l’ordinateur local.
  -   Pour planifier, afficher ou modifier une tâche sur un ordinateur distant, vous devez être membre du groupe Administrateurs sur l’ordinateur distant, ou vous devez utiliser le paramètre **/u** pour fournir les informations d’identification d’un administrateur de l’ordinateur distant.
  -   Vous pouvez utiliser le paramètre **/u** dans une opération **/Create** ou **/change** uniquement lorsque les ordinateurs locaux et distants se trouvent dans le même domaine ou que l’ordinateur local est dans un domaine approuvé par le domaine de l’ordinateur distant. Dans le cas contraire, l’ordinateur distant ne peut pas authentifier le compte d’utilisateur spécifié et il ne peut pas vérifier que le compte est membre du groupe administrateurs.
  -   La tâche doit être autorisée à s’exécuter. Les autorisations requises varient en fonction de la tâche. Par défaut, les tâches s’exécutent avec les autorisations de l’utilisateur actuel de l’ordinateur local, ou avec les autorisations de l’utilisateur spécifié par le paramètre **/u** , le cas échéant. Pour exécuter une tâche avec les autorisations d’un autre compte d’utilisateur ou avec des autorisations système, utilisez le paramètre **/ru** .
- Pour vérifier qu’une tâche planifiée s’est exécutée ou pour déterminer la raison de l’échec de l’exécution d’une tâche planifiée, consultez le journal des transactions du service Planificateur de tâches, *systemroot*\SchedLgU.txt. Ces enregistrements de journal ont tenté des exécutions lancées par tous les outils qui utilisent le service, y compris les **tâches planifiées** et **schtasks. exe**.
- Dans de rares occasions, les fichiers de tâche sont endommagés. Les tâches endommagées ne sont pas exécutées. Lorsque vous essayez d’effectuer une opération sur des tâches endommagées, **schtasks. exe** affiche le message d’erreur suivant :  
  ```
  ERROR: The data is invalid.
  ```  
  Vous ne pouvez pas récupérer les tâches endommagées. Pour restaurer les fonctionnalités de planification des tâches du système, utilisez **schtasks. exe** ou des **tâches planifiées** pour supprimer les tâches du système et les replanifier.

## <a name="BKMK_create"></a>créer une schtasks

Planifie une tâche.

**Schtasks** utilise des combinaisons de paramètres différentes pour chaque type de planification. Pour voir la syntaxe combinée pour la création de tâches ou pour voir la syntaxe de création d’une tâche avec un type de planification particulier, cliquez sur l’une des options suivantes.
-   [Description combinée de la syntaxe et des paramètres](#BKMK_syntax)
-   [Pour planifier une tâche qui s’exécute toutes les N minutes](#BKMK_minutes)
-   [Pour planifier une tâche qui s’exécute toutes les N heures](#BKMK_hours)
-   [Pour planifier une tâche qui s’exécute tous les N jours](#BKMK_days)
-   [Pour planifier une tâche qui s’exécute toutes les N semaines](#BKMK_weeks)
-   [Pour planifier une tâche qui s’exécute tous les N mois](#BKMK_months)
-   [Pour planifier une tâche qui s’exécute un jour spécifique de la semaine](#BKMK_spec_day)
-   [Pour planifier une tâche qui s’exécute à une semaine spécifique du mois](#BKMK_spec_week)
-   [Pour planifier une tâche qui s’exécute chaque mois à une date spécifique](#BKMK_spec_date)
-   [Pour planifier une tâche qui s’exécute le dernier jour du mois](#BKMK_last_day)
-   [Pour planifier une tâche qui s’exécute une fois](#BKMK_once)
-   [Pour planifier une tâche qui s’exécute chaque fois que le système démarre](#BKMK_startup)
-   [Pour planifier une tâche qui s’exécute lorsqu’un utilisateur ouvre une session](#BKMK_logon)
-   [Pour planifier une tâche qui s’exécute lorsque le système est inactif](#BKMK_idle)
-   [Pour planifier une tâche qui s’exécute maintenant](#BKMK_now)
-   [Pour planifier une tâche qui s’exécute avec des autorisations différentes](#BKMK_diff_perms)
-   [Pour planifier une tâche qui s’exécute avec les autorisations système](#BKMK_sys_perms)
-   [Pour planifier une tâche qui exécute plusieurs programmes](#BKMK_multi_progs)
-   [Pour planifier une tâche qui s’exécute sur un ordinateur distant](#BKMK_remote)

### <a name="BKMK_syntax"></a>Description combinée de la syntaxe et des paramètres

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Paramètres

##### <a name="sc-scheduletype"></a>/SC \<ScheduleType >

Spécifie le type de planification. Les valeurs valides sont les minutes, toutes les heures, tous les jours, toutes les semaines, tous les mois, une fois, ONSTARt, ONLOGON, ONIDLE.

|Type de planification|Description|
|-------------|-----------|
|MINUTE, TOUTES LES HEURES, TOUS LES JOURS, TOUTES LES SEMAINES, TOUS LES MOIS|Spécifie l’unité de temps pour la planification.|
|TOUTES|La tâche s’exécute une fois à une date et une heure spécifiées.|
|ONSTART|La tâche s’exécute chaque fois que le système démarre. Vous pouvez spécifier une date de début ou exécuter la tâche lors du prochain démarrage du système.|
|ONLOGON|La tâche s’exécute chaque fois qu’un utilisateur (n’importe quel utilisateur) se connecte. Vous pouvez spécifier une date ou exécuter la tâche à la prochaine ouverture de session de l’utilisateur.|
|ONIDLE|La tâche s’exécute chaque fois que le système est inactif pendant un laps de temps spécifié. Vous pouvez spécifier une date ou exécuter la tâche la prochaine fois que le système est inactif.|

##### <a name="tn-taskname"></a>/TN \<TaskName >

Spécifie un nom pour la tâche. Chaque tâche sur le système doit avoir un nom unique. Le nom doit être conforme aux règles applicables aux noms de fichiers et ne doit pas dépasser 238 caractères. Utilisez des guillemets pour encadrer les noms qui incluent des espaces.

##### <a name="tr-taskrun"></a>/TR \<TaskRun >

Spécifie le programme ou la commande que la tâche exécute. Tapez le chemin d’accès complet et le nom de fichier d’un fichier exécutable, d’un fichier de script ou d’un fichier de commandes. Le nom du chemin d’accès ne doit pas dépasser 262 caractères. Si vous omettez le chemin d’accès, **schtasks** suppose que le fichier se trouve dans le répertoire *systemroot*\System32

##### <a name="s-computer"></a>/s \<Computer >

Planifie une tâche sur l’ordinateur distant spécifié. Tapez le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barre oblique inverse). La valeur par défaut est l'ordinateur local. Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**.

##### <a name="u-domainuser"></a>/u [\<Domain > \] @ no__t-2

Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est les autorisations de l’utilisateur actuel de l’ordinateur local. Les paramètres **/u** et **/p** ne sont valides que pour la planification d’une tâche sur un ordinateur distant ( **/s**).

Les autorisations du compte spécifié sont utilisées pour planifier la tâche et pour exécuter la tâche. Pour exécuter la tâche avec les autorisations d’un autre utilisateur, utilisez le paramètre **/ru** .

Le compte d’utilisateur doit être membre du groupe Administrateurs sur l’ordinateur distant. En outre, l’ordinateur local doit se trouver dans le même domaine que l’ordinateur distant, ou dans un domaine approuvé par le domaine de l’ordinateur distant.

##### <a name="p-password"></a>/p \<mot de passe >

Fournit le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . Si vous utilisez le paramètre **/u** , mais omettez le paramètre **/p** ou l’argument Password, **schtasks** vous invite à entrer un mot de passe et masque le texte que vous tapez.

Les paramètres **/u** et **/p** ne sont valides que pour la planification d’une tâche sur un ordinateur distant ( **/s**).

##### <a name="ru-domainuser--system"></a>/ru {[\<Domain > \] @ no__t-2 | Requise

Exécute la tâche avec les autorisations du compte d’utilisateur spécifié. Par défaut, la tâche s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local, ou avec l’autorisation de l’utilisateur spécifié par le paramètre **/u** , le cas échéant. Le paramètre **/ru** est valide lors de la planification de tâches sur des ordinateurs locaux ou distants.


|       Value        |                                                    Description                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Domain > \] @ no__t-2 |                                       Spécifie un autre compte d’utilisateur.                                        |
|    Système ou «»    | Spécifie le compte système local, un compte doté de privilèges élevés utilisé par le système d’exploitation et les services système. |

##### <a name="rp-password"></a>/RP \<Password >

Fournit le mot de passe du compte d’utilisateur spécifié dans le paramètre **/ru** . Si vous omettez ce paramètre lorsque vous spécifiez un compte d’utilisateur, **schtasks. exe** vous invite à entrer le mot de passe et masque le texte que vous tapez.

N’utilisez pas le paramètre **/RP** pour les tâches exécutées avec les informations d’identification du compte système ( **/ru System**). Le compte système n’a pas de mot de passe et **schtasks. exe** ne demande pas de mot de passe.

##### <a name="mo-modifier"></a>/Mo \<Modifier >

Spécifie la fréquence d’exécution de la tâche dans son type de planification. Ce paramètre est valide, mais facultatif, pour une MINUTE, toutes les heures, tous les jours, toutes les semaines et tous les mois. La valeur par défaut est 1.

|Type de planification|Valeurs de modificateur|Description|
|-------------|---------------|-----------|
|DERNIÈRES|1 - 1439|La tâche s’exécute chaque fois que @no__t 0N > minutes.|
|HORAIRE|1 - 23|La tâche s’exécute toutes les heures de > @no__t 0N.|
|QUOTIDIENNE|1 - 365|La tâche s’exécute chaque jour @no__t 0N >.|
|HEBDOMADAIRE|1 - 52|La tâche s’exécute chaque semaine de @no__t 0N >.|
|TOUTES|Aucun modificateur.|La tâche s’exécute une seule fois.|
|ONSTART|Aucun modificateur.|La tâche s’exécute au démarrage.|
|ONLOGON|Aucun modificateur.|La tâche s’exécute lorsque l’utilisateur spécifié par le paramètre **/u** ouvre une session.|
|ONIDLE|Aucun modificateur.|La tâche s’exécute une fois que le système est inactif pendant le nombre de minutes spécifié par le paramètre **/i** , ce qui est requis pour une utilisation avec OnIdle.|
|MENSUEL|1 - 12|La tâche s’exécute chaque mois de @no__t 0N >.|
|MENSUEL|LASTDAY|La tâche s’exécute le dernier jour du mois.|
|MENSUEL|PREMIER, DEUXIÈME, TROISIÈME, QUATRIÈME, DERNIER|Utilisez avec le paramètre **/d**\<Day > pour exécuter une tâche sur une semaine et un jour particuliers. Par exemple, le troisième mercredi du mois.|

##### <a name="d-dayday--"></a>/d jour [, jour...] | *

Spécifie un jour (ou des jours) de la semaine ou un ou plusieurs jours d’un mois. Valide uniquement avec une planification hebdomadaire ou mensuelle.


| Type de planification |              Modificateur              |     Valeurs de jour (/d)      |                                                                                                 Description                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    HEBDOMADAIRE     |               1 - 52               | LUN-DIM [, LUN-DIM...] |                                                                                                     \*                                                                                                      |
|    MENSUEL    | PREMIER, DEUXIÈME, TROISIÈME, QUATRIÈME, DERNIER |        LUN-DIM         |                                                                                   Requis pour une planification de semaine spécifique.                                                                                    |
|    MENSUEL    |          Aucun ou {1-12}          |          1 - 31          | Facultatif et valide uniquement avec aucun paramètre de modificateur ( **/Mo**) (une planification de date spécifique) ou lorsque le **/Mo** est 1-12 (une planification «chaque \<n > mois). La valeur par défaut est le jour 1 (le premier jour du mois). |

##### <a name="m-monthmonth"></a>/m mois [, mois...]

Spécifie un ou plusieurs mois de l’année pendant laquelle la tâche planifiée doit s’exécuter. Les valeurs valides sont JAN-DEC et * (tous les mois). Le paramètre **/m** est valide uniquement avec une planification mensuelle. Elle est requise lorsque le modificateur LASTDAY est utilisé. Dans le cas contraire, il est facultatif et la valeur par défaut est * (tous les mois).

##### <a name="i-idletime"></a>/i \<IdleTime >

Spécifie le nombre de minutes d’inactivité de l’ordinateur avant le démarrage de la tâche. Une valeur valide est un nombre entier compris entre 1 et 999. Ce paramètre est valide uniquement avec une planification ONIDLE, puis il est requis.

##### <a name="st-starttime"></a>/St \<StartTime >

Spécifie l’heure de début de la tâche (à chaque démarrage) au \<HH : MM > format 24 heures. La valeur par défaut est l’heure actuelle sur l’ordinateur local. Le paramètre **/St** est valide avec les planifications de minute, horaire, quotidienne, hebdomadaire, mensuelle et une fois. Il est requis pour une planification une fois.

##### <a name="ri-interval"></a>/RI \<Interval >

Spécifie l’intervalle de répétition en minutes. Cela ne s’applique pas aux types de planification : MINUTE, toutes les heures, ONSTARt, ONLOGON et ONIDLE. La plage valide est comprise entre 1 et 599940 minutes (599940 minutes = 9999 heures). Si l’option/ET ou/DU est spécifiée, l’intervalle de répétition est défini par défaut à 10 minutes.

##### <a name="et-endtime"></a>/et \<EndTime >

Spécifie l’heure de la journée à laquelle une planification de tâche de minute ou horaire se termine au \<HH : MM > format 24 heures. Après l’heure de fin spécifiée, **schtasks** ne redémarre pas la tâche avant la récurrence de l’heure de début. Par défaut, les planifications des tâches n’ont pas d’heure de fin. Ce paramètre est facultatif et valide uniquement avec une MINUTE ou une planification horaire.

Pour obtenir un exemple, consultez :
-   « Pour planifier une tâche qui s’exécute toutes les 100 minutes en dehors des heures de bureau » dans la section **pour planifier une tâche qui s’exécute toutes** les \<N > **minutes** .

##### <a name="du-duration"></a>/du \<Duration >

Spécifie une durée maximale pour une minute ou une planification horaire au format \<HHHH : MM > au format 24 heures. Une fois le temps spécifié écoulé, **schtasks** ne redémarre pas la tâche jusqu’à ce que l’heure de début se reproduise. Par défaut, les planifications des tâches n’ont pas de durée maximale. Ce paramètre est facultatif et valide uniquement avec une MINUTE ou une planification horaire.

Pour obtenir un exemple, consultez :
-   « Pour planifier une tâche qui s’exécute toutes les 3 heures pendant 10 heures » dans la section **pour planifier une tâche qui s’exécute toutes** les \<N > **heures** .

##### <a name="k"></a>/k

Arrête le programme que la tâche exécute à l’heure spécifiée par **/et** ou **/du**. Sans **/k**, **schtasks** ne redémarre pas le programme après avoir atteint l’heure spécifiée par **/et** ou **/du**, mais il n’arrête pas le programme s’il est toujours en cours d’exécution. Ce paramètre est facultatif et valide uniquement avec une MINUTE ou une planification horaire.

Pour obtenir un exemple, consultez :
-   « Pour planifier une tâche qui s’exécute toutes les 100 minutes en dehors des heures de bureau » dans la section **pour planifier une tâche qui s’exécute toutes** les \<N > **minutes** .

##### <a name="sd-startdate"></a>/SD \<StartDate >

Spécifie la date de début de la planification des tâches. La valeur par défaut est la date actuelle sur l’ordinateur local. Le paramètre **/SD** est valide et facultatif pour tous les types de planification.

Le format de *StartDate* varie en fonction des paramètres régionaux sélectionnés pour l’ordinateur local dans **Options régionales et linguistiques** du **panneau de configuration**. Un seul format est valide pour chaque paramètre régional.

Les formats de date valides sont répertoriés dans le tableau suivant. Utilisez le format le plus semblable au format sélectionné pour **date abrégée** dans **Options régionales et linguistiques** du **panneau de configuration** sur l’ordinateur local.


|       Value       |                                        Description                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \< MM >/<DD>/<YYYY> | À utiliser pour les formats mensuels, tels que l' **anglais (États-Unis)** et l' **espagnol (Panama)** . |
| \<DD >/<MM> @ NO__T-2 @ NO__T-3 |       À utiliser pour les formats quotidiens, tels que **bulgare** et **néerlandais (Pays-Bas)** .        |
| \<YYYY >/<MM> @ NO__T-2<DD> |          À utiliser pour les formats d’année en premier, tels que **suédois** et **français (Canada)** .          |

/Ed @no__t 0EndDate >

Spécifie la date de fin de la planification. Ce paramètre est facultatif. Elle n’est pas valide dans une planification une fois, ONSTARt, ONLOGON ou ONIDLE. Par défaut, les planifications n’ont pas de date de fin.

Le format de *EndDate* varie en fonction des paramètres régionaux sélectionnés pour l’ordinateur local dans **Options régionales et linguistiques** du **panneau de configuration**. Un seul format est valide pour chaque paramètre régional.

Les formats de date valides sont répertoriés dans le tableau suivant. Utilisez le format le plus semblable au format sélectionné pour **date abrégée** dans **Options régionales et linguistiques** du **panneau de configuration** sur l’ordinateur local.


|       Value       |                                        Description                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \< MM >/<DD>/<YYYY> | À utiliser pour les formats mensuels, tels que l' **anglais (États-Unis)** et l' **espagnol (Panama)** . |
| \<DD >/<MM> @ NO__T-2 @ NO__T-3 |       À utiliser pour les formats quotidiens, tels que **bulgare** et **néerlandais (Pays-Bas)** .        |
| \<YYYY >/<MM> @ NO__T-2<DD> |          À utiliser pour les formats d’année en premier, tels que **suédois** et **français (Canada)** .          |

##### <a name="it"></a>/IT

Spécifie d’exécuter la tâche uniquement lorsque l’utilisateur « exécuter en tant que » (compte d’utilisateur sous lequel la tâche s’exécute) est connecté à l’ordinateur. Ce paramètre n’a aucun effet sur les tâches qui s’exécutent avec des autorisations système.

Par défaut, l’utilisateur « exécuter en tant que » est l’utilisateur actuel de l’ordinateur local lorsque la tâche est planifiée ou le compte spécifié par le paramètre **/u** , le cas échéant. Toutefois, si la commande comprend le paramètre **/ru** , l’utilisateur « exécuter en tant que » est le compte spécifié par le paramètre **/ru** .

Pour obtenir des exemples, consultez :
-   « Pour planifier une tâche qui s’exécute tous les 70 jours si je suis connecté » dans la section **pour planifier une tâche qui s’exécute tous** les *N* **jours** .
-   « Pour exécuter une tâche uniquement lorsqu’un utilisateur particulier est connecté » dans la section **pour planifier une tâche qui s’exécute avec des autorisations différentes** .

##### <a name="z"></a>z

Spécifie que la tâche doit être supprimée à la fin de sa planification.

##### <a name="f"></a>/f

Spécifie de créer la tâche et de supprimer les avertissements si la tâche spécifiée existe déjà.

##### <a name=""></a>/?

Affiche l'aide à l'invite de commandes.

### <a name="BKMK_minutes"></a>Pour planifier une tâche qui s’exécute toutes les N minutes

#### <a name="minute-schedule-syntax"></a>Syntaxe des planifications par minute

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification de minute, le paramètre **/SC minute** est requis. Le paramètre **/Mo** (modificateur) est facultatif et spécifie le nombre de minutes entre chaque exécution de la tâche. La valeur par défaut de **/Mo** est 1 (toutes les minutes). Les paramètres **/et** (heure de fin) et **/du** (durée) sont facultatifs et peuvent être utilisés avec ou sans le paramètre **/k** (tâche de fin).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Pour planifier une tâche qui s’exécute toutes les 20 minutes

La commande suivante planifie l’exécution d’un script de sécurité, sec. vbs, toutes les 20 minutes. La commande utilise le paramètre **/SC** pour spécifier une planification de minute et le paramètre **/Mo** pour spécifier un intervalle de 20 minutes.

Étant donné que la commande n’inclut pas de date ou d’heure de début, la tâche démarre 20 minutes après la fin de la commande et s’exécute toutes les 20 minutes après chaque exécution du système. Notez que le fichier source du script de sécurité se trouve sur un ordinateur distant, mais que la tâche est planifiée et s’exécute sur l’ordinateur local.
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Pour planifier une tâche qui s’exécute toutes les 100 minutes en dehors des heures de bureau

La commande suivante planifie un script de sécurité, sec. vbs, pour qu’il s’exécute sur l’ordinateur local toutes les 100 minutes entre 5:00 h 00. et 7:59 h 00 tous les jours. La commande utilise le paramètre **/SC** pour spécifier une planification de minute et le paramètre **/Mo** pour spécifier un intervalle de 100 minutes. Elle utilise les paramètres **/St** et **/et** pour spécifier l’heure de début et l’heure de fin de la planification de chaque jour. Il utilise également le paramètre **/k** pour arrêter le script s’il est toujours en cours d’exécution à 7:59 h 00. Sans **/k**, **schtasks** ne démarre pas le script après 7:59, mais si l’instance a démarré à 6:20 h 00 était toujours en cours d’exécution, il ne l’arrêtera pas.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>Pour planifier une tâche qui s’exécute toutes les N heures

#### <a name="hourly-schedule-syntax"></a>Syntaxe de planification horaire

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Au cours d’une planification horaire, le paramètre **/SC HOURLY** est requis. Le paramètre **/Mo** (modificateur) est facultatif et spécifie le nombre d’heures entre chaque exécution de la tâche. La valeur par défaut de **/Mo** est 1 (toutes les heures). Le paramètre **/k** (tâche fin) est facultatif et peut être utilisé avec l’option **/et** (terminer à l’heure spécifiée) ou **/du** (fin après l’intervalle spécifié).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Pour planifier une tâche qui s’exécute toutes les cinq heures

La commande suivante planifie l’exécution du programme MonApp toutes les cinq heures à compter du premier jour du 2002 mars. Elle utilise le paramètre **/Mo** pour spécifier l’intervalle et le paramètre **/SD** pour spécifier la date de début. Étant donné que la commande ne spécifie pas d’heure de début, l’heure actuelle est utilisée comme heure de début.

Étant donné que l’ordinateur local est configuré pour utiliser l’option **anglais (Zimbabwe)** dans **Options régionales et linguistiques** du **panneau de configuration**, le format de la date de début est le suivant : MM/JJ/AAAA (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Pour planifier une tâche qui s’exécute toutes les heures à cinq minutes après l’heure

La commande suivante planifie le programme MonApp pour qu’il s’exécute toutes les heures à partir de 5 minutes après minuit. Étant donné que le paramètre **/Mo** est omis, la commande utilise la valeur par défaut pour la planification horaire, qui est toutes les (1) heure. Si cette commande s’exécute après 12:05, le programme ne s’exécute pas avant le jour suivant.
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Pour planifier une tâche qui s’exécute toutes les 3 heures pendant 10 heures

La commande suivante planifie l’exécution du programme MonApp toutes les 3 heures pendant 10 heures.

La commande utilise le paramètre **/SC** pour spécifier une planification horaire et le paramètre **/Mo** pour spécifier l’intervalle de 3 heures. Elle utilise le paramètre **/St** pour démarrer la planification à minuit et le paramètre **/du** pour mettre fin aux récurrences après 10 heures. Étant donné que le programme s’exécute pendant quelques minutes seulement, le paramètre **/k** , qui arrête le programme s’il est toujours en cours d’exécution lorsque la durée expire, n’est pas nécessaire.
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
Dans cet exemple, la tâche s’exécute à 12:00 h 00, 3:00 h 00, 6:00 h 00 et 9:00 PST. Étant donné que la durée est de 10 heures, la tâche n’est pas exécutée à nouveau à 12:00 h 00. Au lieu de cela, il redémarre à 12:00 du matin. le jour suivant.

### <a name="BKMK_days"></a>Pour planifier une tâche qui s’exécute tous les N jours

#### <a name="daily-schedule-syntax"></a>Syntaxe de planification quotidienne

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification quotidienne, le paramètre **/SC Daily** est requis. Le paramètre **/Mo** (modificateur) est facultatif et spécifie le nombre de jours entre chaque exécution de la tâche. La valeur par défaut de **/Mo** est 1 (tous les jours).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Pour planifier une tâche qui s’exécute tous les jours

L’exemple suivant planifie l’exécution du programme MonApp une fois par jour, tous les jours, à 8:00 h 00. jusqu’au 31 décembre 2002. Étant donné qu’il omet le paramètre **/Mo** , l’intervalle par défaut de 1 est utilisé pour exécuter la commande tous les jours.

Dans cet exemple, comme le système de l’ordinateur local est défini sur l’option **anglais (Royaume-Uni)** dans **Options régionales et linguistiques** du **panneau de configuration**, le format de la date de fin est jj/mm/aaaa (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Pour planifier une tâche qui s’exécute tous les 12 jours

L’exemple suivant planifie l’exécution du programme MonApp tous les douze jours à 1:00 h 00. (13:00) à partir du 31 décembre 2002. La commande utilise le paramètre **/Mo** pour spécifier un intervalle de deux (2) jours et les paramètres **/SD** et **/St** pour spécifier la date et l’heure.

Dans cet exemple, étant donné que le système est défini sur l’option **anglais (Zimbabwe)** dans **Options régionales et linguistiques** du **panneau de configuration**, le format de la date de fin est MM/JJ/AAAA (12/31/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Pour planifier une tâche qui s’exécute tous les 70 jours si je suis connecté

La commande suivante planifie l’exécution d’un script de sécurité, sec. vbs, tous les 70 jours. La commande utilise le paramètre **/Mo** pour spécifier un intervalle de 70 jours. Elle utilise également le paramètre **/IT** pour spécifier que la tâche s’exécute uniquement lorsque l’utilisateur sous le compte duquel la tâche est exécutée est enregistré sur l’ordinateur. Étant donné que la tâche s’exécutera avec les autorisations de mon compte d’utilisateur, la tâche s’exécutera uniquement lorsque je suis connecté.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Pour identifier les tâches avec la propriété interactif uniquement ( **/IT**), utilisez une requête détaillée **(/query/v**). Dans l’affichage des requêtes détaillées d’une tâche avec l’opération **/IT**, le champ **mode d’ouverture de session** a la valeur **interactif uniquement**.

### <a name="BKMK_weeks"></a>Pour planifier une tâche qui s’exécute toutes les N semaines

#### <a name="weekly-schedule-syntax"></a>Syntaxe de planification hebdomadaire

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification hebdomadaire, le paramètre **/SC Weekly** est requis. Le paramètre **/Mo** (modificateur) est facultatif et spécifie le nombre de semaines entre chaque exécution de la tâche. La valeur par défaut de **/Mo** est 1 (toutes les semaines).

Les planifications hebdomadaires ont également un paramètre **/d** facultatif pour planifier l’exécution de la tâche les jours spécifiés de la semaine, ou tous les jours ( *). La valeur par défaut est MON (lundi). L’option tous les jours (* ) est équivalente à la planification d’une tâche quotidienne.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Pour planifier une tâche qui s’exécute toutes les six semaines

La commande suivante permet de planifier l’exécution du programme MonApp sur un ordinateur distant toutes les six semaines. La commande utilise le paramètre **/Mo** pour spécifier l’intervalle. Étant donné que la commande omet le paramètre **/d** , la tâche s’exécute le lundi.

Cette commande utilise également le paramètre **/s** pour spécifier l’ordinateur distant et le paramètre **/u** pour exécuter la commande avec les autorisations du compte d’administrateur de l’utilisateur. Étant donné que le paramètre **/p** est omis, **schtasks. exe** invite l’utilisateur à entrer le mot de passe du compte administrateur.

En outre, étant donné que la commande est exécutée à distance, tous les chemins d’accès de la commande, y compris le chemin d’accès à MyApp. exe, font référence aux chemins d’accès sur l’ordinateur distant.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Pour planifier une tâche qui s’exécute toutes les deux semaines le vendredi

La commande suivante planifie une tâche pour qu’elle s’exécute tous les autres vendredis. Elle utilise le paramètre **/Mo** pour spécifier l’intervalle de deux semaines et le paramètre **/d** pour spécifier le jour de la semaine. Pour planifier une tâche qui s’exécute tous les vendredis, omettez le paramètre **/Mo** ou affectez-lui la valeur 1.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>Pour planifier une tâche qui s’exécute tous les N mois

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans ce type de planification, le paramètre **/sc monthly** est requis. Le paramètre **/Mo** (modificateur), qui spécifie le nombre de mois entre chaque exécution de la tâche, est facultatif et la valeur par défaut est 1 (tous les mois). Ce type de planification a également un paramètre **/d** facultatif pour planifier l’exécution de la tâche à une date spécifiée du mois. La valeur par défaut est 1 (le premier jour du mois).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Pour planifier une tâche qui s’exécute le premier jour de chaque mois

La commande suivante planifie l’exécution du programme MonApp le premier jour de chaque mois. Étant donné que la valeur 1 est la valeur par défaut pour le paramètre **/Mo** (modificateur) et le paramètre **/d** (Day), ces paramètres sont omis de la commande.
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Pour planifier une tâche qui s’exécute tous les trois mois

La commande suivante planifie l’exécution du programme MonApp tous les trois mois. Elle utilise le paramètre **/Mo** pour spécifier l’intervalle.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Pour planifier une tâche qui s’exécute à minuit le 21 de chaque mois

La commande suivante planifie le programme MonApp pour qu’il s’exécute tous les deux mois le 21 du mois à minuit. La commande spécifie que cette tâche doit s’exécuter pendant un an, du 2 juillet au 2002 au 30 juin 2003.

La commande utilise le paramètre **/Mo** pour spécifier l’intervalle mensuel (tous les deux mois), le paramètre **/d** pour spécifier la date et l’option **/St** pour spécifier l’heure. Elle utilise également les paramètres **/SD** et **/Ed** pour spécifier respectivement la date de début et la date de fin. Étant donné que l’ordinateur local est défini sur l’option **anglais (Afrique du Sud)** dans **Options régionales et linguistiques** du **panneau de configuration**, les dates sont spécifiées au format local aaaa/mm/jj.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>Pour planifier une tâche qui s’exécute un jour spécifique de la semaine

#### <a name="weekly-schedule-syntax"></a>Syntaxe de planification hebdomadaire

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

La planification « jour de la semaine » est une variante de la planification hebdomadaire. Dans une planification hebdomadaire, le paramètre **/SC Weekly** est requis. Le paramètre **/Mo** (modificateur) est facultatif et spécifie le nombre de semaines entre chaque exécution de la tâche. La valeur par défaut de **/Mo** est 1 (toutes les semaines). Le paramètre **/d** , qui est facultatif, planifie la tâche pour qu’elle s’exécute les jours spécifiés de la semaine ou tous les jours (\*). La valeur par défaut est MON (lundi). L’option tous les jours ( **/d \*** ) est équivalente à la planification d’une tâche quotidienne.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Pour planifier une tâche qui s’exécute tous les mercredis

La commande suivante planifie l’exécution du programme MonApp chaque semaine le mercredi. La commande utilise le paramètre **/d** pour spécifier le jour de la semaine. Étant donné que la commande omet le paramètre **/Mo** , la tâche s’exécute toutes les semaines.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Pour planifier une tâche qui s’exécute toutes les huit semaines le lundi et le vendredi

La commande suivante planifie l’exécution d’une tâche le lundi et le vendredi de chaque huitième semaine. Elle utilise le paramètre **/d** pour spécifier les jours et le paramètre **/Mo** pour spécifier l’intervalle de huit semaines.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>Pour planifier une tâche qui s’exécute à une semaine spécifique du mois

#### <a name="specific-week-syntax"></a>Syntaxe de semaine spécifique

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans ce type de planification, le paramètre **/sc monthly** , le paramètre **/Mo** (modificateur) et le paramètre **/d** (Day) sont requis. Le paramètre **/Mo** (modificateur) spécifie la semaine d’exécution de la tâche. Le paramètre **/d** spécifie le jour de la semaine. (Vous ne pouvez spécifier qu’un seul jour de la semaine pour ce type de planification.) Cette planification possède également un paramètre facultatif **/m** (mois) qui vous permet de planifier la tâche pour des mois particuliers ou tous les mois (\*). La valeur par défaut du paramètre **/m** est tous les mois (\*).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Pour planifier une tâche le deuxième dimanche de chaque mois

La commande suivante planifie l’exécution du programme MonApp le deuxième dimanche de chaque mois. Elle utilise le paramètre **/Mo** pour spécifier la deuxième semaine du mois et le paramètre **/d** pour spécifier le jour.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Pour planifier une tâche pour le premier lundi en mars et en septembre

La commande suivante permet de planifier l’exécution du programme MonApp le premier lundi en mars et en septembre. Elle utilise le paramètre **/Mo** pour spécifier la première semaine du mois et le paramètre **/d** pour spécifier le jour. Elle utilise le paramètre **/m** pour spécifier le mois, en séparant les arguments month par une virgule.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>Pour planifier une tâche qui s’exécute chaque mois à une date spécifique

#### <a name="specific-date-syntax"></a>Syntaxe de date spécifique

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification de date spécifique, le paramètre **/SC month** et le paramètre **/d** (Day) sont requis. Le paramètre **/d** spécifie une date du mois (1-31), et non un jour de la semaine. Vous ne pouvez spécifier qu’un seul jour dans la planification. Le paramètre **/Mo** (modificateur) n’est pas valide avec ce type de planification.

Le paramètre **/m** (month) est facultatif pour ce type de planification et la valeur par défaut est tous les mois (<em>). **schtasks</em>*  ne vous permet pas de planifier une tâche pour une date qui ne se produit pas dans un mois spécifié par le paramètre **/m** . Toutefois, si vous omettez le paramètre **/m** et planifiez une tâche pour une date qui n’apparaît pas chaque mois, comme le 31ème jour, la tâche ne s’exécute pas dans les mois plus courts. Pour planifier une tâche le dernier jour du mois, utilisez le type de planification de la dernière journée.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Pour planifier une tâche le premier jour de chaque mois

La commande suivante planifie l’exécution du programme MonApp le premier jour de chaque mois. Étant donné que le modificateur par défaut est None (aucun modificateur), le jour par défaut est Day 1 et le mois par défaut est tous les mois, la commande n’a pas besoin de paramètres supplémentaires.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Pour planifier une tâche pour le quinzième jour de mai et juin

La commande suivante planifie l’exécution du programme MonApp le 15 mai et le 15 juin à 3:00 h 00. (15:00). Elle utilise le paramètre **/m** pour spécifier la date et le paramètre **/m** pour spécifier les mois. Elle utilise également le paramètre **/St** pour spécifier l’heure de début.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>Pour planifier une tâche qui s’exécute le dernier jour du mois

#### <a name="last-day-syntax"></a>Syntaxe du dernier jour

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification du dernier jour, le paramètre **/sc monthly** , le paramètre **/Mo LASTDAY** (modificateur) et le paramètre **/m** (month) sont requis. Le paramètre **/d** (Day) n’est pas valide.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Pour planifier une tâche le dernier jour de chaque mois

La commande suivante planifie l’exécution du programme MonApp le dernier jour de chaque mois. Elle utilise le paramètre **/Mo** pour spécifier le dernier jour et le paramètre **/m** avec le caractère générique (*) pour indiquer que le programme s’exécute chaque mois.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Pour planifier une tâche à 6:00 h 00 les derniers jours de février et de mars

La commande suivante planifie l’exécution du programme MonApp le dernier jour de février et le dernier jour de mars à 6:00 h 00. Elle utilise le paramètre **/Mo** pour spécifier le dernier jour, le paramètre **/m** pour spécifier les mois et le paramètre **/St** pour spécifier l’heure de début.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>Pour planifier une tâche qui s’exécute une fois

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification exécuter une fois, le paramètre **/SC once** est requis. Le paramètre **/St** , qui spécifie l’heure d’exécution de la tâche, est obligatoire. Le paramètre **/SD** , qui spécifie la date d’exécution de la tâche, est facultatif. Les paramètres **/Mo** (modificateur) et **/Ed** (date de fin) ne sont pas valides pour ce type de planification.

**Schtasks** ne vous permet pas de planifier l’exécution d’une tâche une seule fois si la date et l’heure spécifiées se trouvent dans le passé, en fonction de l’heure de l’ordinateur local. Pour planifier une tâche qui s’exécute une seule fois sur un ordinateur distant situé dans un autre fuseau horaire, vous devez la planifier avant que la date et l’heure ne se produisent sur l’ordinateur local.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Pour planifier une tâche qui s’exécute une fois

La commande suivante planifie le programme MonApp pour qu’il s’exécute à minuit le 1er janvier 2003. Elle utilise le paramètre **/SC** pour spécifier le type de planification et les paramètres **/SD** et **St** pour spécifier la date et l’heure.

Étant donné que l’ordinateur local utilise l’option **anglais (États-Unis)** dans **Options régionales et linguistiques** du **panneau de configuration**, le format de la date de début est le suivant : mm/jj/aaaa.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>Pour planifier une tâche qui s’exécute chaque fois que le système démarre

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification à l’ouverture, le paramètre **/SC OnStart** est requis. Le paramètre **/SD** (date de début) est facultatif et la valeur par défaut est la date actuelle.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Pour planifier une tâche qui s’exécute au démarrage du système

La commande suivante planifie l’exécution du programme MonApp chaque fois que le système démarre, à partir du 15 mars 2001 :

Étant donné que l’ordinateur local utilise l’option **anglais (États-Unis)** dans **Options régionales et linguistiques** du **panneau de configuration**, le format de la date de début est le suivant : mm/jj/aaaa.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>Pour planifier une tâche qui s’exécute lorsqu’un utilisateur ouvre une session

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Le type de planification « à la connexion » planifie une tâche qui s’exécute chaque fois qu’un utilisateur ouvre une session sur l’ordinateur. Dans le type de planification « on Logon », le paramètre **/SC ONLOGON** est requis. Le paramètre **/SD** (date de début) est facultatif et la valeur par défaut est la date actuelle.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Pour planifier une tâche qui s’exécute lorsqu’un utilisateur se connecte à un ordinateur distant

La commande suivante planifie l’exécution d’un fichier de commandes chaque fois qu’un utilisateur (n’importe quel utilisateur) se connecte à l’ordinateur distant. Elle utilise le paramètre **/s** pour spécifier l’ordinateur distant. Étant donné que la commande est distante, tous les chemins d’accès de la commande, y compris le chemin d’accès au fichier de commandes, font référence à un chemin d’accès sur l’ordinateur distant.
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>Pour planifier une tâche qui s’exécute lorsque le système est inactif

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Le type de planification « en veille » planifie une tâche qui s’exécute chaque fois qu’il n’y a aucune activité de l’utilisateur pendant la durée spécifiée par le paramètre **/i** . Dans le type de planification « en cas d’inactivité », le paramètre **/SC OnIdle** et le paramètre **/i** sont requis. L’option **/SD** (date de début) est facultative et la valeur par défaut est la date actuelle.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Pour planifier une tâche qui s’exécute lorsque l’ordinateur est inactif

La commande suivante permet de planifier l’exécution du programme MonApp lorsque l’ordinateur est inactif. Elle utilise le paramètre **/i** requis pour spécifier que l’ordinateur doit rester inactif pendant dix minutes avant le démarrage de la tâche.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>Pour planifier une tâche qui s’exécute maintenant

**Schtasks** n’a pas l’option « Exécuter maintenant », mais vous pouvez simuler cette option en créant une tâche qui s’exécute une fois et démarre en quelques minutes.

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Pour planifier une tâche qui s’exécute quelques minutes à partir de maintenant.

La commande suivante planifie l’exécution d’une tâche une fois, le 13 novembre 2002 à 2:18 P.M. heure locale.

Étant donné que l’ordinateur local utilise l’option **anglais (États-Unis)** dans **Options régionales et linguistiques** du **panneau de configuration**, le format de la date de début est le suivant : mm/jj/aaaa.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>Pour planifier une tâche qui s’exécute avec des autorisations différentes

Vous pouvez planifier l’exécution des tâches de tous les types avec les autorisations d’un autre compte sur l’ordinateur local et sur un ordinateur distant. Outre les paramètres requis pour le type de planification particulier, le paramètre **/ru** est requis et le paramètre **/RP** est facultatif.

#### <a name="examples"></a>Exemples

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Pour exécuter une tâche avec des autorisations d’administrateur sur l’ordinateur local

La commande suivante permet de planifier l’exécution du programme MonApp sur l’ordinateur local. Elle utilise **/ru** pour spécifier que la tâche doit s’exécuter avec les autorisations du compte d’administrateur de l’utilisateur (Admin06). Dans cet exemple, la tâche est planifiée pour s’exécuter tous les mardis, mais vous pouvez utiliser n’importe quel type de planification pour une tâche exécuter avec d’autres autorisations.
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
En réponse, **schtasks. exe** vous invite à entrer le mot de passe « exécuter en tant que » pour le compte Admin06, puis affiche un message de réussite.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Pour exécuter une tâche avec d’autres autorisations sur un ordinateur distant

La commande suivante permet de planifier l’exécution du programme MonApp sur l’ordinateur de marketing tous les quatre jours.

La commande utilise le paramètre **/SC** pour spécifier une planification quotidienne et un paramètre **/Mo** pour spécifier un intervalle de quatre jours.

La commande utilise le paramètre **/s** pour fournir le nom de l’ordinateur distant et le paramètre **/u** pour spécifier un compte autorisé à planifier une tâche sur l’ordinateur distant (Admin01 sur l’ordinateur de marketing). Elle utilise également le paramètre **/ru** pour spécifier que la tâche doit s’exécuter avec les autorisations du compte non administrateur de l’utilisateur (User01 dans le domaine reskits). Sans le paramètre **/ru** , la tâche s’exécute avec les autorisations du compte spécifié par **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**Schtasks** commence par demander le mot de passe de l’utilisateur nommé par le paramètre **/u** (pour exécuter la commande), puis demande le mot de passe de l’utilisateur nommé par le paramètre **/ru** (pour exécuter la tâche). Après avoir authentifié les mots de passe, **schtasks** affiche un message indiquant que la tâche est planifiée.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Pour exécuter une tâche uniquement lorsqu’un utilisateur particulier est connecté

La commande suivante permet de planifier l’exécution du programme AdminCheck. exe sur l’ordinateur public chaque vendredi à 4:00 h 00, mais uniquement si l’administrateur de l’ordinateur est connecté.

La commande utilise le paramètre **/SC** pour spécifier une planification hebdomadaire, le paramètre **/d** pour spécifier le jour et le paramètre **/St** pour spécifier l’heure de début.

La commande utilise le paramètre **/s** pour fournir le nom de l’ordinateur distant et le paramètre **/u** pour spécifier un compte autorisé à planifier une tâche sur l’ordinateur distant. Elle utilise également le paramètre **/ru** pour configurer la tâche afin qu’elle s’exécute avec les autorisations de l’administrateur de l’ordinateur public (Public\Admin01) et le paramètre **/IT** pour indiquer que la tâche s’exécute uniquement lorsque le compte Public\Admin01 est connecté.
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Remarque**
-   Pour identifier les tâches avec la propriété interactif uniquement ( **/IT**), utilisez une requête détaillée **(/query/v**). Dans l’affichage des requêtes détaillées d’une tâche avec l’opération **/IT**, le champ **mode d’ouverture de session** a la valeur **interactif uniquement**.

### <a name="BKMK_sys_perms"></a>Pour planifier une tâche qui s’exécute avec les autorisations système

Les tâches de tous les types peuvent s’exécuter avec les autorisations du compte système sur l’ordinateur local et un ordinateur distant. Outre les paramètres requis pour le type de planification particulier, le paramètre **système/ru** (ou **/ru «»** ) est requis et le paramètre **/RP** n’est pas valide.

**Important**
-   Le compte système ne dispose pas de droits d’ouverture de session interactifs. Les utilisateurs ne peuvent pas voir ou interagir avec des programmes ou des tâches exécutés avec des autorisations système.
-   Le paramètre **/ru** détermine les autorisations sous lesquelles la tâche est exécutée, et non les autorisations utilisées pour planifier la tâche. Seuls les administrateurs peuvent planifier des tâches, quelle que soit la valeur du paramètre **/ru** .

**Remarque**

Pour identifier les tâches qui s’exécutent avec des autorisations système, utilisez une requête détaillée ( **/query** **/v**). Dans l’affichage de requête détaillée d’une tâche d’exécution système, le champ **exécuter en tant qu’utilisateur** a la valeur **NT AUTHORITY\SYSTEM** et le champ **mode d’ouverture de session** a la valeur **Background only**.

#### <a name="examples"></a>Exemples

#### <a name="to-run-a-task-with-system-permissions"></a>Pour exécuter une tâche avec des autorisations système

La commande suivante permet de planifier l’exécution du programme MonApp sur l’ordinateur local avec les autorisations du compte système. Dans cet exemple, la tâche est planifiée pour s’exécuter le quinzième jour de chaque mois, mais vous pouvez utiliser n’importe quel type de planification pour une tâche exécutée avec des autorisations système.

La commande utilise le paramètre **système/ru** pour spécifier le contexte de sécurité du système. Étant donné que les tâches système n’utilisent pas de mot de passe, le paramètre **/RP** est omis.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
En réponse, **schtasks. exe** affiche un message d’information et un message de réussite. Elle ne demande pas de mot de passe.
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Pour exécuter une tâche avec des autorisations système sur un ordinateur distant

La commande suivante permet de planifier l’exécution du programme MonApp sur l’ordinateur Finance01 chaque matin à 4:00 h 00. avec les autorisations système.

La commande utilise le paramètre **/TN** pour nommer la tâche et le paramètre **/TR** pour spécifier la copie distante du programme MonApp. Elle utilise le paramètre **/SC** pour spécifier une planification quotidienne, mais omet le paramètre **/Mo** , car 1 (tous les jours) est la valeur par défaut. Elle utilise le paramètre **/St** pour spécifier l’heure de début, qui est également l’heure à laquelle la tâche est exécutée chaque jour.

La commande utilise le paramètre **/s** pour fournir le nom de l’ordinateur distant et le paramètre **/u** pour spécifier un compte autorisé à planifier une tâche sur l’ordinateur distant. Elle utilise également le paramètre **/ru** pour spécifier que la tâche doit s’exécuter sous le compte système. Sans le paramètre **/ru** , la tâche s’exécute avec les autorisations du compte spécifié par **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**Schtasks** demande le mot de passe de l’utilisateur nommé par le paramètre **/u** et, après l’authentification du mot de passe, affiche un message indiquant que la tâche est créée et qu’elle s’exécute avec les autorisations du compte système.
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>Pour planifier une tâche qui exécute plusieurs programmes

Chaque tâche exécute un seul programme. Toutefois, vous pouvez créer un fichier de commandes qui exécute plusieurs programmes, puis planifier une tâche pour exécuter le fichier de commandes. La procédure suivante illustre cette méthode :
1. Créez un fichier de commandes qui démarre les programmes que vous souhaitez exécuter.

   Dans cet exemple, vous créez un fichier de commandes qui démarre observateur d’événements (eventvwr. exe) et le moniteur système (Perfmon. exe).  
   - Ouvrez un éditeur de texte, tel que le Bloc-notes.
   - Tapez le nom et le chemin d’accès complet au fichier exécutable pour chaque programme. Dans ce cas, le fichier comprend les instructions suivantes.  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - Enregistrez le fichier sous MyApps. bat.
2. Utilisez **schtasks. exe** pour créer une tâche qui exécute myapps. bat.

   La commande suivante crée la tâche d’analyse, qui s’exécute chaque fois qu’un utilisateur ouvre une session. Elle utilise le paramètre **/TN** pour nommer la tâche et le paramètre **/TR** pour exécuter myapps. bat. Elle utilise le paramètre **/SC** pour indiquer le type de planification ONLOGON et le paramètre **/ru** pour exécuter la tâche avec les autorisations du compte d’administrateur de l’utilisateur.  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   À la suite de cette commande, chaque fois qu’un utilisateur ouvre une session sur l’ordinateur, la tâche démarre à la fois observateur d’événements et le moniteur système.

### <a name="BKMK_remote"></a>Pour planifier une tâche qui s’exécute sur un ordinateur distant

Pour planifier l’exécution d’une tâche sur un ordinateur distant, vous devez ajouter la tâche à la planification de l’ordinateur distant. Les tâches de tous les types peuvent être planifiées sur un ordinateur distant, mais les conditions suivantes doivent être remplies.
-   Vous devez être autorisé à planifier la tâche. Par conséquent, vous devez avoir ouvert une session sur l’ordinateur local avec un compte membre du groupe Administrateurs sur l’ordinateur distant, ou vous devez utiliser le paramètre **/u** pour fournir les informations d’identification d’un administrateur de l’ordinateur distant.
-   Vous pouvez utiliser le paramètre **/u** uniquement lorsque les ordinateurs locaux et distants se trouvent dans le même domaine ou que l’ordinateur local est dans un domaine approuvé par le domaine de l’ordinateur distant. Dans le cas contraire, l’ordinateur distant ne peut pas authentifier le compte d’utilisateur spécifié et il ne peut pas vérifier que le compte est membre du groupe administrateurs.
-   La tâche doit disposer des autorisations suffisantes pour s’exécuter sur l’ordinateur distant. Les autorisations requises varient en fonction de la tâche. Par défaut, la tâche s’exécute avec l’autorisation de l’utilisateur actuel de l’ordinateur local ou, si le paramètre **/u** est utilisé, la tâche s’exécute avec l’autorisation du compte spécifié par le paramètre **/u** . Toutefois, vous pouvez utiliser le paramètre **/ru** pour exécuter la tâche avec les autorisations d’un autre compte d’utilisateur ou avec des autorisations système.

#### <a name="examples"></a>Exemples

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Un administrateur planifie une tâche sur un ordinateur distant

La commande suivante planifie le programme MonApp pour qu’il s’exécute sur l’ordinateur distant SRV01 tous les dix jours démarrant immédiatement. La commande utilise le paramètre **/s** pour fournir le nom de l’ordinateur distant. Étant donné que l’utilisateur local actuel est un administrateur de l’ordinateur distant, le paramètre **/u** , qui fournit d’autres autorisations pour la planification de la tâche, n’est pas nécessaire.

Notez que lorsque vous planifiez des tâches sur un ordinateur distant, tous les paramètres font référence à l’ordinateur distant. Par conséquent, le fichier exécutable spécifié par le paramètre **/TR** fait référence à la copie de MyApp. exe sur l’ordinateur distant.
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
En réponse, **schtasks** affiche un message de réussite indiquant que la tâche est planifiée.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Un utilisateur planifie une commande sur un ordinateur distant (cas 1)

La commande suivante permet de planifier l’exécution du programme MonApp sur l’ordinateur distant SRV06 toutes les trois heures. Étant donné que les autorisations d’administrateur sont requises pour planifier une tâche, la commande utilise les paramètres **/u** et **/p** pour fournir les informations d’identification du compte d’administrateur de l’utilisateur (Admin01 dans le domaine reskits). Par défaut, ces autorisations sont également utilisées pour exécuter la tâche. Toutefois, étant donné que la tâche n’a pas besoin d’autorisations d’administrateur pour s’exécuter, la commande comprend les paramètres **/u** et **/RP** pour remplacer la valeur par défaut et exécuter la tâche avec l’autorisation du compte non administrateur de l’utilisateur sur l’ordinateur distant.
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
En réponse, **schtasks** affiche un message de réussite indiquant que la tâche est planifiée.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Un utilisateur planifie une commande sur un ordinateur distant (cas 2)

La commande suivante permet de planifier l’exécution du programme MonApp sur l’ordinateur distant SRV02 le dernier jour de chaque mois. Étant donné que l’utilisateur local actuel (user03) n’est pas un administrateur de l’ordinateur distant, la commande utilise le paramètre **/u** pour fournir les informations d’identification du compte d’administrateur de l’utilisateur (Admin01 dans le domaine reskit). Les autorisations du compte d’administrateur seront utilisées pour planifier la tâche et pour exécuter la tâche.
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Étant donné que la commande n’a pas inclus le paramètre **/p** (mot de passe), **schtasks** vous invite à entrer le mot de passe. Il affiche ensuite un message de réussite et, dans ce cas, un avertissement.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
Cet avertissement indique que le domaine distant n’a pas pu authentifier le compte spécifié par le paramètre **/u** . Dans ce cas, le domaine distant n’a pas pu authentifier le compte d’utilisateur, car l’ordinateur local n’est pas membre d’un domaine approuvé par le domaine de l’ordinateur distant. Dans ce cas, la tâche s’affiche dans la liste des tâches planifiées, mais la tâche est en fait vide et ne s’exécute pas.

L’affichage suivant d’une requête détaillée présente le problème avec la tâche. Dans l’affichage, Notez que la valeur de l’heure de la **prochaine exécution** n’est **jamais** et que la valeur de **exécuter en tant qu’utilisateur** n’a **pas pu être extraite de la base de données du planificateur de tâches**.

Si cet ordinateur était membre du même domaine ou d’un domaine approuvé, la tâche aurait été correctement planifiée et aurait été exécutée comme spécifié.
```
HostName: SRV44
TaskName: My App
Next Run Time: Never
Status:
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user03
Schedule: At 3:52 PM on day 31 of every month, start
 starting 12/14/2001
Task To Run: c:\program files\corpapps\myapp.exe
Start In: myapp.exe
Comment: N/A
Scheduled Task State: Disabled
Scheduled Type: Monthly
Start Time: 3:52:00 PM
Start Date: 12/14/2001
End Date: N/A
Days: 31
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NO
V,DEC
Run As User: Could not be retrieved from the task sched
uler database
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Every: Disabled
Repeat: Until: Time: Disabled
Repeat: Until: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle Time: Disabled
Power Management: Disabled
```

#### <a name="remarks"></a>Notes

-   Pour exécuter une commande **/Create** avec les autorisations d’un autre utilisateur, utilisez le paramètre **/u** . Le paramètre **/u** est valide uniquement pour la planification de tâches sur des ordinateurs distants.
-   Pour afficher d’autres exemples de **SCHTASKS/Create** , tapez **SCHTASKS/Create/ ?** à l’invite de commandes.
-   Pour planifier une tâche qui s’exécute avec les autorisations d’un autre utilisateur, utilisez le paramètre **/ru** . Le paramètre **/ru** est valide pour les tâches sur les ordinateurs locaux et distants.
-   Pour utiliser le paramètre **/u** , l’ordinateur local doit se trouver dans le même domaine que l’ordinateur distant ou dans un domaine approuvé par le domaine de l’ordinateur distant. Dans le cas contraire, soit la tâche n’est pas créée, soit la tâche est vide et la tâche ne s’exécute pas.
-   **Schtasks** vous invite toujours à entrer un mot de passe, sauf si vous en fournissez un, même lorsque vous planifiez une tâche sur l’ordinateur local à l’aide du compte d’utilisateur actuel. Il s’agit d’un comportement normal pour **schtasks**.
-   **Schtasks** ne vérifie pas les emplacements des fichiers programme ou les mots de passe des comptes d’utilisateur. Si vous n’entrez pas l’emplacement de fichier correct ou le mot de passe approprié pour le compte d’utilisateur, la tâche est créée, mais elle n’est pas exécutée. En outre, si le mot de passe d’un compte change ou expire et que vous ne modifiez pas le mot de passe enregistré dans la tâche, la tâche ne s’exécute pas.
-   Le compte système ne dispose pas de droits d’ouverture de session interactifs. Les utilisateurs ne voient pas et ne peuvent pas interagir avec les programmes exécutés avec des autorisations système.
-   Chaque tâche exécute un seul programme. Toutefois, vous pouvez créer un fichier de commandes qui démarre plusieurs tâches, puis planifier une tâche qui exécute le fichier de commandes.
-   Vous pouvez tester une tâche dès que vous la créez. Utilisez l’opération d' **exécution** pour tester la tâche, puis vérifiez les erreurs dans le fichier SchedLgU. txt (*systemroot*\SchedLgU.txt).

## <a name="BKMK_change"></a>modification de schtasks

Modifie une ou plusieurs des propriétés suivantes d’une tâche.
-   Le programme exécuté par la tâche ( **/TR**).
-   Compte d’utilisateur sous lequel la tâche s’exécute ( **/ru**).
-   Mot de passe du compte d’utilisateur ( **/RP**).
-   Ajoute la propriété interactif uniquement à la tâche ( **/IT**).

### <a name="syntax"></a>Syntaxe

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Paramètres

|          Terme           |                                                                                                                                                                                                                                                                                                                                     Définition                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /TN \<TaskName >     |                                                                                                                                                                                                                                                                                                               Identifie la tâche à modifier. Entrez le nom de la tâche.                                                                                                                                                                                                                                                                                                               |
|     /s \<Computer >      |                                                                                                                                                                                                                                                                               Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barre oblique inverse). La valeur par défaut est l'ordinateur local.                                                                                                                                                                                                                                                                               |
|  /u [\<Domain > \] @ no__t-2  |                                                                                                                                                                 Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est les autorisations de l’utilisateur actuel de l’ordinateur local. Le compte d’utilisateur spécifié doit être membre du groupe Administrateurs sur l’ordinateur distant. Les paramètres **/u** et **/p** ne sont valides que pour la modification d’une tâche sur un ordinateur distant ( **/s**).                                                                                                                                                                  |
|     /p \<mot de passe >      |                                                                                                                                                                                              Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . Si vous utilisez le paramètre **/u** , mais omettez le paramètre **/p** ou l’argument Password, **schtasks** vous invite à entrer un mot de passe.</br>Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**.                                                                                                                                                                                               |
| /ru {[\<Domain > \] @ no__t-2 |                                                                                                                                                                                                                                                                                                                                       Requise                                                                                                                                                                                                                                                                                                                                       |
|     /RP \<Password >     |                                                                                                                                                                                                                                                 Spécifie un nouveau mot de passe pour le compte d’utilisateur existant ou le compte d’utilisateur spécifié par le paramètre **/ru** . Ce paramètre est ignoré avec utilisé avec le compte système local.                                                                                                                                                                                                                                                  |
|     /TR \<TaskRun >      |                                                                                                                                                                                  Modifie le programme exécuté par la tâche. Entrez le chemin d’accès complet et le nom de fichier d’un fichier exécutable, d’un fichier de script ou d’un fichier de commandes. Si vous omettez le chemin d’accès, **schtasks** suppose que le fichier se trouve dans le répertoire \<systemroot > \System32. Le programme spécifié remplace le programme d’origine exécuté par la tâche.                                                                                                                                                                                  |
|    /St \<Starttime >     |                                                                                                                                                                                                                                                              Spécifie l’heure de début de la tâche, en utilisant le format de 24 heures, HH : mm. Par exemple, la valeur 14:30 équivaut à l’heure de 12 heures de 2:30 PM.                                                                                                                                                                                                                                                               |
|     /RI \<Interval >     |                                                                                                                                                                                                                                                                           Spécifie l’intervalle de répétition pour la tâche planifiée, en minutes. La plage valide est 1-599940 (599940 minutes = 9999 heures).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               Spécifie l’heure de fin de la tâche, en utilisant le format de 24 heures, HH : mm. Par exemple, la valeur 14:30 équivaut à l’heure de 12 heures de 2:30 PM.                                                                                                                                                                                                                                                                |
|     /du \<Duration >     |                                                                                                                                                                                                                                                                                                     Spécifie de fermer la tâche au niveau de la \<EndTime > ou <Duration>, si elle est spécifiée.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Arrête le programme que la tâche exécute à l’heure spécifiée par **/et** ou **/du**. Sans **/k**, **schtasks** ne redémarre pas le programme après avoir atteint l’heure spécifiée par **/et** ou **/du**, mais il n’arrête pas le programme s’il est toujours en cours d’exécution. Ce paramètre est facultatif et valide uniquement avec une MINUTE ou une planification horaire.                                                                                                                                                                   |
|    /SD \<StartDate >     |                                                                                                                                                                                                                                                                                              Spécifie la première date à laquelle la tâche doit être exécutée. Le format de date est le suivant : MM/jj/aaaa.                                                                                                                                                                                                                                                                                               |
|     /Ed @no__t 0EndDate >      |                                                                                                                                                                                                                                                                                                 Spécifie la dernière date à laquelle la tâche doit être exécutée. Le format est MM/jj/aaaa.                                                                                                                                                                                                                                                                                                  |
|         /ENABLE         |                                                                                                                                                                                                                                                                                                                       Spécifie l’activation de la tâche planifiée.                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      Spécifie la désactivation de la tâche planifiée.                                                                                                                                                                                                                                                                                                                       |
|           /IT           | Spécifie d’exécuter la tâche planifiée uniquement lorsque l’utilisateur « exécuter en tant que » (compte d’utilisateur sous lequel la tâche s’exécute) est connecté à l’ordinateur.</br>Ce paramètre n’a aucun effet sur les tâches qui s’exécutent avec des autorisations système ou des tâches pour lesquelles la propriété interactif uniquement est déjà définie. Vous ne pouvez pas utiliser une commande de modification pour supprimer la propriété interactif uniquement d’une tâche.</br>Par défaut, l’utilisateur « exécuter en tant que » est l’utilisateur actuel de l’ordinateur local lorsque la tâche est planifiée ou le compte spécifié par le paramètre **/u** , le cas échéant. Toutefois, si la commande comprend le paramètre **/ru** , l’utilisateur « exécuter en tant que » est le compte spécifié par le paramètre **/ru** . |
|           z            |                                                                                                                                                                                                                                                                                                          Spécifie que la tâche doit être supprimée à la fin de sa planification.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Notes

-   Les paramètres **/TN** et **/s** identifient la tâche. Les paramètres **/TR**, **/ru**et **/RP** spécifient les propriétés de la tâche que vous pouvez modifier.
-   Les paramètres **/ru**et **/RP** spécifient les autorisations d’exécution de la tâche. Les paramètres **/u** et **/p** spécifient les autorisations utilisées pour modifier la tâche.
-   Pour modifier des tâches sur un ordinateur distant, l’utilisateur doit être connecté à l’ordinateur local à l’aide d’un compte membre du groupe Administrateurs sur l’ordinateur distant.
-   Pour exécuter une commande **/change** avec les autorisations d’un autre utilisateur ( **/u**, **/p**), l’ordinateur local doit se trouver dans le même domaine que l’ordinateur distant ou dans un domaine approuvé par le domaine de l’ordinateur distant.
-   Le compte système ne dispose pas de droits d’ouverture de session interactifs. Les utilisateurs ne voient pas et ne peuvent pas interagir avec les programmes exécutés avec des autorisations système.
-   Pour identifier les tâches avec la propriété **/IT** , utilisez une requête détaillée ( **/query/v**). Dans l’affichage des requêtes détaillées d’une tâche avec l’opération **/IT**, le champ **mode d’ouverture de session** a la valeur **interactif uniquement**.

### <a name="examples"></a>Exemples

### <a name="to-change-the-program-that-a-task-runs"></a>Pour modifier le programme exécuté par une tâche

La commande suivante modifie le programme que la tâche de vérification antivirus exécute à partir de VirusCheck. exe vers VirusCheck2. exe. Cette commande utilise le paramètre **/TN** pour identifier la tâche et le paramètre **/TR** pour spécifier le nouveau programme pour la tâche. (Vous ne pouvez pas modifier le nom de la tâche.)
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
En réponse, **schtasks. exe** affiche le message de réussite suivant :
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
À la suite de cette commande, la tâche de vérification antivirus exécute désormais VirusCheck2. exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Pour modifier le mot de passe d’une tâche à distance

La commande suivante modifie le mot de passe du compte d’utilisateur pour la tâche RemindMe sur l’ordinateur distant, Svr01. La commande utilise le paramètre **/TN** pour identifier la tâche et le paramètre **/s** pour spécifier l’ordinateur distant. Elle utilise le paramètre **/RP** pour spécifier le nouveau mot de passe, p@ssWord3.

Cette procédure est requise chaque fois que le mot de passe d’un compte d’utilisateur expire ou change. Si le mot de passe enregistré dans une tâche n’est plus valide, la tâche ne s’exécute pas.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
En réponse, **schtasks. exe** affiche le message de réussite suivant :
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
À la suite de cette commande, la tâche RemindMe s’exécute maintenant sous son compte d’utilisateur d’origine, mais avec un nouveau mot de passe.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Pour modifier le programme et le compte d’utilisateur d’une tâche

La commande suivante modifie le programme exécuté par une tâche et modifie le compte d’utilisateur sous lequel la tâche s’exécute. Fondamentalement, elle utilise une ancienne planification pour une nouvelle tâche. Cette commande modifie la tâche ChkNews, qui démarre Notepad. exe tous les matins à 9:00 A.M., pour démarrer Internet Explorer à la place.

La commande utilise le paramètre **/TN** pour identifier la tâche. Elle utilise le paramètre **/TR** pour modifier le programme exécuté par la tâche et le paramètre **/ru** pour modifier le compte d’utilisateur sous lequel la tâche s’exécute.

Le paramètre **/ru**et **/RP** , qui fournit le mot de passe du compte d’utilisateur, est omis. Vous devez fournir un mot de passe pour le compte, mais vous pouvez utiliser le paramètre **/ru**et **/RP** et taper le mot de passe en texte clair, ou attendre que **schtasks. exe** vous invite à entrer un mot de passe, puis entrer le mot de passe dans du texte masqué.
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
En réponse, **schtasks. exe** demande le mot de passe du compte d’utilisateur. Il masque le texte que vous tapez, donc le mot de passe n’est pas visible.
```
Please enter the password for DomainX\Admin01: 
```
Notez que le paramètre **/TN** identifie la tâche et que les paramètres **/TR** et **/ru** modifient les propriétés de la tâche. Vous ne pouvez pas utiliser un autre paramètre pour identifier la tâche et vous ne pouvez pas modifier le nom de la tâche.

En réponse, **schtasks. exe** affiche le message de réussite suivant :
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
À la suite de cette commande, la tâche ChkNews exécute désormais Internet Explorer avec les autorisations d’un compte d’administrateur.

### <a name="to-change-a-program-to-the-system-account"></a>Pour modifier un programme en compte système

La commande suivante modifie la tâche SecurityScript pour qu’elle s’exécute avec les autorisations du compte système. Elle utilise le paramètre **/ru «»** pour indiquer le compte système.
```
schtasks /change /tn SecurityScript /ru ""
```
En réponse, **schtasks. exe** affiche le message de réussite suivant :
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
Étant donné que les tâches exécutées avec les autorisations du compte système ne requièrent pas de mot de passe, **schtasks. exe** ne demande pas de mot de passe.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Pour exécuter un programme uniquement lorsque je suis connecté

La commande suivante ajoute la propriété interactif uniquement à MyApp, une tâche existante. Cette propriété permet de s’assurer que la tâche s’exécute uniquement lorsque l’utilisateur « exécuter en tant que », autrement dit, le compte d’utilisateur sous lequel la tâche s’exécute, est connecté à l’ordinateur.

La commande utilise le paramètre **/TN** pour identifier la tâche et le paramètre **/IT** pour ajouter la propriété interactif uniquement à la tâche. Étant donné que la tâche s’exécute déjà avec les autorisations de mon compte d’utilisateur, je n’ai pas besoin de modifier le paramètre **/ru** pour la tâche.
```
schtasks /change /tn MyApp /it
```
En réponse, **schtasks. exe** affiche le message de réussite suivant.
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>SCHTASKS-exécuter

Démarre immédiatement une tâche planifiée. L’opération d' **exécution** ignore la planification, mais utilise l’emplacement du fichier programme, le compte d’utilisateur et le mot de passe enregistrés dans la tâche pour exécuter immédiatement la tâche.

### <a name="syntax"></a>Syntaxe

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                                 Définition                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<TaskName >    |                                                                                                                                                       Obligatoire. Identifie la tâche.                                                                                                                                                        |
|    /s \<Computer >     |                                                                                                           Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barre oblique inverse). La valeur par défaut est l'ordinateur local.                                                                                                           |
| /u [\<Domain > \] @ no__t-2 | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local.</br>Le compte d’utilisateur spécifié doit être membre du groupe Administrateurs sur l’ordinateur distant. Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                          Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . Si vous utilisez le paramètre **/u** , mais omettez le paramètre **/p** ou l’argument Password, **schtasks** vous invite à entrer un mot de passe.</br>Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**.                           |
|          /?           |                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                     |

### <a name="remarks"></a>Notes

-   Utilisez cette opération pour tester vos tâches. Si une tâche ne s’exécute pas, recherchez les erreurs dans le journal des transactions du service Planificateur de tâches, \<Systemroot > \SchedLgU.txt.
-   L’exécution d’une tâche n’affecte pas la planification des tâches et ne modifie pas la prochaine exécution planifiée pour la tâche.
-   Pour exécuter une tâche à distance, la tâche doit être planifiée sur l’ordinateur distant. Lorsque vous l’exécutez, la tâche s’exécute uniquement sur l’ordinateur distant. Pour vérifier qu’une tâche est en cours d’exécution sur un ordinateur distant, utilisez le gestionnaire des tâches ou le journal des transactions Planificateur de tâches, \<Systemroot > \SchedLgU.txt.

### <a name="examples"></a>Exemples

### <a name="to-run-a-task-on-the-local-computer"></a>Pour exécuter une tâche sur l’ordinateur local

La commande suivante démarre la tâche « script de sécurité ».
```
schtasks /run /tn "Security Script"
```
En réponse, **schtasks. exe** démarre le script associé à la tâche et affiche le message suivant :
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
Comme l’indique le message, **schtasks** tente de démarrer le programme, mais il ne peut pas être très bien Démarré par le programme.

### <a name="to-run-a-task-on-a-remote-computer"></a>Pour exécuter une tâche sur un ordinateur distant

La commande suivante démarre la tâche de mise à jour sur un ordinateur distant, SVR01 :
```
schtasks /run /tn Update /s Svr01
```
Dans ce cas, **schtasks. exe** affiche le message d’erreur suivant :
```
ERROR: Unable to run the scheduled task "Update".
```
Pour trouver la cause de l’erreur, examinez le journal des transactions des tâches planifiées, C:\Windows\SchedLgU.txt sur Svr01. Dans ce cas, l’entrée suivante apparaît dans le journal :
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
Apparemment, le nom d’utilisateur ou le mot de passe dans la tâche n’est pas valide sur le système. La commande **SCHTASKS/Change** suivante met à jour le nom d’utilisateur et le mot de passe de la tâche de mise à jour sur SVR01 :
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Une fois la commande de **modification** terminée, la commande **exécuter** est répétée. Cette fois-ci, le programme Update. exe démarre et **schtasks. exe** affiche le message suivant :
```
SUCCESS: Attempted to run the scheduled task "Update".
```
Comme l’indique le message, **schtasks** tente de démarrer le programme, mais il ne peut pas être très bien Démarré par le programme.

## <a name="BKMK_end"></a>fin de schtasks

Arrête un programme Démarré par une tâche.

### <a name="syntax"></a>Syntaxe

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                               Définition                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<TaskName >    |                                                                                                                                         Obligatoire. Identifie la tâche qui a démarré le programme.                                                                                                                                         |
|    /s \<Computer >     |                                                                                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant. La valeur par défaut est l'ordinateur local.                                                                                                                        |
| /u [\<Domain > \] @ no__t-2 | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local. Le compte d’utilisateur spécifié doit être membre du groupe Administrateurs sur l’ordinateur distant. Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . Si vous utilisez le paramètre **/u** , mais omettez le paramètre **/p** ou l’argument Password, **schtasks** vous invite à entrer un mot de passe.</br>Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**.                         |
|          /?           |                                                                                                                                                             Affiche l’aide.                                                                                                                                                              |

### <a name="remarks"></a>Notes

**Schtasks. exe** termine uniquement les instances d’un programme démarrées par une tâche planifiée. Pour arrêter d’autres processus, utilisez TaskKill. Pour plus d’informations, consultez [taskkill](taskkill.md).

### <a name="examples"></a>Exemples

### <a name="to-end-a-task-on-a-local-computer"></a>Pour arrêter une tâche sur un ordinateur local

La commande suivante arrête l’instance de Notepad. exe qui a été démarrée par la tâche mon bloc-notes :
```
schtasks /end /tn "My Notepad"
```
En réponse, **schtasks. exe** arrête l’instance de Notepad. exe que la tâche a démarrée et affiche le message de réussite suivant :
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Pour mettre fin à une tâche sur un ordinateur distant

La commande suivante arrête l’instance d’Internet Explorer qui a été démarrée par la tâche InternetOn sur l’ordinateur distant, SVR01 :
```
schtasks /end /tn InternetOn /s Svr01
```
En réponse, **schtasks. exe** arrête l’instance d’Internet Explorer que la tâche a démarrée et affiche le message de réussite suivant :
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>SCHTASKS supprimer

Supprime une tâche planifiée.

### <a name="syntax"></a>Syntaxe

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                                 Définition                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /TN {@no__t 0TaskName >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Supprime le message de confirmation. La tâche est supprimée sans avertissement.                                                                                                                                  |
|    /s \<Computer >     |                                                                                                           Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barre oblique inverse). La valeur par défaut est l'ordinateur local.                                                                                                           |
| /u [\<Domain > \] @ no__t-2 | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local.</br>Le compte d’utilisateur spécifié doit être membre du groupe Administrateurs sur l’ordinateur distant. Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                          Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . Si vous utilisez le paramètre **/u** , mais omettez le paramètre **/p** ou l’argument Password, **schtasks** vous invite à entrer un mot de passe.</br>Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**.                           |
|          /?           |                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                     |

### <a name="remarks"></a>Notes

- L’opération de **suppression** supprime la tâche de la planification. Elle ne supprime pas le programme exécuté par la tâche ou n’interrompt pas un programme en cours d’exécution.
- La commande **delete \\** * supprime toutes les tâches planifiées pour l’ordinateur, pas seulement les tâches planifiées par l’utilisateur actuel.

### <a name="examples"></a>Exemples

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Pour supprimer une tâche de la planification d’un ordinateur distant

La commande suivante supprime la tâche « démarrer la messagerie » de la planification d’un ordinateur distant. Elle utilise le paramètre **/s** pour identifier l’ordinateur distant.
```
schtasks /delete /tn "Start Mail" /s Svr16
```
En réponse, **schtasks. exe** affiche le message de confirmation suivant. Pour supprimer la tâche, appuyez sur o<strong>.</strong> Pour annuler la commande, tapez **n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Pour supprimer toutes les tâches planifiées pour l’ordinateur local

La commande suivante supprime toutes les tâches de la planification de l’ordinateur local, y compris les tâches planifiées par d’autres utilisateurs. Elle utilise le paramètre **/tn \\** * pour représenter toutes les tâches sur l’ordinateur et le paramètre **/f** pour supprimer le message de confirmation.
```
schtasks /delete /tn * /f
```
En réponse, **schtasks. exe** affiche les messages de réussite suivants indiquant que la seule tâche planifiée, SecureScript, est supprimée.

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>requête schtasks

Affiche les tâches planifiées pour s’exécuter sur l’ordinateur.

### <a name="syntax"></a>Syntaxe

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                                 Définition                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       Query        |                                                                                                                        Le nom de l’opération est facultatif. Si vous tapez **schtasks** sans aucun paramètre, une requête est exécutée.                                                                                                                         |
|      /FO {TABLE       |                                                                                                                                                                    TARIFS                                                                                                                                                                     |
|          /NH          |                                                                                                            Omet les en-têtes de colonne de l’affichage du tableau. Ce paramètre est valide avec les formats de sortie **table** et **CSV** .                                                                                                             |
|          /v           |                                                                                                         Ajoute des propriétés avancées des tâches à l’affichage.</br>Les requêtes utilisant **/v** doivent être au format **liste** ou **CSV**.                                                                                                          |
|    /s \<Computer >     |                                                                                                           Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barre oblique inverse). La valeur par défaut est l'ordinateur local.                                                                                                           |
| /u [\<Domain > \] @ no__t-2 | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local.</br>Le compte d’utilisateur spécifié doit être membre du groupe Administrateurs sur l’ordinateur distant. Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . Si vous utilisez **/u**, mais omettez **/p** ou l’argument de mot de passe, **schtasks** vous invite à entrer un mot de passe.</br>Les paramètres **/u** et **/p** ne sont valides que lorsque vous utilisez **/s**.                                         |
|          /?           |                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                     |

### <a name="remarks"></a>Notes

**Schtasks. exe** termine uniquement les instances d’un programme démarrées par une tâche planifiée. Pour arrêter d’autres processus, utilisez TaskKill. Pour plus d’informations, consultez [taskkill](taskkill.md).

### <a name="examples"></a>Exemples

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Pour afficher les tâches planifiées sur l’ordinateur local

Les commandes suivantes affichent toutes les tâches planifiées pour l’ordinateur local. Ces commandes produisent le même résultat et peuvent être utilisées indifféremment.
```
schtasks
schtasks /query
```
En réponse, **schtasks. exe** affiche les tâches dans le format de table par défaut, simple, comme indiqué dans le tableau suivant :
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Pour afficher les propriétés avancées des tâches planifiées

La commande suivante demande un affichage détaillé des tâches sur l’ordinateur local. Elle utilise le paramètre **/v** pour demander un affichage détaillé (détaillé) et le paramètre **/FO list** pour mettre en forme l’affichage en tant que liste pour faciliter la lecture. Vous pouvez utiliser cette commande pour vérifier qu’une tâche que vous avez créée a la périodicité prévue.

**SCHTASKS/Query/FO LIST/v**

En réponse, **schtasks. exe** affiche une liste détaillée des propriétés pour toutes les tâches. L’affichage suivant présente la liste des tâches pour une tâche dont l’exécution est planifiée à 4:00 h 00. le dernier vendredi de chaque mois :
```
HostName: RESKIT01
TaskName: SecureScript
Next Run Time: 4:00:00 AM , 3/30/2001
Status: Not yet run
Logon mode: Interactive/Background
Last Run Time: Never
Last Result: 0
Creator: user01
Schedule: At 4:00 AM on the last Fri of every month, starting 3/24/2001
Task To Run: C:\WINDOWS\system32\notepad.exe
Start In: notepad.exe
Comment: N/A
Scheduled Task State: Enabled
Scheduled Type: Monthly
Modifier: Last FRIDAY
Start Time: 4:00:00 AM
Start Date: 3/24/2001
End Date: N/A
Days: FRIDAY
Months: JAN,FEB,MAR,APR,MAY,JUN,JUL,AUG,SEP,OCT,NOV,DEC
Run As User: RESKIT\user01
Delete Task If Not Rescheduled: Enabled
Stop Task If Runs X Hours and X Mins: 72:0
Repeat: Until Time: Disabled
Repeat: Duration: Disabled
Repeat: Stop If Still Running: Disabled
Idle: Start Time(For IDLE Scheduled Type): Disabled
Idle: Only Start If Idle for X Minutes: Disabled
Idle: If Not Idle Retry For X Minutes: Disabled
Idle: Stop Task If Idle State End: Disabled
Power Mgmt: No Start On Batteries: Disabled
Power Mgmt: Stop On Battery Mode: Disabled
```

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Pour enregistrer les tâches planifiées pour un ordinateur distant

La commande suivante demande une liste de tâches planifiées pour un ordinateur distant et ajoute les tâches à un fichier journal séparé par des virgules sur l’ordinateur local. Vous pouvez utiliser ce format de commande pour collecter et suivre les tâches planifiées pour plusieurs ordinateurs.

La commande utilise le paramètre **/s** pour identifier l’ordinateur distant, Reskit16, le paramètre **/FO** pour spécifier le format et le paramètre **/NH** pour supprimer les en-têtes de colonne. Le symbole d’ajout **>>** redirige la sortie vers le journal des tâches, P0102. csv, sur l’ordinateur local, Svr01. Étant donné que la commande s’exécute sur l’ordinateur distant, le chemin d’accès de l’ordinateur local doit être complet.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
En réponse, **schtasks. exe** ajoute les tâches planifiées pour l’ordinateur Reskit16 au fichier P0102. csv sur l’ordinateur local, Svr01.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
