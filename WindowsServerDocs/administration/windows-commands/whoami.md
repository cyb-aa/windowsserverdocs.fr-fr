---
title: whoami
description: La rubrique commandes Windows pour whoami, qui affiche des informations d’utilisateur, de groupe et de privilèges pour l’utilisateur actuellement connecté au système local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ff45ed95b35215859f2f83aec75b33570ef46d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829272"
---
# <a name="whoami"></a>whoami



Affiche des informations sur l’utilisateur, le groupe et les privilèges de l’utilisateur actuellement connecté au système local. En cas d’utilisation sans paramètre, **whoami** affiche le domaine et le nom d’utilisateur actuels.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/upn|Affiche le nom d’utilisateur au format UPN (user principal name).|
|/fqdn|Affiche le nom d’utilisateur au format de nom de domaine complet (FQDN).|
|/logonid|Affiche l’ID d’ouverture de session de l’utilisateur actuel.|
|/User|Affiche le domaine et le nom d’utilisateur actuels et l’identificateur de sécurité (SID).|
|/groups|Affiche les groupes d’utilisateurs auxquels appartient l’utilisateur actuel.|
|/priv|Affiche les privilèges de sécurité de l’utilisateur actuel.|
|/FO \<format >|Spécifie le format de sortie. Les valeurs valides sont les suivantes :</br>**table** Affiche la sortie dans une table. Il s'agit de la valeur par défaut.</br>**liste** Affiche la sortie dans une liste.</br>fichier **CSV** Affiche la sortie au format CSV (valeurs séparées par des virgules).|
|/all|Affiche toutes les informations du jeton d’accès actuel, y compris le nom d’utilisateur actuel, les identificateurs de sécurité (SID), les privilèges et les groupes auxquels l’utilisateur actuel appartient.|
|/NH|Spécifie que l’en-tête de colonne ne doit pas être affiché dans la sortie. Cela est valide uniquement pour les formats table et CSV.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher le domaine et le nom d’utilisateur de la personne qui est actuellement connectée à cet ordinateur, tapez :
```
whoami
```
Une sortie similaire à ce qui suit s’affiche :
```
DOMAIN1\administrator
```
Pour afficher toutes les informations du jeton d’accès actuel, tapez :
```
whoami /all
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)