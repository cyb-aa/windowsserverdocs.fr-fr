---
title: eventcreate
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d575364adbeba9d9ea4da75a0a866bcc02acea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818710"
---
# <a name="eventcreate"></a>eventcreate



Permet à un administrateur créer un événement personnalisé dans un journal des événements spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur >|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u \<domaine\nom d’utilisateur >|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par \<utilisateur > ou < domaine\nom d’utilisateur >. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p \<mot de passe >|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/ l {APPLICATION\|système}|Spécifie le nom du journal des événements où l’événement doit être créé. Les noms des journaux valides sont APPLICATION et du système.|
|/so \<SrcName>|Spécifie la source à utiliser pour l’événement. Une source valide peut être n’importe quelle chaîne et doit représenter l’application ou le composant qui génère l’événement.|
|/t {erreur\|avertissement\|informations\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Spécifie le type d’événement à créer. Les types valides sont l’erreur, avertissement, INFORMATION, SUCCESSAUDIT et FAILUREAUDIT.|
|/ID \<EventID >|Spécifie l’ID d’événement pour l’événement. Un ID valide est n’importe quel nombre compris entre 1 et 1000.|
|/d \<Description>|Spécifie la description à utiliser pour l’événement nouvellement créé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Événements personnalisés ne peuvent pas être écrits dans le journal de sécurité.

## <a name="BKMK_examples"></a>Exemples

Les exemples suivants montrent comment vous pouvez utiliser la commande eventcreate :
```
eventcreate /t error /id 100 /l application /d "Create event in application log"
eventcreate /t information /id 1000 /so winmgmt /d "Create event in WinMgmt source"
eventcreate /t error /id 2001 /so winword /l application /d "new src Winword in application log"
eventcreate /s server /t error /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t error /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d "Remote machine with partial user credentials"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
