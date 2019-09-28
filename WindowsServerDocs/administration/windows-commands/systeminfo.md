---
title: systeminfo
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32a84a33c5339e9949648a4e40d71daf25c055d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370693"
---
# <a name="systeminfo"></a>systeminfo



Affiche des informations de configuration détaillées sur un ordinateur et son système d’exploitation, notamment la configuration du système d’exploitation, les informations de sécurité, l’ID de produit et les propriétés matérielles (telles que la RAM, l’espace disque et les cartes réseau).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<Computer >|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u \<Domain > \<UserName >|Exécute la commande avec les autorisations de compte du compte d’utilisateur spécifié. Si l’option **/u** n’est pas spécifiée, cette commande utilise les autorisations de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande.|
|/p \<mot de passe >|Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .|
|/FO \<Format >|Spécifie le format de sortie avec l’une des valeurs suivantes :</br>TABLEAU Affiche la sortie dans une table.</br>TARIFS Affiche la sortie dans une liste.</br>VIRGULE Affiche la sortie au format de valeurs séparées par des virgules.|
|/NH|Supprime les en-têtes de colonnes dans la sortie. Valide lorsque le paramètre **/FO** est défini sur table ou CSV.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Illustre

Pour afficher les informations de configuration d’un ordinateur nommé Srvmain, tapez :

**SystemInfo/s Srvmain**

Pour afficher à distance les informations de configuration d’un ordinateur nommé Srvmain2 qui se trouve sur le domaine maindol, tapez :

**SystemInfo/s Srvmain2/u maindom\hiropln**

Pour afficher à distance les informations de configuration (sous forme de liste) pour un ordinateur nommé Srvmain2 qui se trouve sur le domaine maindol, tapez :

**SystemInfo/s Srvmain2/u maindom\hiropln/p p@ssW23/FO list**

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)