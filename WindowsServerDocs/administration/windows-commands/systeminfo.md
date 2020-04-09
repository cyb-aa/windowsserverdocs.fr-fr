---
title: systeminfo
description: La rubrique commandes Windows pour SystemInfo, qui affiche des informations de configuration détaillées sur un ordinateur et son système d’exploitation, notamment la configuration du système d’exploitation, les informations de sécurité, l’ID de produit et les propriétés matérielles (telles que la RAM, l’espace disque et les cartes réseau).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41c2a499bc10f5b44f250958471b90f4b88dfede
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833572"
---
# <a name="systeminfo"></a>systeminfo

Affiche des informations de configuration détaillées sur un ordinateur et son système d’exploitation, notamment la configuration du système d’exploitation, les informations de sécurité, l’ID de produit et les propriétés matérielles (telles que la RAM, l’espace disque et les cartes réseau).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<> de l’ordinateur|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u \<domaine >\<nom d’utilisateur >|Exécute la commande avec les autorisations de compte du compte d’utilisateur spécifié. Si l’option **/u** n’est pas spécifiée, cette commande utilise les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande.|
|/p \<mot de passe >|Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .|
|/FO \<format >|Spécifie le format de sortie avec l’une des valeurs suivantes :</br>TABLE : affiche la sortie dans une table.</br>LIST : affiche la sortie dans une liste.</br>CSV : affiche la sortie au format de valeurs séparées par des virgules.|
|/NH|Supprime les en-têtes de colonnes dans la sortie. Valide lorsque le paramètre **/FO** est défini sur table ou CSV.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher les informations de configuration d’un ordinateur nommé Srvmain, tapez :

**SystemInfo/s Srvmain**

Pour afficher à distance les informations de configuration d’un ordinateur nommé Srvmain2 qui se trouve sur le domaine maindol, tapez :

**SystemInfo/s Srvmain2/u maindom\hiropln**

Pour afficher à distance les informations de configuration (sous forme de liste) pour un ordinateur nommé Srvmain2 qui se trouve sur le domaine maindol, tapez :

**SystemInfo/s Srvmain2/u maindom\hiropln/p p@ssW23/FO list**

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)