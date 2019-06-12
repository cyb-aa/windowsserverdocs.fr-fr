---
title: taskkill
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13366307bbf685d1e4af8c0a58d5b9d6643111dc
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811048"
---
# <a name="taskkill"></a>taskkill

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Met fin à une ou plusieurs tâches ou à un ou plusieurs processus. Les processus peuvent être terminés par ID de processus ou par nom d’image. **TASKKILL** remplace le **kill** outil.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                                                                                        Description                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<computer>       |                                                                                    Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                     |
| /u \<domaine >\\\<nom d’utilisateur > | Exécute la commande avec les autorisations de compte de l’utilisateur qui est spécifié par *nom d’utilisateur* ou *domaine*\\*nom d’utilisateur*. **/u** peut être spécifié uniquement si **/s** est spécifié. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande. |
|      /p \<mot de passe >       |                                                                                                   Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.                                                                                                   |
|       /fi \<Filter>       |          Applique un filtre pour sélectionner un ensemble de tâches. Vous pouvez utiliser plusieurs filtres ou utiliser le caractère générique ( **\\** \*) pour spécifier toutes les tâches ou de noms d’image. Consultez les rubriques suivantes [table pour les noms de filtre valide](#filter-names-operators-and-values), opérateurs et valeurs.           |
|     /PID \<ProcessID >     |                                                                                                                 Spécifie l’ID de processus du processus à arrêter.                                                                                                                 |
|     /im \<ImageName>      |                                                                                Spécifie le nom de l’image de la fin du processus. Utilisez le caractère générique ( **\\** \*) pour spécifier tous les noms d’image.                                                                                |
|            /f             |                                                                    Spécifie que les processus doit être forcé. Ce paramètre est ignoré pour les processus distants ; tous les processus distants sont toujours forcés.                                                                     |
|            /t             |                                                                                                          Termine le processus spécifié et tous les processus enfant démarrés par ce dernier.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Filtrer les noms, opérateurs et valeurs

| Nom du filtre |    Opérateurs valides     |                                                                Ou les valeurs valides                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STatUS    |         eq, ne         |                                                 EN COURS D’EXÉCUTION &AMP;#124; NE RÉPOND NE PAS &AMP;#124; INCONNU                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  Nom de l’image                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valeur PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Numéro de session                                                                |
|   CPUtime   | eq, ne, gt, lt, ge, le | Temps processeur dans le format <em>HH</em> **:** <em>MM</em> **:** <em>SS</em>, où *MM* et *SS* sont comprises entre 0 et 59 et *HH* est le nombre non signé |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Utilisation de la mémoire en Ko                                                              |
|  NOM D’UTILISATEUR   |         eq, ne         |                                               N’importe quel nom d’utilisateur valide (*utilisateur* ou *domaine*\\*utilisateur*)                                               |
|  SERVICES   |         eq, ne         |                                                                 Nom du service                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Titre de la fenêtre                                                                 |
|   MODULES   |         eq, ne         |                                                                   Nom de la DLL                                                                   |

## <a name="remarks"></a>Notes
* Les filtres WINDOWTITLE et état ne sont pas pris en charge lorsqu’un système distant est spécifié.
* Le caractère générique ( **\\** <em>) est accepté pour la * */im</em>*  option uniquement lorsqu’un filtre est appliqué.
* Arrêt des processus distants est toujours effectué volontairement, indépendamment de si le **/f** option est spécifiée.
* En fournissant un nom d’ordinateur pour le filtre de nom d’hôte provoque un arrêt et tous les processus sont arrêtés.
* Vous pouvez utiliser **tasklist** pour déterminer l’ID de processus (PID) pour le processus à arrêter.

## <a name="examples"></a>Exemples

Pour terminer les processus avec le processus ID 1230, 1241 et 1253, tapez :

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

À la fin de force le processus « Notepad.exe » s’il a été démarré par le système, tapez :

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Pour mettre fin à tous les processus sur l’ordinateur distant « Srvmain » avec un image le nom commence par « note », tout en utilisant les informations d’identification du compte d’utilisateur Hiropln, tapez :

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Pour terminer le processus avec le processus ID 2134 et tout enfant les processus qui elle a, mais uniquement si ces processus ont été démarrés par le compte d’administrateur, tapez :

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Pour mettre fin à tous les processus qui ont un ID de processus supérieur ou égal à 1 000, quel que soit leur nom d’image, tapez :

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
