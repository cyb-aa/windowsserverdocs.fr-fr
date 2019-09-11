---
title: taskkill
description: 'Rubrique relative aux commandes Windows pour * * * *- '
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
ms.openlocfilehash: 9050788a06b66e54be59c69dd8cc837092123b00
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878135"
---
# <a name="taskkill"></a>taskkill

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Met fin à une ou plusieurs tâches ou à un ou plusieurs processus. Les processus peuvent être terminés par ID de processus ou par nom d’image. **taskkill** remplace l’outil **Kill** .
Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```

## <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                                                                                        Description                                                                                                                                        |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /s \<ordinateur >       |                                                                                    Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.                                                                                     |
| /u \<domaine >\\nomd’utilisateur>\< | Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par le *nom* d’utilisateur ou le*nom d'* utilisateur de *domaine*\\. **/u** ne peut être spécifié que si **/s** est spécifié. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande. |
|      /p \<mot de passe >       |                                                                                                   Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                                                                   |
|       /fi \<filtre >       |          Applique un filtre pour sélectionner un ensemble de tâches. Vous pouvez utiliser plusieurs filtres ou utiliser le caractère générique ( **\\** \*) pour spécifier l’ensemble des tâches ou des noms d’images. Consultez le tableau suivant pour connaître les noms de filtre, les opérateurs et les valeurs [valides](#filter-names-operators-and-values).           |
|     /PID \<ProcessId >     |                                                                                                                 Spécifie l’ID de processus du processus à arrêter.                                                                                                                 |
|     /im \<ImageName >      |                                                                                Spécifie le nom de l’image du processus à arrêter. Utilisez le caractère générique ( **\\** \*) pour spécifier tous les noms d’images.                                                                                |
|            /f             |                                                                    Spécifie que les processus doivent être arrêtés de force. Ce paramètre est ignoré pour les processus distants ; tous les processus distants sont arrêtés de force.                                                                     |
|            commutateur             |                                                                                                          Met fin au processus spécifié et à tous les processus enfants démarrés par celui-ci.                                                                                                          |

#### <a name="filter-names-operators-and-values"></a>Filtrer les noms, les opérateurs et les valeurs

| Nom du filtre |    Opérateurs valides     |                                                                Valeur (s) valide (s)                                                                |
|-------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|   STATU    |         eq, ne         |                                                 EXÉCUTION &#124; SANS RÉPONSE &#124; INCONNUE                                                 |
|  IMAGENAME  |         eq, ne         |                                                                  Nom de l’image                                                                  |
|     PID     | eq, ne, gt, lt, ge, le |                                                                  Valeur PID                                                                   |
|   SESSION   | eq, ne, gt, lt, ge, le |                                                                Numéro de session                                                                |
|   CpuTime   | eq, ne, gt, lt, ge, le | Temps processeur au format <em>hh</em> **:** <em>mm</em> **:** <em>SS</em>, où *mm* et *SS* sont compris entre 0 et 59 et *hh* est un nombre non signé |
|  MEMUSAGE   | eq, ne, gt, lt, ge, le |                                                              Utilisation de la mémoire en Ko                                                              |
|  NOM D’UTILISATEUR   |         eq, ne         |                                               N’importe quel nom d'*utilisateur valide (utilisateur* ou *domaine*\\ *)*                                               |
|  SERVICES   |         eq, ne         |                                                                 Nom du service                                                                 |
| WINDOWTITLE |         eq, ne         |                                                                 Titre de la fenêtre                                                                 |
|   ACTUALIS   |         eq, ne         |                                                                   Nom de la DLL                                                                   |

## <a name="remarks"></a>Notes
* WINDOWTITLE et les filtres d’État ne sont pas pris en charge lorsqu’un système distant est spécifié.
* Le caractère générique ( **\\** <em>) est accepté pour l’option * */im</em>*  uniquement lorsqu’un filtre est appliqué.
* L’arrêt des processus distants est toujours effectué de force, que l’option **/f** soit spécifiée ou non.
* La spécification d’un nom d’ordinateur dans le filtre de nom d’hôte provoque un arrêt et l’arrêt de tous les processus.
* Vous pouvez utiliser la **TaskList** pour déterminer l’ID de processus (PID) du processus à arrêter.

## <a name="examples"></a>Exemples

Pour mettre fin aux processus avec les ID de processus 1230, 1241 et 1253, tapez :

```
taskkill /pid 1230 /pid 1241 /pid 1253
```

Pour forcer la fin du processus « Notepad. exe » s’il a été démarré par le système, tapez :

```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```

Pour mettre fin à tous les processus sur l’ordinateur distant « Srvmain » avec un nom d’image commençant par « remarque » tout en utilisant les informations d’identification du compte d’utilisateur hiropln, tapez :

```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```

Pour mettre fin au processus avec l’ID de processus 2134 et les processus enfants qu’il a démarré, mais uniquement si ces processus ont été démarrés par le compte d’administrateur, tapez :

```
taskkill /pid 2134 /t /fi "username eq administrator"
```

Pour mettre fin à tous les processus dont l’ID de processus est supérieur ou égal à 1000, quel que soit leur nom d’image, tapez :

```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
