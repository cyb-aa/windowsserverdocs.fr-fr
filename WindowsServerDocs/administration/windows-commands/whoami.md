---
title: whoami
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6844ba001c2ebd7407b77f97204069a48a1b595b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840150"
---
# <a name="whoami"></a>whoami



Affiche des informations utilisateur, groupe et les privilèges de l’utilisateur actuellement connecté au système local. Si utilisée sans paramètres, **whoami** affiche le nom de domaine et d’utilisateur actuel.

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
|/upn|Affiche le nom d’utilisateur dans le format nom utilisateur principal (UPN).|
|/fqdn|Affiche le nom d’utilisateur au format nom de domaine complet (FQDN).|
|/logonid|Affiche l’ID d’ouverture de session de l’utilisateur actuel.|
|/User|Affiche le nom de domaine et d’utilisateur actuel et l’identificateur de sécurité (SID).|
|/Groups|Affiche les groupes d’utilisateurs à laquelle appartient l’utilisateur actuel.|
|/priv|Affiche les privilèges de sécurité de l’utilisateur actuel.|
|/Fo \<format >|Spécifie le format de sortie. Les valeurs valides sont les suivantes :</br>**table** affiche la sortie dans une table. Valeur par défaut.</br>**liste** affiche la sortie dans une liste.</br>**CSV** affiche la sortie au format de valeurs séparées par des virgules (CSV).|
|/all|Affiche toutes les informations dans le jeton d’accès actuel, y compris le nom d’utilisateur actuel, les identificateurs de sécurité (SID), les privilèges et les groupes auxquels appartient l’utilisateur actuel.|
|/nh|Spécifie que l’en-tête de colonne ne doit pas être affiché dans la sortie. Cela est valide uniquement pour les formats CSV et de table.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher le nom de domaine et d’utilisateur de la personne qui est actuellement connecté à cet ordinateur, tapez :
```
whoami
```
Une sortie similaire à ce qui suit apparaît :
```
DOMAIN1\administrator
```
Pour afficher toutes les informations dans le jeton d’accès actuel, tapez :
```
whoami /all
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)