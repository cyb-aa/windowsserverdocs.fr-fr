---
title: eventcreate
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797298622ba1021caef3d04e2f2f06f016ef6a70
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725760"
---
# <a name="eventcreate"></a>eventcreate



Permet à un administrateur de créer un événement personnalisé dans un journal des événements spécifié. 

## <a name="syntax"></a>Syntaxe

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u \<domaine\utilisateur>|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié \<par l’utilisateur> ou <domaine\utilisateur>. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.|
|/p \<mot de passe>|Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .|
|/l {système\|d’applications}|Spécifie le nom du journal des événements dans lequel l’événement sera créé. Les noms de journaux valides sont APPLICATION et système.|
|/So \<NomSource>|Spécifie la source à utiliser pour l’événement. Une source valide peut être n’importe quelle chaîne et doit représenter l’application ou le composant qui génère l’événement.|
|/t {informations\|d'\|avertissement d’erreur\|</br>SUCCESSAUDIT\|FailureAudit}|Spécifie le type d’événement à créer. Les types valides sont ERROR, WARNING, INFORMATION, SUCCESSAUDIT et FAILUREAUDIT.|
|/ID \<EventID>|Spécifie l’ID d’événement de l’événement. Un ID valide est un nombre compris entre 1 et 1000.|
|/d \<Description>|Spécifie la description à utiliser pour l’événement nouvellement créé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Les événements personnalisés ne peuvent pas être écrits dans le journal de sécurité.

## <a name="examples"></a>Exemples

Les exemples suivants illustrent la façon dont vous pouvez utiliser la commande eventcreate :
```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

#### <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
