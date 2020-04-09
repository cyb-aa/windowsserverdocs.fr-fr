---
title: eventcreate
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79b10963abef9918e5962fdaf7d387a129873452
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845092"
---
# <a name="eventcreate"></a>eventcreate



Permet à un administrateur de créer un événement personnalisé dans un journal des événements spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<> de l’ordinateur|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u \<domaine\utilisateur >|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par \<utilisateur > ou < domaine\utilisateur >. Par défaut, il s’agit des autorisations de l’utilisateur actuellement connecté sur l’ordinateur qui émet la commande.|
|/p \<mot de passe >|Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .|
|/l {APPLICATION\|système}|Spécifie le nom du journal des événements dans lequel l’événement sera créé. Les noms de journaux valides sont APPLICATION et système.|
|/So \<NomSource >|Spécifie la source à utiliser pour l’événement. Une source valide peut être n’importe quelle chaîne et doit représenter l’application ou le composant qui génère l’événement.|
|/t {ERROR\|WARNING\|des informations\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Spécifie le type d’événement à créer. Les types valides sont ERROR, WARNING, INFORMATION, SUCCESSAUDIT et FAILUREAUDIT.|
|/ID \<EventID >|Spécifie l’ID d’événement de l’événement. Un ID valide est un nombre compris entre 1 et 1000.|
|Description \</d >|Spécifie la description à utiliser pour l’événement nouvellement créé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Les événements personnalisés ne peuvent pas être écrits dans le journal de sécurité.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
