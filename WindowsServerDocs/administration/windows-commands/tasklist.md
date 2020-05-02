---
title: tasklist
description: Découvrez comment afficher une liste des processus en cours d’exécution sur l’ordinateur local ou distant.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c189af48e30729b53b04b343d2d4c0e4e81c4d0e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721540"
---
# <a name="tasklist"></a>tasklist

Affiche la liste des processus actuellement en cours d’exécution sur l’ordinateur local ou sur un ordinateur distant. **TaskList** remplace l’outil **tlist** .



## <a name="syntax"></a>Syntaxe

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

### <a name="parameters"></a>Paramètres

|          Paramètre           |                                                                                                                                            Description                                                                                                                                             |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        /s \<ordinateur>        |                                                                                         Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                         |
| /u [\<domaine>\\ \] \<nom d’utilisateur> | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par le *nom* d’utilisateur ou le nom d’utilisateur de *domaine*\*<em>. \*/u \*</em> \* ne peut être spécifié que si **/s** est spécifié. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande. |
|        /p \<mot de passe>        |                                                                                                       Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                        |
|         /m \<module>         |                                                               Répertorie toutes les tâches avec des modules de DLL chargés qui correspondent au nom de modèle donné. Si le nom du module n’est pas spécifié, cette option affiche tous les modules chargés par chaque tâche.                                                                |
|             /SVC             |                                                                                    Répertorie toutes les informations de service pour chaque processus sans troncation. Valide lorsque le paramètre **/FO** est défini sur **table**.                                                                                    |
|              /v              |                                                                                 Affiche des informations détaillées sur les tâches dans la sortie. Pour obtenir une sortie détaillée sans troncation, utilisez **/v** et **/SVC** ensemble.                                                                                 |
|  /FO {table \| List \| CSV}  |                                                                             Spécifie le format à utiliser pour la sortie. Les valeurs valides sont **table**, **List**et **CSV**. Le format par défaut de la sortie est **table**.                                                                             |
|             /NH              |                                                                                             Supprime les en-têtes de colonnes dans la sortie. Valide lorsque le paramètre **/FO** est défini sur **table** ou **CSV**.                                                                                              |
|        /fi \<filtre>         |                                                                          Spécifie les types de processus à inclure dans la requête ou à exclure de celle-ci. Consultez le tableau suivant pour connaître les noms de filtre, les opérateurs et les valeurs valides.                                                                          |
|              /?              |                                                                                                                                Affiche l'aide à l'invite de commandes.                                                                                                                                |

### <a name="filter-names-operators-and-values"></a>Filtrer les noms, les opérateurs et les valeurs

| Nom du filtre |    Opérateurs valides     |                                                                 Valeurs valides                                                                 |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATUT    |         eq, ne         |                                                                   RUNNING                                                                    |
|  IMAGENAME  |         eq, ne         |                                                                  Nom de l’image                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valeur PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Numéro de session                                                                |
| SESSION |         eq, ne         |                                                                 Nom de session                                                                 |
|   CPUTIME   | eq, ne, gt, lt, ge, le | Temps processeur au format <em>hh</em>**:**<em>mm</em>**:**<em>SS</em>, où *mm* et *SS* sont compris entre 0 et 59 et *hh* est un nombre non signé |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Utilisation de la mémoire en Ko                                                              |
|  USERNAME   |         eq, ne         |                                                             Nom d’utilisateur non valide                                                              |
|  SYNCHRONISATION DES IDENTITÉS   |         eq, ne         |                                                                 Nom du service                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Titre de la fenêtre                                                                 |
|   ACTUALIS   |         eq, ne         |                                                                   Nom de la DLL                                                                   |

## <a name="remarks"></a>Notes 

WINDOWTITLE et les filtres d’État ne sont pas pris en charge lorsqu’un système distant est spécifié.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour répertorier toutes les tâches dont l’ID de processus est supérieur à 1000, et les afficher au format CSV, tapez :
```
tasklist /v /fi "PID gt 1000" /fo csv
```
Pour répertorier les processus système en cours d’exécution, tapez :
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
Pour afficher des informations détaillées sur tous les processus en cours d’exécution, tapez :
```
tasklist /v /fi "STATUS eq running"
```
Pour répertorier toutes les informations de service pour les processus sur l’ordinateur distant « Srvmain » qui ont un nom de DLL commençant par « ntdll », tapez :
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Pour répertorier les processus sur l’ordinateur distant « Srvmain », à l’aide des informations d’identification de votre compte d’utilisateur actuellement connecté, tapez :
```
tasklist /s srvmain 
```
Pour répertorier les processus sur l’ordinateur distant « Srvmain », à l’aide des informations d’identification du compte d’utilisateur hiropln, tapez :
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)