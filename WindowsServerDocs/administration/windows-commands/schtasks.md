---
title: schtasks
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 76ae264ded3cd0611c06b56c1074a2d916513da4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441617"
---
# <a name="schtasks"></a>schtasks



Planifie l’exécution périodique ou à une heure spécifique de commandes et programmes. Ajoute et supprime les tâches de la planification, démarre et arrête des tâches à la demande, affiche et modifie les tâches planifiées.

Pour afficher la syntaxe de commande, cliquez sur une des commandes suivantes :
-   [SCHTASKS créer](#BKMK_create)
-   [modification de Schtasks](#BKMK_change)
-   [schtasks run](#BKMK_run)
-   [schtasks end](#BKMK_end)
-   [schtasks delete](#BKMK_delete)
-   [schtasks query](#BKMK_query)

## <a name="remarks"></a>Notes

- **SchTasks.exe** effectue les mêmes opérations que **tâches planifiées** dans **le panneau de configuration**. Vous pouvez utiliser ces outils ensemble et de manière interchangeable.
- **SCHTASKS** remplace **At.exe**, un outil inclus dans les versions précédentes de Windows. Bien que **At.exe** est toujours incluse dans la famille Windows Server 2003, **schtasks** est la tâche de ligne de commande recommandée, outil de planification.
- Les paramètres dans un **schtasks** commande peut apparaître dans n’importe quel ordre. Tapant **schtasks** sans aucun paramètre effectue une requête.
- Autorisations pour **schtasks**  
  -   Vous devez être autorisé à exécuter la commande. N’importe quel utilisateur peut planifier une tâche sur l’ordinateur local, et ils peuvent afficher et modifier les tâches qu’ils ont planifiées. Les membres du groupe administrateurs peuvent planifier, afficher et modifier toutes les tâches sur l’ordinateur local.
  -   Pour planifier, afficher ou modifier une tâche sur un ordinateur distant, vous devez être membre du groupe Administrateurs sur l’ordinateur distant, ou vous devez utiliser le **/u** paramètre pour fournir les informations d’identification d’un administrateur de l’ordinateur distant.
  -   Vous pouvez utiliser la **/u** paramètre dans un **/ créer** ou **/modifier** opération uniquement lorsque les ordinateurs locaux et distants sont dans le même domaine ou de l’ordinateur local est dans un domaine qui approbations de domaine de l’ordinateur distant. Sinon, l’ordinateur distant ne peut pas authentifier le compte d’utilisateur spécifié et il ne peut pas vérifier que le compte est membre du groupe Administrateurs.
  -   La tâche doit avoir l’autorisation d’exécuter. Les autorisations requises varient selon la tâche. Par défaut, les tâches s’exécutent avec les autorisations de l’utilisateur actuel de l’ordinateur local, ou avec les autorisations de l’utilisateur spécifié par le **/u** paramètre, s’il est utilisé. Pour exécuter une tâche avec les autorisations d’un compte d’utilisateur différent ou avec les autorisations système, utilisez le **/ru** paramètre.
- Pour vérifier qu’une tâche planifiée a été exécutée ou pour déterminer pourquoi une tâche planifiée ne n’avez pas exécuté, consultez le journal des transactions service Planificateur de tâches, *SystemRoot*\SchedLgU.txt. Cette enregistrements du journal des tentatives d’exécution initiées par tous les outils qui utilisent le service, y compris **tâches planifiées** et **SchTasks.exe**.
- Dans de rares occasions, les fichiers de la tâche endommagées. Tâches corrompues ne s’exécutent pas. Lorsque vous essayez d’effectuer une opération sur des tâches corrompues, **SchTasks.exe** affiche le message d’erreur suivant :  
  ```
  ERROR: The data is invalid.
  ```  
  Vous ne pouvez pas récupérer les tâches corrompues. Pour restaurer les fonctionnalités du système de planification des tâches, utilisez **SchTasks.exe** ou **tâches planifiées** pour supprimer les tâches dans le système et les replanifier.

## <a name="BKMK_create"></a>SCHTASKS créer

Planifie une tâche.

**SCHTASKS** utilise différentes combinaisons de paramètres pour chaque type de planification. Pour afficher la syntaxe combinée pour la création de tâches ou pour afficher la syntaxe pour la création d’une tâche avec un type de planification particulière, cliquez sur une des options suivantes.
-   [Descriptions de syntaxe et les paramètres combinées](#BKMK_syntax)
-   [Pour planifier une tâche qui s’exécute toutes les N minutes](#BKMK_minutes)
-   [Pour planifier une tâche qui s’exécute toutes les N heures](#BKMK_hours)
-   [Pour planifier une tâche qui s’exécute tous les N jours](#BKMK_days)
-   [Pour planifier une tâche qui s’exécute toutes les N semaines](#BKMK_weeks)
-   [Pour planifier une tâche qui s’exécute tous les mois N](#BKMK_months)
-   [Pour planifier une tâche qui s’exécute sur un jour spécifique de la semaine](#BKMK_spec_day)
-   [Pour planifier une tâche qui s’exécute sur une semaine spécifique au cours du mois](#BKMK_spec_week)
-   [Pour planifier une tâche qui s’exécute sur une date spécifique chaque mois](#BKMK_spec_date)
-   [Pour planifier une tâche qui s’exécute sur le dernier jour du mois](#BKMK_last_day)
-   [Pour planifier une tâche qui s’exécute une fois](#BKMK_once)
-   [Pour planifier une tâche qui s’exécute chaque fois que le système démarre](#BKMK_startup)
-   [Pour planifier une tâche qui s’exécute lorsqu’un utilisateur ouvre une session](#BKMK_logon)
-   [Pour planifier une tâche qui s’exécute lorsque le système est inactif](#BKMK_idle)
-   [Pour planifier une tâche qui s’exécute maintenant](#BKMK_now)
-   [Pour planifier une tâche qui s’exécute avec des autorisations différentes](#BKMK_diff_perms)
-   [Pour planifier une tâche qui s’exécute avec les autorisations système](#BKMK_sys_perms)
-   [Pour planifier une tâche qui exécute plusieurs programmes](#BKMK_multi_progs)
-   [Pour planifier une tâche qui s’exécute sur un ordinateur distant](#BKMK_remote)

### <a name="BKMK_syntax"></a>Descriptions de syntaxe et les paramètres combinées

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /sc <ScheduleType> /tn <TaskName> /tr <TaskRun> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/mo <Modifier>] [/d <Day>[,<Day>...] | *] [/m <Month>[,<Month>...]] [/i <IdleTime>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/it] [/z] [/f]
```

#### <a name="parameters"></a>Paramètres

##### <a name="sc-scheduletype"></a>/SC \<ScheduleType >

Spécifie le type de planification. Les valeurs valides sont : MINUTE, horaire, quotidienne, hebdomadaire, mensuelle, une fois, ONSTART, ONLOGON, ONIDLE.

|Type de planification|Description|
|-------------|-----------|
|MINUTES, HORAIRE, QUOTIDIENNE, HEBDOMADAIRE, MENSUELLE|Spécifie l’unité de temps pour la planification.|
|FOIS|La tâche s’exécute une fois à une date et heure spécifiées.|
|ONSTART|La tâche s’exécute chaque fois que le système démarre. Vous pouvez spécifier une date de début, ou exécuter la tâche du prochain que démarrage du système.|
|ONLOGON|La tâche s’exécute chaque fois qu’un utilisateur (n’importe quel utilisateur) se connecte. Vous pouvez spécifier une date, ou exécuter la tâche à la prochaine fois que l’utilisateur ouvre une session.|
|ONIDLE|La tâche s’exécute chaque fois que le système est inactif pendant une période spécifiée. Vous pouvez spécifier une date, ou exécuter la tâche de la prochaine fois que le système est inactif.|

##### <a name="tn-taskname"></a>/TN \<Nom_tâche >

Spécifie un nom pour la tâche. Chaque tâche sur le système doit avoir un nom unique. Le nom doit être conforme aux règles des noms de fichiers et ne doit pas dépasser 238 caractères. Utilisez des guillemets pour délimiter les noms incluant des espaces.

##### <a name="tr-taskrun"></a>/TR \<Exécution_tâche >

Spécifie le programme ou une commande qui s’exécute la tâche. Tapez le chemin d’accès et le nom qualifié complet d’un fichier exécutable, un fichier de script ou un fichier de commandes. Le nom de chemin d’accès ne doit pas dépasser 262 caractères. Si vous omettez le chemin d’accès, **schtasks** suppose que le fichier se trouve dans le *SystemRoot*répertoire \System32.

##### <a name="s-computer"></a>/s \<ordinateur >

Planifie une tâche sur l’ordinateur distant spécifié. Tapez le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barres obliques inverses). La valeur par défaut est l'ordinateur local. Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**.

##### <a name="u-domainuser"></a>/u [\<domaine >\]<User>

Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est les autorisations de l’utilisateur actuel de l’ordinateur local. Le **/u** et **/p** paramètres sont valides uniquement pour la planification d’une tâche sur un ordinateur distant ( **/s**).

Les autorisations du compte spécifié sont utilisées pour planifier la tâche et pour exécuter la tâche. Pour exécuter la tâche avec les autorisations d’un autre utilisateur, utilisez la **/ru** paramètre.

Le compte d’utilisateur doit être membre du groupe Administrateurs sur l’ordinateur distant. En outre, l’ordinateur local doit être dans le même domaine que l’ordinateur distant, ou il doit être dans un domaine qui est approuvé par le domaine de l’ordinateur distant.

##### <a name="p-password"></a>/p \<mot de passe >

Fournit le mot de passe pour le compte d’utilisateur spécifié dans le **/u** paramètre. Si vous utilisez le **/u** paramètre, mais omettez les **/p** paramètre ou l’argument de mot de passe, **schtasks** vous invitent à entrer un mot de passe et masque le texte que vous tapez.

Le **/u** et **/p** paramètres sont valides uniquement pour la planification d’une tâche sur un ordinateur distant ( **/s**).

##### <a name="ru-domainuser--system"></a>/RU {[\<domaine >\] <User> | Système}

Exécute la tâche avec les autorisations du compte d’utilisateur spécifié. Par défaut, la tâche s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local, ou avec l’autorisation de l’utilisateur spécifié par le **/u** paramètre, s’il est utilisé. Le **/ru** paramètre n’est valide lors de la planification des tâches sur les ordinateurs locaux ou distants.


|       Value        |                                                    Description                                                    |
|--------------------|-------------------------------------------------------------------------------------------------------------------|
| [\<Domain>\]<User> |                                       Spécifie un autre compte d’utilisateur.                                        |
|    Système ou « »    | Spécifie le compte système local, un compte à privilèges élevés utilisé par le système d’exploitation et les services système. |

##### <a name="rp-password"></a>/RP \<mot de passe >

Fournit le mot de passe du compte d’utilisateur qui est spécifié dans le **/ru** paramètre. Si vous omettez ce paramètre lorsque vous spécifiez un compte d’utilisateur, **SchTasks.exe** vous invitent à entrer le mot de passe et masque le texte que vous tapez.

N’utilisez pas le **/rp** paramètre pour les tâches exécuter avec les informations d’identification du compte système ( **/ru système**). Le compte système ne dispose pas d’un mot de passe et **SchTasks.exe** ne demande pas.

##### <a name="mo-modifier"></a>/mois \<modificateur >

Spécifie la fréquence à laquelle la tâche s’exécute au sein de son type de planification. Ce paramètre est valide, mais facultatif, pendant une MINUTE, horaire, quotidienne, hebdomadaire et mensuelle planifier. La valeur par défaut est 1.

|Type de planification|Valeurs de modificateur|Description|
|-------------|---------------|-----------|
|MINUTE|1 - 1439|La tâche s’exécute chaque \<N > minutes.|
|TOUTES LES HEURES|1 - 23|La tâche s’exécute chaque \<N > heures.|
|QUOTIDIENNE|1 - 365|La tâche s’exécute chaque \<N > jours.|
|HEBDOMADAIRE|1 - 52|La tâche s’exécute chaque \<N > semaines.|
|FOIS|Aucun modificateur.|La tâche s’exécute une fois.|
|ONSTART|Aucun modificateur.|La tâche s’exécute au démarrage.|
|ONLOGON|Aucun modificateur.|La tâche s’exécute lorsque l’utilisateur spécifié par le **/u** paramètre ouvre une session.|
|ONIDLE|Aucun modificateur.|La tâche s’exécute après que le système est inactif pendant le nombre de minutes spécifié par le **/i** paramètre, qui est requis pour une utilisation avec ONIDLE.|
|MENSUEL|1 - 12|La tâche s’exécute chaque \<N > mois.|
|MENSUEL|LASTDAY|La tâche s’exécute sur le dernier jour du mois.|
|MENSUEL|TOUT D’ABORD, DEUXIÈME, TROISIÈME, QUATRIÈME, DERNIÈRE|Utiliser avec le **/d**\<jour > paramètre pour exécuter une tâche sur une semaine particulière et le jour. Par exemple, sur le troisième mercredi du mois.|

##### <a name="d-dayday--"></a>/d jour [,... jours] | *

Spécifie un jour ou les jours de la semaine ou un (ou plusieurs jours) d’un mois. Valide uniquement avec une planification hebdomadaire ou mensuelle.


| Type de planification |              Modificateur              |     Les valeurs de jour (/ d)      |                                                                                                 Description                                                                                                 |
|---------------|------------------------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    HEBDOMADAIRE     |               1 - 52               | MON - SUN [, LUN - DIM...] |                                                                                                     \*                                                                                                      |
|    MENSUEL    | TOUT D’ABORD, DEUXIÈME, TROISIÈME, QUATRIÈME, DERNIÈRE |        LUN - DIM         |                                                                                   Requis pour planifier une semaine spécifique.                                                                                    |
|    MENSUEL    |          None ou {1-12}          |          1 - 31          | Facultatif et valide uniquement avec aucun modificateur ( **/mois**) paramètre (une planification date spécifique) ou lorsque le **/mois** est 1-12 (un « chaque \<N > mois « planification). La valeur par défaut est le jour 1 (le premier jour du mois). |

##### <a name="m-monthmonth"></a>/m mois [,... mois]

Spécifie un ou plusieurs mois de l’année au cours de laquelle la tâche planifiée doit s’exécuter. Les valeurs valides sont JAN - DEC et * (tous les mois). Le **/m** paramètre est valide uniquement avec une planification mensuelle. Il est requis lorsque le modificateur LASTDAY est utilisé. Sinon, il est facultatif et la valeur par défaut est * (tous les mois).

##### <a name="i-idletime"></a>/i \<IdleTime>

Spécifie le nombre de minutes l’ordinateur est inactif avant le démarrage de la tâche. Une valeur valide est un nombre entier compris entre 1 et 999. Ce paramètre est valide uniquement avec une planification ONIDLE, et il est requis.

##### <a name="st-starttime"></a>/st \<StartTime >

Spécifie l’heure de démarrage de la tâche (chaque fois qu’il démarre) dans \<hh : mm > 24 heures au format. La valeur par défaut est l’heure actuelle sur l’ordinateur local. Le **/st** paramètre est valide avec minutes, horaire, quotidienne, hebdomadaire, mensuelle et planifie une seule fois. Il est requis pour une planification d’une seule fois.

##### <a name="ri-interval"></a>/ RI \<intervalle >

Spécifie l’intervalle de répétition en minutes. Cela n’est pas applicable pour les types de planification : MINUTE, toutes les heures, ONSTART, ONLOGON et ONIDLE. Plage valide est de 1 à la 599 (REACTION à 940 (minutes (599 940 minutes = 9999 heures). Si /ET ou /DU est spécifié, l’intervalle de répétition par défaut est de 10 minutes.

##### <a name="et-endtime"></a>/et \<EndTime >

Spécifie l’heure du jour qui se termine par une planification de tâche minute ou horaire \<hh : mm > 24 heures au format. Après l’heure de fin spécifiée, **schtasks** ne redémarre pas la tâche jusqu'à l’heure de début se répète. Par défaut, les planifications de tâches n’ont pas fin. Ce paramètre est facultatif et valide uniquement avec une planification toutes les heures ou minutes.

Pour obtenir un exemple, consultez :
-   « Pour planifier une tâche qui s’exécute toutes les 100 minutes pendant les heures non ouvrées » le **pour planifier une tâche qui s’exécute chaque** \<N > **minutes** section.

##### <a name="du-duration"></a>/du \<Duration>

Spécifie une longueur maximale de temps pour une minute ou la planification toutes les heures dans \<HHHH > 24 heures au format. Après la durée spécifiée soit écoulée, **schtasks** ne redémarre pas la tâche jusqu'à l’heure de début se répète. Par défaut, les calendriers des tâches n’ont aucun durée maximale. Ce paramètre est facultatif et valide uniquement avec une planification toutes les heures ou minutes.

Pour obtenir un exemple, consultez :
-   « Pour planifier une tâche qui s’exécute toutes les 3 heures pendant 10 heures » le **pour planifier une tâche qui s’exécute chaque** \<N > **heures** section.

##### <a name="k"></a>/k

Arrête le programme exécuté par la tâche à l’heure indiquée par **/et** ou **/du**. Sans **/k**, **schtasks** ne démarre pas le programme à nouveau après avoir atteint l’heure spécifiée par **/et** ou **/du**, mais il ne s’arrête pas le programme si elle est en cours d’exécution. Ce paramètre est facultatif et valide uniquement avec une planification toutes les heures ou minutes.

Pour obtenir un exemple, consultez :
-   « Pour planifier une tâche qui s’exécute toutes les 100 minutes pendant les heures non ouvrées » le **pour planifier une tâche qui s’exécute chaque** \<N > **minutes** section.

##### <a name="sd-startdate"></a>/sd \<StartDate>

Spécifie la date à laquelle la planification de la tâche commence. La valeur par défaut est la date actuelle sur l’ordinateur local. Le **/sd** paramètre n’est valide et facultatif pour tous les planifier des types.

Le format de *StartDate* varie selon les paramètres régionaux sélectionné pour l’ordinateur local dans **Options régionales et linguistiques** dans **le panneau de configuration**. Un seul format est valide pour chacun des paramètres régionaux.

Les formats de date valides sont répertoriées dans le tableau suivant. Utilisez le format le plus proche du format sélectionné pour **date courte** dans **Options régionales et linguistiques** dans **le panneau de configuration** sur l’ordinateur local.


|       Value       |                                        Description                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Utiliser des formats de mois en premier, tel que **anglais (États-Unis)** et **Espagnol (Panama)** . |
| \<DD>/<MM>/<YYYY> |       Utiliser des formats de jour en premier, tel que **bulgare** et **néerlandais (pays-bas)** .        |
| \<AAAA &GT; /<MM>/<DD> |          Utiliser des formats de l’année en premier, tel que **suédois** et **Français (Canada)** .          |

/ed \<EndDate>

Spécifie la date de fin de la planification. Ce paramètre est facultatif. Il n’est pas valide dans une planification d’une seule fois, ONSTART, ONLOGON ou ONIDLE. Par défaut, planifications n’ont aucune date de fin.

Le format de *EndDate* varie selon les paramètres régionaux sélectionné pour l’ordinateur local dans **Options régionales et linguistiques** dans **le panneau de configuration**. Un seul format est valide pour chacun des paramètres régionaux.

Les formats de date valides sont répertoriées dans le tableau suivant. Utilisez le format le plus proche du format sélectionné pour **date courte** dans **Options régionales et linguistiques** dans **le panneau de configuration** sur l’ordinateur local.


|       Value       |                                        Description                                         |
|-------------------|--------------------------------------------------------------------------------------------|
| \<MM>/<DD>/<YYYY> | Utiliser des formats de mois en premier, tel que **anglais (États-Unis)** et **Espagnol (Panama)** . |
| \<DD>/<MM>/<YYYY> |       Utiliser des formats de jour en premier, tel que **bulgare** et **néerlandais (pays-bas)** .        |
| \<AAAA &GT; /<MM>/<DD> |          Utiliser des formats de l’année en premier, tel que **suédois** et **Français (Canada)** .          |

##### <a name="it"></a>/it

Indique qu’il faut exécuter la tâche uniquement lorsque l’utilisateur « exécuter en tant que » (le compte d’utilisateur sous lequel la tâche s’exécute) est connecté à l’ordinateur. Ce paramètre n’a aucun effet sur les tâches qui s’exécutent avec les autorisations système.

Par défaut, l’utilisateur « exécuter en tant que » est l’utilisateur actuel de l’ordinateur local lorsque la tâche est planifiée ou le compte spécifié par le **/u** paramètre, le cas échéant. Toutefois, si la commande inclut le **/ru** paramètre, puis l’utilisateur « exécuter en tant que » est le compte spécifié par le **/ru** paramètre.

Pour obtenir des exemples, consultez :
-   « Pour planifier une tâche qui s’exécute tous les 70 jours si je suis connecté » dans le **pour planifier une tâche qui s’exécute chaque** *N* **jours** section.
-   « Pour exécuter une tâche uniquement lorsqu’un utilisateur particulier est connecté » dans le **pour planifier une tâche qui s’exécute avec des autorisations différentes** section.

##### <a name="z"></a>/z

Spécifie pour supprimer la tâche à l’achèvement de sa planification.

##### <a name="f"></a>/f

Spécifie pour créer la tâche et supprimer des avertissements si la tâche spécifiée existe déjà.

##### <a name=""></a>/?

Affiche l'aide à l'invite de commandes.

### <a name="BKMK_minutes"></a>Pour planifier une tâche qui s’exécute toutes les N minutes

#### <a name="minute-schedule-syntax"></a>Syntaxe de la planification en minutes

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc minute [/mo {1 - 1439}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification en minutes, le **/sc minute** paramètre est obligatoire. Le **/mois** paramètre de (modificateur) est facultatif et spécifie le nombre de minutes entre chaque exécution de la tâche. La valeur par défaut **/mois** est 1 (toutes les minutes). Le **/et** (heure de fin) et **/du** les paramètres de (durée) sont facultatifs et peut être utilisées avec ou sans le **/k** (fin de tâche) paramètre.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-20-minutes"></a>Pour planifier une tâche qui s’exécute toutes les 20 minutes

La commande suivante permet de planifier un script de sécurité, Sec.vbs, pour exécuter toutes les 20 minutes. La commande utilise le **/sc** paramètre pour spécifier une planification en minutes et le **/mois** paramètre pour spécifier un intervalle de 20 minutes.

Étant donné que la commande n’inclut pas une date de début ou une heure, la tâche démarre 20 minutes une fois la commande terminée et s’exécute toutes les 20 minutes par la suite, chaque fois que le système est en cours d’exécution. Notez que le fichier de source de script de sécurité se trouve sur un ordinateur distant, mais que la tâche est planifiée et s’exécute sur l’ordinateur local.
```
schtasks /create /sc minute /mo 20 /tn "Security Script" /tr \\central\data\scripts\sec.vbs
```

#### <a name="to-schedule-a-task-that-runs-every-100-minutes-during-non-business-hours"></a>Pour planifier une tâche qui s’exécute toutes les 100 minutes pendant les heures creuses

La commande suivante planifie un script de sécurité, Sec.vbs, à exécuter sur l’ordinateur local toutes les 100 minutes entre 17 h 00 et 7:59 du matin. chaque jour. La commande utilise le **/sc** paramètre pour spécifier une planification en minutes et le **/mois** paramètre pour spécifier un intervalle de 100 minutes. Il utilise le **/st** et **/et** paramètres pour spécifier l’heure de début et l’heure de fin de la planification chaque jour de. Il utilise également le **/k** paramètre pour interrompre le script si elle est en cours d’exécution à 7 h 59 du matin Sans **/k**, **schtasks** ne démarrait pas le script après 7 h 59, mais si l’instance a démarré à 6 h 20. était en cours d’exécution, il ne l’arrête pas.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc minute /mo 100 /st 17:00 /et 08:00 /k
```

### <a name="BKMK_hours"></a>Pour planifier une tâche qui s’exécute toutes les N heures

#### <a name="hourly-schedule-syntax"></a>Syntaxe de la planification horaire

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc hourly [/mo {1 - 23}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [{/et <HH:MM> | /du <HHHH:MM>} [/k]] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification horaire, le **/sc toutes les heures** paramètre est obligatoire. Le **/mois** paramètre de (modificateur) est facultatif et spécifie le nombre d’heures entre chaque exécution de la tâche. La valeur par défaut **/mois** est 1 (toutes les heures). Le **/k** (fin de tâche) paramètre est facultatif et peut être utilisé avec **/et** (fin à l’heure spécifiée) ou **/du** (fin après l’intervalle spécifié).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-five-hours"></a>Pour planifier une tâche qui s’exécute toutes les cinq heures

La commande suivante planifie le programme MyApp pour exécuter toutes les cinq heures à compter sur le premier jour de mars 2002. Il utilise le **/mois** paramètre pour spécifier l’intervalle et le **/sd** paramètre pour spécifier la date de début. Étant donné que la commande ne spécifie pas une heure de début, l’heure actuelle est utilisée en tant que l’heure de début.

Étant donné que l’ordinateur local est configuré pour utiliser le **anglais (Zimbabwe)** option **Options régionales et linguistiques** dans **le panneau de configuration**, le format de la date de début est MM/jj/aaaa (03/01/2002).
```
schtasks /create /sc hourly /mo 5 /sd 03/01/2002 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-hour-at-five-minutes-past-the-hour"></a>Pour planifier une tâche qui exécute toutes les heures à cinq minutes après l’heure

La commande suivante planifie le programme MyApp pour exécuter toutes les heures commençant à cinq minutes après minuit. Étant donné que le **/mois** paramètre est omis, la commande utilise la valeur par défaut pour la planification horaire, c'est-à-dire toutes les heures (1). Si cette commande est exécutée après 12 h 05, le programme ne s’exécute pas jusqu’au lendemain.
```
schtasks /create /sc hourly /st 00:05 /tn "My App" /tr c:\apps\myapp.exe
```

#### <a name="to-schedule-a-task-that-runs-every-3-hours-for-10-hours"></a>Pour planifier une tâche qui s’exécute toutes les 3 heures pendant 10 heures

La commande suivante planifie le programme MyApp pour exécuter toutes les 3 heures pour 10 heures.

La commande utilise le **/sc** paramètre pour spécifier une planification horaire et le **/mois** paramètre pour spécifier l’intervalle de 3 heures. Il utilise le **/st** paramètre pour démarrer la planification à minuit et le **/du** paramètre pour terminer les récurrences après 10 heures. Étant donné que le programme s’exécute pendant quelques minutes, le **/k** paramètre, qui arrête le programme si elle est en cours d’exécution lors de l’expiration de la durée, n’est pas nécessaire.
```
schtasks /create /tn "My App" /tr myapp.exe /sc hourly /mo 3 /st 00:00 /du 0010:00
```
Dans cet exemple, la tâche s’exécute à 00:00, 3 h 00, 6 h 00 et 9 h 00 Étant donné que la durée est de 10 heures, la tâche n’est pas exécutée à nouveau à 12 h 00 Au lieu de cela, il commence à nouveau à 12 h 00 le jour suivant.

### <a name="BKMK_days"></a>Pour planifier une tâche qui s’exécute tous les N jours

#### <a name="daily-schedule-syntax"></a>Syntaxe de la planification quotidienne

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc daily [/mo {1 - 365}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification quotidienne, le **/sc quotidiennement** paramètre est obligatoire. Le **/mois** paramètre de (modificateur) est facultatif et spécifie le nombre de jours entre chaque exécution de la tâche. La valeur par défaut **/mois** est 1 (tous les jours).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-day"></a>Pour planifier une tâche qui s’exécute chaque jour

L’exemple suivant planifie le programme MyApp pour exécuter une fois par jour, tous les jours à 8 h 00 jusqu’au 31 décembre 2002. Car elle omet le **/mois** paramètre, l’intervalle par défaut de 1 est utilisé pour exécuter la commande tous les jours.

Dans cet exemple, car le système de l’ordinateur local est défini sur le **anglais (Royaume-Uni)** option **Options régionales et linguistiques** dans **le panneau de configuration**, le format pour la date de fin est jj/MM/AAAA (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /st 08:00 /ed 31/12/2002
```

#### <a name="to-schedule-a-task-that-runs-every-12-days"></a>Pour planifier une tâche qui s’exécute tous les 12 jours

L’exemple suivant planifie le programme MyApp pour exécuter tous les 12 jours à 13 h 00 (13:00) à compter du 31 décembre 2002. La commande utilise le **/mois** paramètre pour spécifier un intervalle de deux (2) jours et la **/sd** et **/st** paramètres pour spécifier la date et l’heure.

Dans cet exemple, étant donné que le système est configuré pour le **anglais (Zimbabwe)** option **Options régionales et linguistiques** dans **le panneau de configuration**, le format de la date de fin est MM/JJ / AAAA (31/12/2002)
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc daily /mo 12 /sd 12/31/2002 /st 13:00
```

#### <a name="to-schedule-a-task-that-runs-every-70-days-if-i-am-logged-on"></a>Pour planifier une tâche qui s’exécute tous les 70 jours si je suis connecté

La commande suivante permet de planifier un script de sécurité, Sec.vbs, pour exécuter tous les 70 jours. La commande utilise le **/mois** paramètre pour spécifier un intervalle de 70 jours. Il utilise également le **/it** paramètre pour spécifier que la tâche s’exécute uniquement lorsque l’utilisateur sous le compte duquel la tâche s’exécute est connecté à l’ordinateur. Étant donné que la tâche s’exécutera avec les autorisations de mon compte d’utilisateur, la tâche sera exécutée uniquement lorsque je suis connecté.
```
schtasks /create /tn "Security Script" /tr sec.vbs /sc daily /mo 70 /it
```

> [!NOTE]
> Pour identifier les tâches avec l’interactif uniquement ( **/it**) propriété, utilisez une requête détaillée **(/ interroger /v**). Dans l’affichage d’une requête détaillée d’une tâche avec **/it**, le **le Mode d’ouverture de session** champ a la valeur **Interactive uniquement**.

### <a name="BKMK_weeks"></a>Pour planifier une tâche qui s’exécute toutes les N semaines

#### <a name="weekly-schedule-syntax"></a>Syntaxe de la planification hebdomadaire

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/mo {1 - 52}] [/d {<MON - SUN>[,MON - SUN...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans une planification hebdomadaire, le **/sc hebdomadaire** paramètre est obligatoire. Le **/mois** paramètre de (modificateur) est facultatif et spécifie le nombre de semaines entre chaque exécution de la tâche. La valeur par défaut **/mois** est 1 (chaque semaine).

Les planifications hebdomadaires ont également facultatif **/d** paramètre pour planifier la tâche à exécuter sur les jours spécifiés de la semaine, ou sur tous les jours ( *). La valeur par défaut est MON (lundi). Tous les jours (* ) option est équivalente à la planification d’une tâche quotidienne.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-six-weeks"></a>Pour planifier une tâche qui s’exécute toutes les six semaines

La commande suivante planifie l’exécution du programme MonApp sur un ordinateur distant toutes les six semaines. La commande utilise le **/mois** paramètre pour spécifier l’intervalle. Étant donné que la commande omet le **/d** paramètre, la tâche s’exécute lundi.

Cette commande utilise également le **/s** paramètre pour spécifier l’ordinateur distant et le **/u** paramètre pour exécuter la commande avec les autorisations du compte d’administrateur de l’utilisateur. Étant donné que le **/p** paramètre est omis, **SchTasks.exe** invite l’utilisateur pour le mot de passe du compte administrateur.

En outre, étant donné que la commande est exécutée à distance, tous les chemins d’accès dans la commande, y compris le chemin d’accès à MyApp.exe, reportez-vous aux chemins d’accès sur l’ordinateur distant.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 6 /s Server16 /u Admin01
```

#### <a name="to-schedule-a-task-that-runs-every-other-week-on-friday"></a>Pour planifier une tâche qui s’exécute vendredi chaque semaine

La commande suivante permet de planifier une tâche à exécuter tous les vendredis. Il utilise le **/mois** paramètre pour spécifier l’intervalle de deux semaines et la **/d** paramètre pour spécifier le jour de la semaine. Pour planifier une tâche qui s’exécute tous les vendredis, omettez la **/mois** paramètre ou la valeur 1.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 2 /d FRI
```

### <a name="BKMK_months"></a>Pour planifier une tâche qui s’exécute tous les mois N

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly [/mo {1 - 12}] [/d {1 - 31}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans ce type de planification, le **/sc tous les mois** paramètre est obligatoire. Le **/mois** paramètre (modificateur), qui spécifie le nombre de mois entre chaque exécution de la tâche, est facultatif et la valeur par défaut est 1 (chaque mois). Ce type de planification a également facultatif **/d** paramètre pour planifier la tâche à exécuter sur une date spécifiée du mois. La valeur par défaut est 1 (le premier jour du mois).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-on-the-first-day-of-every-month"></a>Pour planifier une tâche qui s’exécute sur le premier jour de chaque mois

La commande suivante planifie le programme MyApp sur le premier jour de chaque mois. Étant donné que la valeur 1 est la valeur par défaut pour les deux le **/mois** les paramètre (modificateur) et le **/d** paramètre (jour), ces paramètres sont omis de la commande.
```
schtasks /create /tn "My App" /tr myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-that-runs-every-three-months"></a>Pour planifier une tâche qui s’exécute tous les trois mois

La commande suivante planifie le programme MyApp pour exécuter tous les trois mois. Il utilise le **/mois** paramètre pour spécifier l’intervalle.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 3
```

#### <a name="to-schedule-a-task-that-runs-at-midnight-on-the-21st-day-of-every-other-month"></a>Pour planifier une tâche qui s’exécute à minuit le jour jusqu’au 21 du mois

La commande suivante planifie le programme MyApp mois sur le 21 jours du mois à minuit. La commande spécifie que cette tâche doit s’exécuter pendant un an, 2 juillet 2002 au 30 juin 2003.

La commande utilise le **/mois** paramètre pour spécifier l’intervalle mensuel (tous les deux mois), le **/d** paramètre pour spécifier la date et le **/st** pour spécifier le délai. Il utilise également le **/sd** et **/ED** paramètres pour spécifier le début de date et date de fin. Étant donné que l’ordinateur local est définie sur le **anglais (Afrique du Sud)** option **Options régionales et linguistiques** dans **le panneau de configuration**, les dates sont spécifiés dans le format local , AAAA/MM/JJ.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo 2 /d 21 /st 00:00 /sd 2002/07/01 /ed 2003/06/30 
```

### <a name="BKMK_spec_day"></a>Pour planifier une tâche qui s’exécute sur un jour spécifique de la semaine

#### <a name="weekly-schedule-syntax"></a>Syntaxe de la planification hebdomadaire

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc weekly [/d {<MON - SUN>[,MON - SUN...] | *}] [/mo {1 - 52}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

La planification « jour de la semaine » est une variante de la planification hebdomadaire. Dans une planification hebdomadaire, le **/sc hebdomadaire** paramètre est obligatoire. Le **/mois** paramètre de (modificateur) est facultatif et spécifie le nombre de semaines entre chaque exécution de la tâche. La valeur par défaut **/mois** est 1 (chaque semaine). Le **/d** paramètre, qui est facultatif, planifie la tâche à exécuter sur les jours spécifiés de la semaine, ou sur tous les jours (<em>). La valeur par défaut est MON (lundi). L’option tous les jours (</em>* /d \***) est équivalente à la planification d’une tâche quotidienne.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-every-wednesday"></a>Pour planifier une tâche qui s’exécute tous les mercredis

La commande suivante planifie le programme MyApp pour exécuter chaque semaine le mercredi. La commande utilise le **/d** paramètre pour spécifier le jour de la semaine. Étant donné que la commande omet le **/mois** paramètre, la tâche s’exécute chaque semaine.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /d WED
```

#### <a name="to-schedule-a-task-that-runs-every-eight-weeks-on-monday-and-friday"></a>Pour planifier une tâche qui s’exécute toutes les huit semaines le lundi et vendredi

La commande suivante permet de planifier une tâche à exécuter le lundi et vendredi de chaque semaine huitième. Il utilise le **/d** paramètre pour spécifier les jours et les **/mois** paramètre pour spécifier l’intervalle de huit semaines.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc weekly /mo 8 /d MON,FRI
```

### <a name="BKMK_spec_week"></a>Pour planifier une tâche qui s’exécute sur une semaine spécifique au cours du mois

#### <a name="specific-week-syntax"></a>Syntaxe de la semaine spécifique

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo {FIRST | SECOND | THIRD | FOURTH | LAST} /d MON - SUN [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans ce type de planification, le **/sc tous les mois** paramètre, le **/mois** paramètre (modificateur) et le **/d** paramètre de (jours) sont requis. Le **/mois** les paramètre (modificateur) spécifiant la semaine sur lequel la tâche s’exécute. Le **/d** paramètre spécifie le jour de la semaine. (Vous pouvez spécifier qu’un seul jour de la semaine pour ce type de planification.) Cette grille est également facultative **/m** paramètre (mois) qui vous permet de planifier la tâche des mois spécifiques ou chaque mois (<em>). La valeur par défaut pour le * */m</em>* paramètre est chaque mois (* ).

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-for-the-second-sunday-of-every-month"></a>Pour planifier une tâche pour le deuxième dimanche de chaque mois

La commande suivante planifie le programme MyApp sur le deuxième dimanche de chaque mois. Il utilise le **/mois** paramètre pour spécifier la deuxième semaine du mois et le **/d** paramètre pour spécifier le jour.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo SECOND /d SUN
```

#### <a name="to-schedule-a-task-for-the-first-monday-in-march-and-september"></a>Pour planifier une tâche pour le premier lundi en mars et en septembre

La commande suivante planifie le programme MyApp sur le premier lundi en mars et en septembre. Il utilise le **/mois** paramètre pour spécifier la première semaine du mois et le **/d** paramètre pour spécifier le jour. Il utilise **/m** paramètre pour spécifier le mois, en séparant les arguments mois par une virgule.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo FIRST /d MON /m MAR,SEP
```

### <a name="BKMK_spec_date"></a>Pour planifier une tâche qui s’exécute sur une date spécifique chaque mois

#### <a name="specific-date-syntax"></a>Syntaxe date spécifique

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /d {1 - 31} [/m {JAN - DEC[,JAN - DEC...] | *}] [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification date spécifique, le **/sc tous les mois** paramètre et le **/d** paramètre de (jours) sont requis. Le **/d** paramètre spécifie une date du mois (1 à 31), pas un jour de la semaine. Vous pouvez spécifier qu’un seul jour dans la planification. Le **/mois** les paramètre (modificateur) ne sont pas valide avec ce type de planification.

Le **/m** paramètre (month) est facultatif pour ce type de planification et la valeur par défaut est chaque mois (<em>). **Schtasks</em>*  ne permet pas de planifier une tâche pour une date qui ne se produit pas dans un mois spécifié par le **/m** paramètre. Toutefois, si omettre la **/m** paramètre et la planification d’une tâche pour une date qui n’apparaît pas dans tous les mois, par exemple le 31e jour, puis la tâche ne s’exécute pas dans les mois plus courts. Pour planifier une tâche pour le dernier jour du mois, utilisez le dernier type de planification de jour.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-for-the-first-day-of-every-month"></a>Pour planifier une tâche pour le premier jour de chaque mois

La commande suivante planifie le programme MyApp sur le premier jour de chaque mois. Étant donné que le modificateur par défaut est none (aucun modificateur), le jour de la valeur par défaut est le jour 1 et le mois par défaut est chaque mois, la commande n’a pas besoin de tous les paramètres supplémentaires.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly
```

#### <a name="to-schedule-a-task-for-the-15th-days-of-may-and-june"></a>Pour planifier une tâche pendant les 15 jours de mai et juin

La commande suivante planifie le programme MyApp sur 15 mai et le 15 juin à 3 h 00 (15:00). Il utilise le **/m** paramètre pour spécifier la date et le **/m** paramètre pour spécifier les mois. Il utilise également le **/st** paramètre pour spécifier l’heure de début.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /m MAY,JUN /st 15:00
```

### <a name="BKMK_last_day"></a>Pour planifier une tâche qui s’exécute sur le dernier jour du mois

#### <a name="last-day-syntax"></a>Syntaxe de la dernière journée

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc monthly /mo LASTDAY /m {JAN - DEC[,JAN - DEC...] | *} [/st <HH:MM>] [/sd <StartDate>] [/ed <EndDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Le dernier jour type de planification, le **/sc tous les mois** paramètre, le **/mo LASTDAY** paramètre (modificateur) et le **/m** paramètre (month) sont requis. Le **/d** paramètre (day) n’est pas valide.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-for-the-last-day-of-every-month"></a>Pour planifier une tâche pour le dernier jour de chaque mois

La commande suivante planifie le programme MyApp sur le dernier jour de chaque mois. Il utilise le **/mois** paramètre pour spécifier le dernier jour et le **/m** paramètre avec le caractère générique (*) pour indiquer que le programme s’exécute chaque mois.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m *
```

#### <a name="to-schedule-a-task-at-600-pm-on-the-last-days-of-february-and-march"></a>Pour planifier une tâche à 18:00. le dernier jour de février et mars

La commande suivante planifie le programme MyApp sur le dernier jour du mois de février et le dernier jour du mois de mars à 18:00. Il utilise le **/mois** paramètre pour spécifier le dernier jour, le **/m** paramètre pour spécifier les mois et le **/st** paramètre pour spécifier l’heure de début.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /mo lastday /m FEB,MAR /st 18:00
```

### <a name="BKMK_once"></a>Pour planifier une tâche qui s’exécute une fois

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM> [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification, le **/sc qu’une seule fois** paramètre est obligatoire. Le **/st** paramètre qui spécifie l’heure à laquelle la tâche s’exécute, est obligatoire. Le **/sd** paramètre qui spécifie la date à laquelle la tâche s’exécute, est facultatif. Le **/mois** (modificateur) et **/ED** (date de fin) paramètres ne sont pas valides pour ce type de planification.

**SCHTASKS** ne vous autorise pas à planifier une tâche à exécuter une fois si la date et l’heure spécifiées se trouvent dans le passé, en fonction de l’heure de l’ordinateur local. Pour planifier une tâche qui s’exécute qu’une seule fois sur un ordinateur distant dans un autre fuseau horaire, vous devez le planifier avant cette date et heure se produit sur l’ordinateur local.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-one-time"></a>Pour planifier une tâche qui exécute une seule fois

La commande suivante planifie le programme MyApp exécuté à minuit le 1er janvier 2003. Il utilise le **/sc** paramètre pour spécifier le type de planification et le **/sd** et **st** pour spécifier la date et l’heure.

Étant donné que l’ordinateur local utilise le **anglais (États-Unis)** option **Options régionales et linguistiques** dans **le panneau de configuration**, le format de la date de début est MM/jj/aaaa.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /sd 01/01/2003 /st 00:00
```

### <a name="BKMK_startup"></a>Pour planifier une tâche qui s’exécute chaque fois que le système démarre

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Dans le type de planification de démarrage, le **/sc onstart** paramètre est obligatoire. Le **/sd** (date de début) paramètre est facultatif et la valeur par défaut est la date actuelle.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-when-the-system-starts"></a>Pour planifier une tâche qui s’exécute au démarrage du système

La commande suivante planifie le programme MyApp exécute chaque fois que le système démarre, en commençant le 15 mars 2001 :

Étant donné que l’ordinateur local est utilise le **anglais (États-Unis)** option **Options régionales et linguistiques** dans **le panneau de configuration**, le format de la date de début est MM/jj/aaaa .
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onstart /sd 03/15/2001
```

### <a name="BKMK_logon"></a>Pour planifier une tâche qui s’exécute lorsqu’un utilisateur ouvre une session

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

Le type de planification « sur l’ouverture de session » planifie une tâche qui s’exécute chaque fois qu’un utilisateur ouvre une session sur l’ordinateur. Dans le type de planification « sur l’ouverture de session », le **/sc onlogon** paramètre est obligatoire. Le **/sd** (date de début) paramètre est facultatif et la valeur par défaut est la date actuelle.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-when-a-user-logs-on-to-a-remote-computer"></a>Pour planifier une tâche qui s’exécute lorsqu’un utilisateur ouvre une session sur un ordinateur distant

La commande suivante permet de planifier un fichier de commandes à exécuter chaque fois qu’un utilisateur (n’importe quel utilisateur) ouvre une session sur l’ordinateur distant. Il utilise le **/s** paramètre pour spécifier l’ordinateur distant. Étant donné que la commande est distante, tous les chemins d’accès dans la commande, y compris le chemin d’accès au fichier de commandes, reportez-vous à un chemin d’accès sur l’ordinateur distant.
```
schtasks /create /tn "Start Web Site" /tr c:\myiis\webstart.bat /sc onlogon /s Server23
```

### <a name="BKMK_idle"></a>Pour planifier une tâche qui s’exécute lorsque le système est inactif

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999} [/sd <StartDate>] [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="remarks"></a>Notes

La planification « lors de l’inactivité » type planifie une tâche qui s’exécute chaque fois qu’il n’existe aucune activité de l’utilisateur pendant la durée spécifiée par la **/i** paramètre. Dans la planification « lors de l’inactivité » type, le **/sc onidle** paramètre et le **/i** paramètre sont requises. Le **/sd** (date de début) est facultatif et la valeur par défaut est la date actuelle.

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-whenever-the-computer-is-idle"></a>Pour planifier une tâche qui s’exécute chaque fois que l’ordinateur est inactif

La commande suivante planifie le programme MyApp pour exécuter chaque fois que l’ordinateur est inactif. Il utilise requis **/i** paramètre pour spécifier que l’ordinateur doit rester inactif pendant dix minutes avant le démarrage de la tâche.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc onidle /i 10
```

### <a name="BKMK_now"></a>Pour planifier une tâche qui s’exécute maintenant

**SCHTASKS** ne pas avoir un « exécuter maintenant » option, mais vous pouvez la simuler en créant une tâche qui s’exécute qu’une seule fois et démarre dans quelques minutes.

#### <a name="syntax"></a>Syntaxe

```
schtasks /create /tn <TaskName> /tr <TaskRun> /sc once [/st <HH:MM>] /sd <MM/DD/YYYY> [/it] [/ru {[<Domain>\]<User> [/rp <Password>] | System}] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

#### <a name="examples"></a>Exemples

#### <a name="to-schedule-a-task-that-runs-a-few-minutes-from-now"></a>Pour planifier une tâche qui exécute quelques minutes à partir de maintenant.

La commande suivante permet de planifier une tâche à exécuter une seule fois, le 13 novembre 2002 à 14:18 h 00 heure locale.

Étant donné que l’ordinateur local est utilise le **anglais (États-Unis)** option **Options régionales et linguistiques** dans **le panneau de configuration**, le format de la date de début est MM/jj/aaaa .
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc once /st 14:18 /sd 11/13/2002
```

### <a name="BKMK_diff_perms"></a>Pour planifier une tâche qui s’exécute avec des autorisations différentes

Vous pouvez planifier des tâches de tous les types de s’exécuter avec les autorisations d’un autre compte sur l’ordinateur local et un ordinateur distant. Outre les paramètres requis pour le type de planification particulière, le **/ru** paramètre est obligatoire et le **/rp** paramètre est facultatif.

#### <a name="examples"></a>Exemples

#### <a name="to-run-a-task-with-administrator-permissions-on-the-local-computer"></a>Pour exécuter une tâche avec les autorisations d’administrateur sur l’ordinateur local

La commande suivante planifie le programme MyApp à exécuter sur l’ordinateur local. Il utilise le **/ru** pour spécifier que la tâche doit s’exécuter avec les autorisations du compte d’administrateur de l’utilisateur (Admin06). Dans cet exemple, la tâche est planifiée pour s’exécuter tous les mardis, mais vous pouvez utiliser n’importe quel type de planification pour une tâche à exécuter avec d’autres autorisations.
```
schtasks /create /tn "My App" /tr myapp.exe /sc weekly /d TUE /ru Admin06
```
En réponse, **SchTasks.exe** vous invite à entrer le mot de passe « exécuter en tant que » pour le compte Admin06, puis affiche un message de réussite.
```
Please enter the run as password for Admin06: ********
SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-alternate-permissions-on-a-remote-computer"></a>Pour exécuter une tâche avec d’autres autorisations sur un ordinateur distant

La commande suivante planifie l’exécution du programme MonApp sur l’ordinateur de Marketing tous les quatre jours.

La commande utilise le **/sc** paramètre pour spécifier une planification quotidienne et **/mois** paramètre pour spécifier un intervalle de quatre jours.

La commande utilise le **/s** paramètre pour fournir le nom de l’ordinateur distant et le **/u** paramètre pour spécifier un compte autorisé à planifier une tâche sur l’ordinateur distant (Admin01 concernant la commercialisation ordinateur). Il utilise également le **/ru** paramètre pour spécifier que la tâche doit s’exécuter avec les autorisations de compte non administrateur de l’utilisateur (User01 dans le domaine Reskits). Sans le **/ru** paramètre, la tâche s’exécutera avec les autorisations du compte spécifié par **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /mo 4 /s Marketing /u Marketing\Admin01 /ru Reskits\User01
```
**SCHTASKS** demande tout d’abord le mot de passe de l’utilisateur nommé par le **/u** paramètre (pour exécuter la commande), puis demande le mot de passe de l’utilisateur nommé par le **/ru** paramètre (pour exécuter la tâche). Après avoir authentifié les mots de passe, **schtasks** affiche un message indiquant que la tâche est planifiée.
```
Type the password for Marketing\Admin01:********

Please enter the run as password for Reskits\User01: ********

SUCCESS: The scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-only-when-a-particular-user-is-logged-on"></a>Pour exécuter une tâche uniquement lorsqu’un utilisateur a ouvert une session

La commande suivante permet de planifier le programme AdminCheck.exe sur l’ordinateur Public chaque vendredi à 4 h 00, mais uniquement si l’administrateur de l’ordinateur est connecté.

La commande utilise le **/sc** paramètre pour spécifier une planification hebdomadaire, le **/d** paramètre pour spécifier le jour et le **/st** paramètre pour spécifier l’heure de début.

La commande utilise le **/s** paramètre pour fournir le nom de l’ordinateur distant et le **/u** paramètre pour spécifier un compte autorisé à planifier une tâche sur l’ordinateur distant. Il utilise également le **/ru** paramètre pour configurer la tâche s’exécute avec les autorisations de l’administrateur de l’ordinateur Public (Public\Admin01) et le **/it** paramètre pour indiquer que la tâche s’exécute uniquement Lorsque le compte Public\Admin01 est connecté.
```
schtasks /create /tn "Check Admin" /tr AdminCheck.exe /sc weekly /d FRI /st 04:00 /s Public /u Domain3\Admin06 /ru Public\Admin01 /it
```
**Remarque**
-   Pour identifier les tâches avec l’interactif uniquement ( **/it**) propriété, utilisez une requête détaillée **(/ interroger /v**). Dans l’affichage d’une requête détaillée d’une tâche avec **/it**, le **le Mode d’ouverture de session** champ a la valeur **Interactive uniquement**.

### <a name="BKMK_sys_perms"></a>Pour planifier une tâche qui s’exécute avec les autorisations système

Tous les types de tâches peuvent s’exécuter avec les autorisations du compte système sur l’ordinateur local et un ordinateur distant. Outre les paramètres requis pour le type de planification particulière, le **/rusystem** (ou **/ru » «** ) paramètre est obligatoire et le **/rp** paramètre n’est pas valide.

**Important**
-   Le compte système n’a pas de droits d’ouverture de session interactive. Les utilisateurs ne peuvent pas voir ou interagir avec des programmes ou des tâches s’exécutent avec les autorisations système.
-   Le **/ru** paramètre détermine les autorisations sous lequel la tâche s’exécute, pas les autorisations utilisées pour planifier la tâche. Seuls les administrateurs peuvent planifier des tâches, quelle que soit la valeur de la **/ru** paramètre.

**Remarque**

Pour identifier les tâches qui s’exécutent avec les autorisations système, utilisez une requête détaillée ( **/interroger** **/v**). Dans l’affichage d’une requête détaillée d’une tâche d’exécution du système, le **exécuter en tant qu’utilisateur** champ a la valeur **NT AUTHORITY\SYSTEM** et le **le Mode d’ouverture de session** champ a la valeur  **En arrière-plan uniquement**.

#### <a name="examples"></a>Exemples

#### <a name="to-run-a-task-with-system-permissions"></a>Pour exécuter une tâche avec les autorisations système

La commande suivante planifie le programme MyApp à exécuter sur l’ordinateur local avec les autorisations du compte système. Dans cet exemple, la tâche est planifiée pour s’exécuter sur le quinzième jour de chaque mois, mais vous pouvez utiliser n’importe quel type de planification pour exécuter une tâche avec les autorisations du système.

La commande utilise le **/ru système** paramètre pour spécifier le contexte de sécurité du système. Étant donné que les tâches système n’utilisent pas un mot de passe, le **/rp** paramètre est omis.
```
schtasks /create /tn "My App" /tr c:\apps\myapp.exe /sc monthly /d 15 /ru System
```
En réponse, **SchTasks.exe** affiche un message d’information et un message de réussite. Il ne demande pas un mot de passe.
```
INFO: The task will be created under user name ("NT AUTHORITY\SYSTEM").
SUCCESS: The Scheduled task "My App" has successfully been created.
```

#### <a name="to-run-a-task-with-system-permissions-on-a-remote-computer"></a>Pour exécuter une tâche avec les autorisations système sur un ordinateur distant

La commande suivante planifie le programme MyApp pour s’exécuter sur l’ordinateur Finance01 chaque matin à 4 h 00 avec les autorisations du système.

La commande utilise le **/tn** paramètre pour nommer la tâche et le **/tr** paramètre pour spécifier la copie distante du programme MyApp. Il utilise le **/sc** paramètre pour spécifier une planification quotidienne, mais omet le **/mois** paramètre, car 1 (chaque jour) est la valeur par défaut. Il utilise le **/st** paramètre pour spécifier l’heure de début, qui est également l’heure de la tâche sera exécutée chaque jour.

La commande utilise le **/s** paramètre pour fournir le nom de l’ordinateur distant et le **/u** paramètre pour spécifier un compte autorisé à planifier une tâche sur l’ordinateur distant. Il utilise également le **/ru** paramètre pour spécifier que la tâche doit s’exécuter sous le compte système. Sans le **/ru** paramètre, la tâche s’exécutera avec les autorisations du compte spécifié par **/u**.
```
schtasks /create /tn "My App" /tr myapp.exe /sc daily /st 04:00 /s Finance01 /u Admin01 /ru System
```
**SCHTASKS** demande le mot de passe de l’utilisateur nommé par le **/u** paramètre et, après authentification, le mot de passe, affiche un message indiquant que la tâche est créée et qu’elle s’exécute avec les autorisations du système compte.
```
Type the password for Admin01:**********

INFO: The Schedule Task "My App" will be created under user name ("NT AUTHORITY\
SYSTEM").
SUCCESS: The scheduled task "My App" has successfully been created.
```

### <a name="BKMK_multi_progs"></a>Pour planifier une tâche qui exécute plusieurs programmes

Chaque tâche exécute un seul programme. Toutefois, vous pouvez créer un fichier de commandes qui s’exécute plusieurs programmes et ensuite planifier une tâche à exécuter le fichier de commandes. La procédure suivante illustre cette méthode :
1. Créer un fichier de commandes qui démarre les programmes que vous souhaitez exécuter.

   Dans cet exemple, vous créez un fichier de commandes qui démarre l’Observateur d’événements (Eventvwr.exe) et le Moniteur système (Perfmon.exe).  
   - Ouvrez un éditeur de texte, tel que le Bloc-notes.
   - Tapez le nom et le chemin d’accès complet au fichier exécutable pour chaque programme. Dans ce cas, le fichier comprend les instructions suivantes.  
     ```
     C:\Windows\System32\Eventvwr.exe 
     C:\Windows\System32\Perfmon.exe
     ```  
   - Enregistrez le fichier sous le nom MonApp.bat.
2. Utilisez **Schtasks.exe** pour créer une tâche qui MonApp.bat.

   La commande suivante crée la tâche d’analyse, qui s’exécute chaque fois que l’utilisateur se connecte. Il utilise le **/tn** paramètre pour nommer la tâche et le **/tr** paramètre pour exécuter MonApp.bat. Il utilise le **/sc** paramètre pour indiquer le type de planification OnLogon et le **/ru** paramètre pour exécuter la tâche avec les autorisations du compte d’administrateur de l’utilisateur.  
   ```
   schtasks /create /tn Monitor /tr C:\MyApps.bat /sc onlogon /ru Reskit\Administrator
   ```  
   À la suite de cette commande, chaque fois qu’un utilisateur se connecte à l’ordinateur, la tâche démarre l’Observateur d’événements et le Moniteur système.

### <a name="BKMK_remote"></a>Pour planifier une tâche qui s’exécute sur un ordinateur distant

Pour planifier une tâche à exécuter sur un ordinateur distant, vous devez ajouter la tâche à la planification de l’ordinateur distant. Tâches de tous les types peuvent être planifiées sur un ordinateur distant, mais les conditions suivantes doivent être remplies.
-   Vous devez être autorisé à planifier la tâche. Par conséquent, vous devez être connecté à l’ordinateur local avec un compte qui est membre du groupe Administrateurs sur l’ordinateur distant, ou vous devez utiliser le **/u** paramètre pour fournir les informations d’identification d’un administrateur de l’ordinateur distant .
-   Vous pouvez utiliser la **/u** paramètre uniquement quand les ordinateurs locaux et distants sont dans le même domaine ou de l’ordinateur local est dans un domaine qui approuve le domaine de l’ordinateur distant. Sinon, l’ordinateur distant ne peut pas authentifier le compte d’utilisateur spécifié et il ne peut pas vérifier que le compte est membre du groupe Administrateurs.
-   La tâche doit avoir des autorisations suffisantes pour s’exécuter sur l’ordinateur distant. Les autorisations requises varient selon la tâche. Par défaut, la tâche s’exécute avec l’autorisation de l’utilisateur actuel de l’ordinateur local ou, si le **/u** paramètre est utilisé, la tâche s’exécute avec l’autorisation du compte spécifié par le **/u** paramètre. Toutefois, vous pouvez utiliser la **/ru** paramètre pour exécuter la tâche avec les autorisations d’un compte d’utilisateur différent ou avec les autorisations système.

#### <a name="examples"></a>Exemples

#### <a name="an-administrator-schedules-a-task-on-a-remote-computer"></a>Un administrateur planifie une tâche sur un ordinateur distant

La commande suivante planifie le programme MyApp pour s’exécuter sur l’ordinateur distant SRV01 tous les dix jours en commençant immédiatement. La commande utilise le **/s** paramètre pour fournir le nom de l’ordinateur distant. Étant donné que l’utilisateur local actuel est un administrateur de l’ordinateur distant, le **/u** paramètre, qui fournit d’autres autorisations pour la planification de la tâche, n’est pas nécessaire.

Veuillez noter que lors de la planification des tâches sur un ordinateur distant, tous les paramètres font référence à l’ordinateur distant. Par conséquent, le fichier exécutable spécifié par le **/tr** paramètre fait référence à la copie de MyApp.exe sur l’ordinateur distant.
```
schtasks /create /s SRV01 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc daily /mo 10
```
En réponse, **schtasks** affiche un message indiquant que la tâche est planifiée.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-1"></a>Un utilisateur planifie une commande sur un ordinateur distant (cas 1)

La commande suivante planifie le programme MyApp pour s’exécuter sur l’ordinateur distant SRV06 toutes les trois heures. Étant donné que les autorisations d’administrateur sont nécessaires pour planifier une tâche, la commande utilise le **/u** et **/p** paramètres pour fournir les informations d’identification d’administrateur de l’utilisateur du compte (Admin01 dans le Reskits domaine). Par défaut, ces autorisations sont également utilisées pour exécuter la tâche. Toutefois, étant donné que la tâche n’a pas besoin des autorisations d’administrateur pour exécuter, la commande inclut le **/u** et **/rp** paramètres pour remplacer la valeur par défaut et d’exécuter la tâche avec l’autorisation de l’utilisateur compte non administrateur sur l’ordinateur distant.
```
schtasks /create /s SRV06 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc hourly /mo 3 /u reskits\admin01 /p R43253@4$ /ru SRV06\user03 /rp MyFav!!Pswd
```
En réponse, **schtasks** affiche un message indiquant que la tâche est planifiée.

#### <a name="a-user-schedules-a-command-on-a-remote-computer-case-2"></a>Un utilisateur planifie une commande sur un ordinateur distant (cas 2)

La commande suivante planifie le programme MyApp exécuté sur l’ordinateur distant SRV02 le dernier jour de chaque mois. Étant donné que l’utilisateur local actuel (user03) n’est pas un administrateur de l’ordinateur distant, la commande utilise le **/u** paramètre pour fournir les informations d’identification d’administrateur de l’utilisateur du compte (Admin01 dans le domaine Reskits). Les autorisations du compte administrateur servira pour planifier la tâche et pour exécuter la tâche.
```
schtasks /create /s SRV02 /tn "My App" /tr "c:\program files\corpapps\myapp.exe" /sc monthly /mo LASTDAY /m * /u reskits\admin01
```
Étant donné que la commande ne comprenait pas le **/p** paramètre (mot de passe), **schtasks** vous invite à entrer le mot de passe. Puis il affiche un message de réussite et, dans ce cas, un avertissement.
```
Type the password for reskits\admin01:********

SUCCESS: The scheduled task "My App" has successfully been created.

WARNING: The Scheduled task "My App" has been created, but may not run because
the account information could not be set.
```
Cet avertissement indique que le domaine distant pas pu authentifier le compte spécifié par le **/u** paramètre. Dans ce cas, le domaine distant n’a pas pu authentifier le compte d’utilisateur, car l’ordinateur local n’est pas un membre d’un domaine qui approuve le domaine de l’ordinateur distant. Lorsque cela se produit, la tâche apparaît dans la liste des tâches planifiées, mais la tâche est en fait vide et ne fonctionnera pas.

L’affichage suivant à partir d’une requête détaillée expose le problème avec la tâche. Dans l’affichage, notez que la valeur de **prochaine exécution** est **jamais** et que la valeur de **exécuter en tant qu’utilisateur** est **n’a pas pu être récupéré du Planificateur de tâches base de données**.

Cet ordinateur avait été membre du même domaine ou un domaine approuvé, la tâche aurait ont été planifiée avec succès et aurait été exécutée comme spécifié.
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

-   Pour exécuter un **/ créer** commande avec les autorisations d’un autre utilisateur, utilisez la **/u** paramètre. Le **/u** paramètre est valide uniquement pour la planification des tâches sur des ordinateurs distants.
-   Pour afficher plus **schtasks / créer** exemples, tapez **schtasks / créer / ?** à l’invite de commandes.
-   Pour planifier une tâche qui s’exécute avec des autorisations d’un autre utilisateur, utilisez la **/ru** paramètre. Le **/ru** paramètre n’est valide pour les tâches sur les ordinateurs locaux et distants.
-   Pour utiliser le **/u** paramètre, l’ordinateur local doit être dans le même domaine que l’ordinateur distant ou doit être dans un domaine qui approuve le domaine de l’ordinateur distant. Sinon, soit la tâche n’est pas créée, ou si le travail de tâche est vide et la tâche ne s’exécute pas.
-   **SCHTASKS** toujours vous invite à entrer un mot de passe, sauf si vous en fournissez un, même lorsque vous planifiez une tâche sur l’ordinateur local à l’aide de compte d’utilisateur actuel. Ce comportement est normal pour **schtasks**.
-   **SCHTASKS** ne vérifie pas les emplacements des fichiers programmes ou des mots de passe de compte utilisateur. Si vous n’entrez pas d’emplacement de fichier incorrect ou le mot de passe du compte d’utilisateur, la tâche est créée, mais il ne s’exécute pas. En outre, si le mot de passe d’un compte change ou expire et que vous ne modifiez pas le mot de passe enregistré dans la tâche, puis la tâche ne s’exécute pas.
-   Le compte système n’a pas de droits d’ouverture de session interactive. Les utilisateurs n’apparaît pas et ne peut pas interagir avec les programmes s’exécutent avec les autorisations système.
-   Chaque tâche exécute un seul programme. Toutefois, vous pouvez créer un fichier de commandes qui démarre plusieurs tâches et puis planifier une tâche qui exécute le fichier de commandes.
-   Vous pouvez tester une tâche dès que vous le créez. Utilisez le **exécuter** opération pour la tâche de test, puis vérifiez le fichier SchedLgU.txt (*SystemRoot*\SchedLgU.txt) pour les erreurs.

## <a name="BKMK_change"></a>modification de Schtasks

Modifie un ou plusieurs des propriétés suivantes d’une tâche.
-   Le programme qui exécute la tâche ( **/tr**).
-   Le compte d’utilisateur sous lequel la tâche s’exécute ( **/ru**).
-   Le mot de passe du compte d’utilisateur ( **/rp**).
-   Ajoute la propriété interactif uniquement à la tâche ( **/it**).

### <a name="syntax"></a>Syntaxe

```
schtasks /change /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]] [/ru {[<Domain>\]<User> | System}] [/rp <Password>] [/tr <TaskRun>] [/st <StartTime>] [/ri <Interval>] [{/et <EndTime> | /du <Duration>} [/k]] [/sd <StartDate>] [/ed <EndDate>] [/{ENABLE | DISABLE}] [/it] [/z]
```

### <a name="parameters"></a>Paramètres

|          Terme           |                                                                                                                                                                                                                                                                                                                                     Définition                                                                                                                                                                                                                                                                                                                                      |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     /TN \<Nom_tâche >     |                                                                                                                                                                                                                                                                                                               Identifie la tâche à modifier. Entrez le nom de la tâche.                                                                                                                                                                                                                                                                                                               |
|     /s \<ordinateur >      |                                                                                                                                                                                                                                                                               Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                                                                                                                                                                                               |
|  /u [\<domaine >\]<User>  |                                                                                                                                                                 Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est les autorisations de l’utilisateur actuel de l’ordinateur local. Le compte d’utilisateur spécifié doit être un membre du groupe Administrateurs sur l’ordinateur distant. Le **/u** et **/p** paramètres sont valides uniquement pour modifier une tâche sur un ordinateur distant ( **/s**).                                                                                                                                                                  |
|     /p \<mot de passe >      |                                                                                                                                                                                              Spécifie le mot de passe du compte d’utilisateur spécifié dans le **/u** paramètre. Si vous utilisez le **/u** paramètre, mais omettez les **/p** paramètre ou l’argument de mot de passe, **schtasks** vous invitent à entrer un mot de passe.</br>Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**.                                                                                                                                                                                               |
| /ru {[\<Domain>\]<User> |                                                                                                                                                                                                                                                                                                                                       Système}                                                                                                                                                                                                                                                                                                                                       |
|     /RP \<mot de passe >     |                                                                                                                                                                                                                                                 Spécifie un nouveau mot de passe pour le compte d’utilisateur existant, ou le compte d’utilisateur spécifié par le **/ru** paramètre. Ce paramètre est ignoré avec utilisé avec le compte système local.                                                                                                                                                                                                                                                  |
|     /TR \<Exécution_tâche >      |                                                                                                                                                                                  Modifie le programme qui exécute la tâche. Entrez le chemin d’accès et le nom qualifié complet d’un fichier exécutable, un fichier de script ou un fichier de commandes. Si vous omettez le chemin d’accès, **schtasks** suppose que le fichier se trouve dans le \<systemroot > répertoire \System32. Le programme spécifié remplace le programme d’origine exécuté par la tâche.                                                                                                                                                                                  |
|    /st \<Starttime >     |                                                                                                                                                                                                                                                              Spécifie l’heure de début pour la tâche, en utilisant le format de 24 heures, hh : mm. Par exemple, une valeur de 14:30 est équivalente à l’heure de 12 heures de 2 h 30.                                                                                                                                                                                                                                                               |
|     / RI \<intervalle >     |                                                                                                                                                                                                                                                                           Spécifie l’intervalle de répétition de la tâche planifiée, en minutes. Plage valide est 1-599 940 (599 940 minutes = 9999 heures).                                                                                                                                                                                                                                                                            |
|     /et \<EndTime >      |                                                                                                                                                                                                                                                               Spécifie l’heure de fin pour la tâche, en utilisant le format de 24 heures, hh : mm. Par exemple, une valeur de 14:30 est équivalente à l’heure de 12 heures de 2 h 30.                                                                                                                                                                                                                                                                |
|     /du \<Duration>     |                                                                                                                                                                                                                                                                                                     Spécifie que la tâche à la \<EndTime > ou <Duration>, si spécifiée.                                                                                                                                                                                                                                                                                                      |
|           /k            |                                                                                                                                                                   Arrête le programme exécuté par la tâche à l’heure indiquée par **/et** ou **/du**. Sans **/k**, **schtasks** ne démarre pas le programme à nouveau après avoir atteint l’heure spécifiée par **/et** ou **/du**, mais il ne s’arrête pas le programme si elle est en cours d’exécution. Ce paramètre est facultatif et valide uniquement avec une planification toutes les heures ou minutes.                                                                                                                                                                   |
|    /sd \<StartDate>     |                                                                                                                                                                                                                                                                                              Spécifie la première date à laquelle la tâche doit être exécutée. Le format de date est MM/jj/aaaa.                                                                                                                                                                                                                                                                                               |
|     /ed \<EndDate>      |                                                                                                                                                                                                                                                                                                 Spécifie la dernière date à laquelle la tâche doit être exécutée. Le format est MM/jj/aaaa.                                                                                                                                                                                                                                                                                                  |
|         / ACTIVER         |                                                                                                                                                                                                                                                                                                                       Spécifie pour activer la tâche planifiée.                                                                                                                                                                                                                                                                                                                       |
|        /DISABLE         |                                                                                                                                                                                                                                                                                                                      Spécifie de désactiver la tâche planifiée.                                                                                                                                                                                                                                                                                                                       |
|           /it           | Spécifie d’exécuter la tâche planifiée uniquement lorsque l’utilisateur « exécuter en tant que » (le compte d’utilisateur sous lequel la tâche s’exécute) est connecté à l’ordinateur.</br>Ce paramètre n’a aucun effet sur les tâches qui s’exécutent avec les autorisations système ou les tâches que vous ont déjà défini la propriété interactif uniquement. Vous ne pouvez pas utiliser une commande de modification pour supprimer la propriété interactif uniquement à partir d’une tâche.</br>Par défaut, l’utilisateur « exécuter en tant que » est l’utilisateur actuel de l’ordinateur local lorsque la tâche est planifiée ou le compte spécifié par le **/u** paramètre, le cas échéant. Toutefois, si la commande inclut le **/ru** paramètre, puis l’utilisateur « exécuter en tant que » est le compte spécifié par le **/ru** paramètre. |
|           /z            |                                                                                                                                                                                                                                                                                                          Spécifie pour supprimer la tâche à la fin de sa planification.                                                                                                                                                                                                                                                                                                          |
|           /?            |                                                                                                                                                                                                                                                                                                                        Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                         |

### <a name="remarks"></a>Notes

-   Le **/tn** et **/s** paramètres identifient la tâche. Le **/tr**, **/ru**, et **/rp** paramètres spécifient les propriétés de la tâche que vous pouvez modifier.
-   Le **/ru**, et **/rp** paramètres spécifient les autorisations sous lesquelles la tâche s’exécute. Le **/u** et **/p** paramètres spécifient les autorisations utilisées pour modifier la tâche.
-   Pour modifier des tâches sur un ordinateur distant, l’utilisateur doit être connecté à l’ordinateur local avec un compte qui est membre du groupe Administrateurs sur l’ordinateur distant.
-   Pour exécuter un **/modifier** commande avec les autorisations d’un autre utilisateur ( **/u**, **/p**), l’ordinateur local doit être dans le même domaine que l’ordinateur distant ou qu’il doit se trouver dans un domaine que le domaine de l’ordinateur distant approuve.
-   Le compte système n’a pas de droits d’ouverture de session interactive. Les utilisateurs n’apparaît pas et ne peut pas interagir avec les programmes s’exécutent avec les autorisations système.
-   Pour identifier les tâches avec la **/it** propriété, utilisez une requête détaillée ( **/interroger /v**). Dans l’affichage d’une requête détaillée d’une tâche avec **/it**, le **le Mode d’ouverture de session** champ a la valeur **Interactive uniquement**.

### <a name="examples"></a>Exemples

### <a name="to-change-the-program-that-a-task-runs"></a>Pour modifier le programme qui exécute une tâche

La commande suivante modifie le programme que la tâche Virus Check exécute à partir de VirusCheck.exe à VirusCheck2.exe. Cette commande utilise le **/tn** paramètre pour identifier la tâche et le **/tr** paramètre pour spécifier le nouveau programme pour la tâche. (Vous ne pouvez pas modifier le nom de la tâche).
```
schtasks /change /tn "Virus Check" /tr C:\VirusCheck2.exe
```
En réponse, **SchTasks.exe** affiche le message suivant :
```
SUCCESS: The parameters of the scheduled task "Virus Check" have been changed.
```
À la suite de cette commande, la tâche de vérification de Virus s’exécute désormais VirusCheck2.exe.

### <a name="to-change-the-password-for-a-remote-task"></a>Pour modifier le mot de passe pour une tâche à distance

La commande suivante modifie le mot de passe du compte d’utilisateur de la tâche RemindMe sur l’ordinateur distant, Svr01. La commande utilise le **/tn** paramètre pour identifier la tâche et le **/s** paramètre pour spécifier l’ordinateur distant. Il utilise le **/rp** paramètre pour spécifier le nouveau mot de passe, p@ssWord3.

Cette procédure est nécessaire chaque fois que le mot de passe pour un compte d’utilisateur arrive à expiration ou change. Si le mot de passe enregistré dans une tâche n’est plus valide, la tâche ne s’exécute pas.
```
schtasks /change /tn RemindMe /s Svr01 /rp p@ssWord3
```
En réponse, **SchTasks.exe** affiche le message suivant :
```
SUCCESS: The parameters of the scheduled task "RemindMe" have been changed.
```
À la suite de cette commande, la tâche RemindMe s’exécute désormais sous son compte d’utilisateur d’origine, mais avec un nouveau mot de passe.

### <a name="to-change-the-program-and-user-account-for-a-task"></a>Pour modifier le compte d’utilisateur et de programme pour une tâche

La commande suivante modifie le programme qui exécute une tâche et les modifications que le compte d’utilisateur sous lequel la tâche s’exécute. Pour l’essentiel, il utilise une ancienne planification pour une nouvelle tâche. Cette commande modifie la tâche ChkNews, qui démarre à Notepad.exe chaque matin à 9 h 00, pour démarrer Internet Explorer à la place.

La commande utilise le **/tn** paramètre pour identifier la tâche. Il utilise le **/tr** paramètre à changer le programme qui exécute la tâche et le **/ru** paramètre pour modifier le compte d’utilisateur sous lequel s’exécute la tâche.

Le **/ru**, et **/rp** , qui fournit le mot de passe du compte d’utilisateur, il est omis. Vous devez fournir un mot de passe pour le compte, mais vous pouvez utiliser la **/ru**, et **/rp** paramètre et tapez le mot de passe en texte clair ou attendre des **SchTasks.exe** vous invite à un mot de passe, puis entrez le mot de passe en texte caché.
```
schtasks /change /tn ChkNews /tr "c:\program files\Internet Explorer\iexplore.exe" /ru DomainX\Admin01
```
En réponse, **SchTasks.exe** demande le mot de passe du compte d’utilisateur. Il masque le texte que vous tapez, par conséquent, le mot de passe n’est pas visible.
```
Please enter the password for DomainX\Admin01: 
```
Notez que le **/tn** paramètre identifie la tâche et que le **/tr** et **/ru** paramètres modifier les propriétés de la tâche. Vous ne pouvez pas utiliser un autre paramètre pour identifier la tâche et vous ne pouvez pas modifier le nom de la tâche.

En réponse, **SchTasks.exe** affiche le message suivant :
```
SUCCESS: The parameters of the scheduled task "ChkNews" have been changed.
```
À la suite de cette commande, la tâche ChkNews exécute maintenant Internet Explorer avec les autorisations d’un compte administrateur.

### <a name="to-change-a-program-to-the-system-account"></a>Pour modifier un programme pour le compte système

La commande suivante modifie la tâche ScriptDeSécurité afin qu’elle s’exécute avec les autorisations du compte système. Il utilise le **/ru » «** paramètre pour indiquer le compte système.
```
schtasks /change /tn SecurityScript /ru ""
```
En réponse, **SchTasks.exe** affiche le message suivant :
```
INFO: The run as user name for the scheduled task "SecurityScript" will be changed to "NT AUTHORITY\SYSTEM".
SUCCESS: The parameters of the scheduled task "SecurityScript" have been changed.
```
Étant donné que les tâches exécutées avec les autorisations du compte système ne nécessitent pas un mot de passe, **SchTasks.exe** ne demande pas.

### <a name="to-run-a-program-only-when-i-am-logged-on"></a>Pour exécuter un programme uniquement lorsque je suis connecté

La commande suivante ajoute la propriété interactif uniquement à MyApp, une tâche existante. Cette propriété garantit que la tâche s’exécute uniquement lorsque l’utilisateur « exécuter en tant que », autrement dit, le compte d’utilisateur sous lequel la tâche s’exécute, est connecté à l’ordinateur.

La commande utilise le **/tn** paramètre pour identifier la tâche et le **/it** paramètre à ajouter la propriété interactif uniquement à la tâche. Étant donné que la tâche s’exécute déjà avec les autorisations de mon compte d’utilisateur, je n’avez pas besoin de modifier le **/ru** paramètre pour la tâche.
```
schtasks /change /tn MyApp /it
```
En réponse, **SchTasks.exe** affiche le message suivant.
```
SUCCESS: The parameters of the scheduled task "MyApp" have been changed.
```

## <a name="BKMK_run"></a>Exécutez Schtasks

Démarre une tâche planifiée immédiatement. Le **exécuter** opération ignore la planification, mais utilise l’emplacement du fichier programme, le compte d’utilisateur et le mot de passe enregistré dans la tâche pour exécuter la tâche immédiatement.

### <a name="syntax"></a>Syntaxe

```
schtasks /run /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                                 Définition                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<Nom_tâche >    |                                                                                                                                                       Obligatoire. Identifie la tâche.                                                                                                                                                        |
|    /s \<ordinateur >     |                                                                                                           Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                           |
| /u [\<domaine >\]<User> | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local.</br>Le compte d’utilisateur spécifié doit être un membre du groupe Administrateurs sur l’ordinateur distant. Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                          Spécifie le mot de passe du compte d’utilisateur spécifié dans le **/u** paramètre. Si vous utilisez le **/u** paramètre, mais omettez les **/p** paramètre ou l’argument de mot de passe, **schtasks** vous invitent à entrer un mot de passe.</br>Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**.                           |
|          /?           |                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                     |

### <a name="remarks"></a>Notes

-   Cette opération permet de tester vos tâches. Si une tâche ne s’exécute pas, vérifiez le journal des transactions Service Planificateur de tâches, \<Systemroot > \SchedLgU.txt, des erreurs.
-   Exécution d’une tâche n’affecte pas la planification de la tâche et ne modifie pas la prochaine exécution planifiée de la tâche.
-   Pour exécuter une tâche à distance, la tâche doit être planifiée sur l’ordinateur distant. Lorsque vous l’exécutez, la tâche s’exécute uniquement sur l’ordinateur distant. Pour vérifier qu’une tâche est en cours d’exécution sur un ordinateur distant, utilisez le Gestionnaire des tâches ou le journal des transactions du Planificateur de tâches, \<Systemroot > \SchedLgU.txt.

### <a name="examples"></a>Exemples

### <a name="to-run-a-task-on-the-local-computer"></a>Pour exécuter une tâche sur l’ordinateur local

La commande suivante démarre la tâche « Script de sécurité ».
```
schtasks /run /tn "Security Script"
```
En réponse, **SchTasks.exe** lance le script associé à la tâche et affiche le message suivant :
```
SUCCESS: Attempted to run the scheduled task "Security Script".
```
Comme l’indique, le message **schtasks** tente de démarrer le programme, mais il ne peut pas vérifier que le programme est réellement démarré.

### <a name="to-run-a-task-on-a-remote-computer"></a>Pour exécuter une tâche sur un ordinateur distant

La commande suivante démarre la tâche de mise à jour sur un ordinateur distant, Svr01 :
```
schtasks /run /tn Update /s Svr01
```
Dans ce cas, **SchTasks.exe** affiche le message d’erreur suivant :
```
ERROR: Unable to run the scheduled task "Update".
```
Pour rechercher la cause de l’erreur, recherchez dans le journal des transactions tâches planifiées, C:\Windows\SchedLgU.txt sur Svr01. Dans ce cas, l’entrée suivante apparaît dans le journal :
```
"Update.job" (update.exe) 3/26/2001 1:15:46 PM ** ERROR **
The attempt to log on to the account associated with the task failed, therefore, the task did not run.
The specific error is:
0x8007052e: Logon failure: unknown user name or bad password.
Verify that the task's Run-as name and password are valid and try again.
```
Apparemment, le nom d’utilisateur ou mot de passe dans la tâche n’est pas valide sur le système. Ce qui suit **schtasks /change** commande met à jour le nom d’utilisateur et le mot de passe pour la tâche de mise à jour sur l’ordinateur Svr01 :
```
schtasks /change /tn Update /s Svr01 /ru Administrator /rp PassW@rd3
```
Après le **modifier** commande terminée, le **exécuter** commande est répétée. Cette fois, le programme Update.exe démarre et **SchTasks.exe** affiche le message suivant :
```
SUCCESS: Attempted to run the scheduled task "Update".
```
Comme l’indique, le message **schtasks** tente de démarrer le programme, mais il ne peut pas vérifier que le programme est réellement démarré.

## <a name="BKMK_end"></a>fin de Schtasks

Arrête un programme démarré par une tâche.

### <a name="syntax"></a>Syntaxe

```
schtasks /end /tn <TaskName> [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                               Définition                                                                                                                                                                |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /TN \<Nom_tâche >    |                                                                                                                                         Obligatoire. Identifie la tâche qui a lancé le programme.                                                                                                                                         |
|    /s \<ordinateur >     |                                                                                                                        Spécifie le nom ou l’adresse IP d’un ordinateur distant. La valeur par défaut est l'ordinateur local.                                                                                                                        |
| /u [\<domaine >\]<User> | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local. Le compte d’utilisateur spécifié doit être un membre du groupe Administrateurs sur l’ordinateur distant. Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le **/u** paramètre. Si vous utilisez le **/u** paramètre, mais omettez les **/p** paramètre ou l’argument de mot de passe, **schtasks** vous invitent à entrer un mot de passe.</br>Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**.                         |
|          /?           |                                                                                                                                                             Affiche l’aide.                                                                                                                                                              |

### <a name="remarks"></a>Notes

**SchTasks.exe** se termine uniquement les instances d’un programme démarré par une tâche planifiée. Pour arrêter les autres processus, utilisez TaskKill. Pour plus d’informations, consultez [Taskkill](taskkill.md).

### <a name="examples"></a>Exemples

### <a name="to-end-a-task-on-a-local-computer"></a>Pour terminer une tâche sur un ordinateur local

La commande suivante arrête l’instance de Notepad.exe a été démarrée par la tâche mon bloc-notes :
```
schtasks /end /tn "My Notepad"
```
En réponse, **SchTasks.exe** arrête l’instance de Notepad.exe que la tâche a commencé, et il affiche le message suivant :
```
SUCCESS: The scheduled task "My Notepad" has been terminated successfully.
```

### <a name="to-end-a-task-on-a-remote-computer"></a>Pour terminer une tâche sur un ordinateur distant

La commande suivante arrête l’instance d’Internet Explorer qui a été démarrée par la tâche InternetOn sur l’ordinateur distant, Svr01 :
```
schtasks /end /tn InternetOn /s Svr01
```
En réponse, **SchTasks.exe** arrête l’instance d’Internet Explorer que la tâche a commencé, et il affiche le message suivant :
```
SUCCESS: The scheduled task "InternetOn" has been terminated successfully.
```

## <a name="BKMK_delete"></a>schtasks delete

Supprime une tâche planifiée.

### <a name="syntax"></a>Syntaxe

```
schtasks /delete /tn {<TaskName> | *} [/f] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                                 Définition                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   /TN {\<Nom_tâche >    |                                                                                                                                                                     \*}                                                                                                                                                                     |
|          /f           |                                                                                                                                  Supprime le message de confirmation. La tâche est supprimée sans avertissement.                                                                                                                                  |
|    /s \<ordinateur >     |                                                                                                           Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                           |
| /u [\<domaine >\]<User> | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local.</br>Le compte d’utilisateur spécifié doit être un membre du groupe Administrateurs sur l’ordinateur distant. Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                          Spécifie le mot de passe du compte d’utilisateur spécifié dans le **/u** paramètre. Si vous utilisez le **/u** paramètre, mais omettez les **/p** paramètre ou l’argument de mot de passe, **schtasks** vous invitent à entrer un mot de passe.</br>Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**.                           |
|          /?           |                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                     |

### <a name="remarks"></a>Notes

- Le **supprimer** opération supprime la tâche à partir de la planification. Il ne pas supprimer le programme qui exécute la tâche ou interrompre un programme en cours d’exécution.
- Le **supprimer \\** * commande supprime toutes les tâches planifiées pour l’ordinateur, pas seulement les tâches planifiées par l’utilisateur actuel.

### <a name="examples"></a>Exemples

### <a name="to-delete-a-task-from-the-schedule-of-a-remote-computer"></a>Pour supprimer une tâche à partir de la planification d’un ordinateur distant

La commande suivante supprime la tâche « Démarrer messagerie » à partir de la planification d’un ordinateur distant. Il utilise le **/s** paramètre pour identifier l’ordinateur distant.
```
schtasks /delete /tn "Start Mail" /s Svr16
```
En réponse, **SchTasks.exe** affiche le message de confirmation suivant. Pour supprimer la tâche, appuyez sur o<strong>.</strong> Pour annuler la commande, tapez **n**:
```
WARNING: Are you sure you want to remove the task "Start Mail" (Y/N )? 
SUCCESS: The scheduled task "Start Mail" was successfully deleted.
```

### <a name="to-delete-all-tasks-scheduled-for-the-local-computer"></a>Pour supprimer toutes les tâches planifiées pour l’ordinateur local

La commande suivante supprime toutes les tâches de la planification de l’ordinateur local, y compris les tâches planifiées par d’autres utilisateurs. Il utilise le **/tn \\** * paramètre pour représenter toutes les tâches sur l’ordinateur et le **/f** paramètre pour supprimer le message de confirmation.
```
schtasks /delete /tn * /f
```
En réponse, **SchTasks.exe** affiche les messages suivants, indiquant que seule la tâche planifiée, le script de sécurité, est supprimée.

`SUCCESS: The scheduled task "SecureScript" was successfully deleted.`

## <a name="BKMK_query"></a>requête de Schtasks

Affiche les tâches planifiées pour s’exécuter sur l’ordinateur.

### <a name="syntax"></a>Syntaxe

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <Computer> [/u [<Domain>\]<User> [/p <Password>]]]
```

### <a name="parameters"></a>Paramètres

|         Terme          |                                                                                                                                                                 Définition                                                                                                                                                                  |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       [/query]        |                                                                                                                        Le nom de l’opération est facultatif. Tapant **schtasks** sans aucun paramètre effectue une requête.                                                                                                                         |
|      /FO {TABLE       |                                                                                                                                                                    LISTE                                                                                                                                                                     |
|          /nh          |                                                                                                            Omet les en-têtes de colonne de l’affichage de la table. Ce paramètre est valide avec le **TABLE** et **CSV** formats de sortie.                                                                                                             |
|          /v           |                                                                                                         Ajoute des propriétés avancées des tâches à l’affichage.</br>Requêtes à l’aide **/v** doit être mis en forme en tant que **liste** ou **CSV**.                                                                                                          |
|    /s \<ordinateur >     |                                                                                                           Spécifie le nom ou l’adresse IP d’un ordinateur distant (avec ou sans barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                                           |
| /u [\<domaine >\]<User> | Exécute cette commande avec les autorisations du compte d’utilisateur spécifié. Par défaut, la commande s’exécute avec les autorisations de l’utilisateur actuel de l’ordinateur local.</br>Le compte d’utilisateur spécifié doit être un membre du groupe Administrateurs sur l’ordinateur distant. Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**. |
|    /p \<mot de passe >     |                                        Spécifie le mot de passe du compte d’utilisateur spécifié dans le **/u** paramètre. Si vous utilisez **/u**, mais omettez **/p** ou l’argument de mot de passe, **schtasks** vous invitent à entrer un mot de passe.</br>Le **/u** et **/p** paramètres sont valides uniquement lorsque vous utilisez **/s**.                                         |
|          /?           |                                                                                                                                                    Affiche l'aide à l'invite de commandes.                                                                                                                                                     |

### <a name="remarks"></a>Notes

**SchTasks.exe** se termine uniquement les instances d’un programme démarré par une tâche planifiée. Pour arrêter les autres processus, utilisez TaskKill. Pour plus d’informations, consultez [Taskkill](taskkill.md).

### <a name="examples"></a>Exemples

### <a name="to-display-the-scheduled-tasks-on-the-local-computer"></a>Pour afficher les tâches planifiées sur l’ordinateur local

Les commandes suivantes affichent toutes les tâches planifiées pour l’ordinateur local. Ces commandes produisent le même résultat et peuvent être utilisés indifféremment.
```
schtasks
schtasks /query
```
En réponse, **SchTasks.exe** affiche les tâches dans le format par défaut, une table simple, comme indiqué dans le tableau suivant :
```
TaskName Next Run Time Status
========================= ======================== ==============
Microsoft Outlook At logon time
SecureScript 14:42:00 PM , 2/4/2001
```

### <a name="to-display-advanced-properties-of-scheduled-tasks"></a>Pour afficher les propriétés avancées des tâches planifiées

La commande suivante demande l’affichage détaillé des tâches sur l’ordinateur local. Il utilise le **/v** paramètre pour demander un affichage détaillé (verbose) et le **/fo liste** paramètre à mettre en forme l’affichage sous forme de liste pour faciliter la lecture. Vous pouvez utiliser cette commande pour vérifier qu’une tâche que vous avez créé le modèle de récurrence prévue.

**SCHTASKS /query /fo liste /v**

En réponse, **SchTasks.exe** affiche une liste de propriétés détaillées pour toutes les tâches. L’affichage suivant montre la liste des tâches pour une tâche planifiée pour s’exécuter à 4 h 00 le dernier vendredi de chaque mois :
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

### <a name="to-log-tasks-scheduled-for-a-remote-computer"></a>Pour vous connecter des tâches planifiées pour un ordinateur distant

La commande suivante demande une liste des tâches planifiées pour un ordinateur distant et ajoute les tâches dans un fichier journal séparées par des virgules sur l’ordinateur local. Vous pouvez utiliser ce format de commande pour collecter et suivre des tâches qui sont planifiées pour plusieurs ordinateurs.

La commande utilise le **/s** paramètre pour identifier l’ordinateur distant, Reskit16, le **/fo** paramètre pour spécifier le format et le **/nh** paramètre pour supprimer la colonne en-têtes. Le **>>** ajouter symbole redirige la sortie dans le journal des tâches, p0102.csv, sur l’ordinateur local, Svr01. Étant donné que la commande s’exécute sur l’ordinateur distant, le chemin d’accès de l’ordinateur local doit être qualifié complet.
```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```
En réponse, **SchTasks.exe** ajoute les tâches planifiées pour l’ordinateur Reskit16 dans le fichier p0102.csv sur l’ordinateur local, Svr01.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
