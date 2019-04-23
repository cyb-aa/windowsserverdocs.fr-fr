---
title: systeminfo
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d08d3f86bdbd176aa4de157f58a58c9ea418470
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841500"
---
# <a name="systeminfo"></a>systeminfo



Affiche des informations de configuration sur un ordinateur et son système d’exploitation, y compris la configuration du système d’exploitation, les informations de sécurité, ID de produit et les propriétés de matériel (par exemple, la RAM, espace disque et les cartes réseau).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur >|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u \<domaine >\<nom d’utilisateur >|Exécute la commande avec les autorisations de compte du compte d’utilisateur spécifié. Si **/u** n’est pas spécifié, cette commande utilise les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande.|
|/p \<mot de passe >|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/Fo \<format >|Spécifie le format de sortie avec l’une des valeurs suivantes :</br>TABLE : Affiche la sortie dans une table.</br>LISTE : Affiche la sortie dans une liste.</br>CSV : Affiche la sortie au format de valeurs séparées par des virgules.|
|/nh|Supprime les en-têtes de colonne dans la sortie. Valide lorsque le **/fo** paramètre est défini sur la TABLE ou CSV.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher les informations de configuration pour un ordinateur nommé Srvmain, tapez :

**systeminfo /s srvmain**

Pour afficher à distance les informations de configuration pour un ordinateur nommé Srvmain2 qui se trouve sur le domaine Maindom, tapez :

**systeminfo /s srvmain2 /u maindom\hiropln**

Pour afficher à distance les informations de configuration (sous forme de liste) pour un ordinateur nommé Srvmain2 qui se trouve sur le domaine Maindom, tapez :

**systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list**

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)