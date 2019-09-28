---
title: whoami
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9731ba3be3983eb53ade88fceaee863800229084
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362143"
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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/upn|Affiche le nom d’utilisateur au format UPN (user principal name).|
|/fqdn|Affiche le nom d’utilisateur au format de nom de domaine complet (FQDN).|
|/logonid|Affiche l’ID d’ouverture de session de l’utilisateur actuel.|
|/User|Affiche le domaine et le nom d’utilisateur actuels et l’identificateur de sécurité (SID).|
|/groups|Affiche les groupes d’utilisateurs auxquels appartient l’utilisateur actuel.|
|/priv|Affiche les privilèges de sécurité de l’utilisateur actuel.|
|/FO \<Format >|Spécifie le format de sortie. Les valeurs valides sont les suivantes:</br>**table** Affiche la sortie dans une table. Valeur par défaut.</br>**liste** Affiche la sortie dans une liste.</br>fichier **CSV** Affiche la sortie au format CSV (valeurs séparées par des virgules).|
|All|Affiche toutes les informations du jeton d’accès actuel, y compris le nom d’utilisateur actuel, les identificateurs de sécurité (SID), les privilèges et les groupes auxquels l’utilisateur actuel appartient.|
|/NH|Spécifie que l’en-tête de colonne ne doit pas être affiché dans la sortie. Cela est valide uniquement pour les formats table et CSV.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Illustre

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

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)