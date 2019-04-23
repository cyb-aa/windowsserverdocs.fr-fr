---
title: tasklist
description: Découvrez comment afficher une liste des processus en cours d’exécution sur l’ordinateur local ou distant.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abb36c8c794836387af5547f3706e910dc06fa42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843000"
---
# <a name="tasklist"></a>tasklist

Affiche la liste des processus actuellement en cours d’exécution sur l’ordinateur local ou sur un ordinateur distant. **TaskList** remplace le **tlist** outil.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur >|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u [\<Domain>\\\]\<UserName>|Exécute la commande avec les autorisations de compte de l’utilisateur qui est spécifié par *nom d’utilisateur* ou *domaine*\** nom d’utilisateur. **/u** peut être spécifié uniquement si **/s** est spécifié. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande.|
|/p \<mot de passe >|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/m \<Module>|Répertorie toutes les tâches avec les modules DLL chargés qui correspondent au nom de modèle donné. Si le nom du module n’est pas spécifié, cette option affiche tous les modules chargés par chaque tâche.|
|/svc|Répertorie toutes les informations de service pour chaque processus sans troncation. Valide lorsque le **/fo** paramètre est défini sur **table**.|
|/v|Affiche des commentaires sur les tâches dans la sortie. Pour obtenir une sortie détaillée complète sans troncation, utilisez **/v** et **/svc** ensemble.|
|/FO {table \| liste \| csv}|Spécifie le format à utiliser pour la sortie. Les valeurs valides sont **table**, **liste**, et **csv**. Le format par défaut pour la sortie est **table**.|
|/nh|Supprime les en-têtes de colonne dans la sortie. Valide lorsque le **/fo** paramètre est défini sur **table** ou **csv**.|
|/fi \<Filter>|Spécifie les types de processus à inclure ou à exclure de la requête. Consultez le tableau suivant pour les noms de filtre valide, les opérateurs et les valeurs.|
|/?|Affiche l'aide à l'invite de commandes.|

### <a name="filter-names-operators-and-values"></a>Filtrer les noms, opérateurs et valeurs

|Nom du filtre|Opérateurs valides|Valeurs valides|
|-----------|---------------|------------|
|ÉTAT|eq, ne|EN COURS D’EXÉCUTION | NE RÉPOND NE PAS | INCONNU|
|IMAGENAME|eq, ne|Nom de l’image|
|PID|eq, ne, gt, lt, ge, le|Valeur PID|
|SESSION|eq, ne, gt, lt, ge, le|Numéro de session|
|NOM DE SESSION|eq, ne|Nom de session|
|CPUTIME|eq, ne, gt, lt, ge, le|Temps processeur dans le format *HH ***:*** MM ***:*** SS*, où *MM* et *SS* sont comprises entre 0 et 59 et *HH* est le nombre non signé|
|MEMUSAGE|eq, ne, gt, lt, ge, le|Utilisation de la mémoire en Ko|
|NOM D’UTILISATEUR|eq, ne|N’importe quel nom d’utilisateur valide|
|SERVICES|eq, ne|Nom du service|
|WINDOWTITLE|eq, ne|Titre de la fenêtre|
|MODULES|eq, ne|Nom de la DLL|

## <a name="remarks"></a>Notes

Les filtres WINDOWTITLE et état ne sont pas pris en charge lorsqu’un système distant est spécifié.

## <a name="BKMK_examples"></a>Exemples

Pour répertorier toutes les tâches avec un ID de processus supérieur à 1 000 et les afficher au format CSV, tapez :
```
tasklist /v /fi "PID gt 1000" /fo csv
```
Pour répertorier les processus système qui sont en cours d’exécution, tapez :
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
Pour répertorier des informations détaillées pour tous les processus en cours d’exécution, tapez :
```
tasklist /v /fi "STATUS eq running"
```
Pour répertorier toutes les informations de service pour les processus sur l’ordinateur distant « Srvmain » qui ont des DLL nom commence par « ntdll », tapez :
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Pour répertorier les processus sur l’ordinateur distant « Srvmain », les informations d’identification de votre compte d’utilisateur actuellement connecté, tapez :
```
tasklist /s srvmain 
```
Pour répertorier les processus sur l’ordinateur distant « Srvmain », les informations d’identification du compte d’utilisateur Hiropln, tapez :
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)