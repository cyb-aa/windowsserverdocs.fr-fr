---
title: getmac
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e356354e63a057201582db0fb74933e1b3ef8d8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851040"
---
# <a name="getmac"></a>getmac

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Retourne le support d’accès (adresse MAC control) et la liste des protocoles réseau associés à chaque adresse pour toutes les cartes réseau de chaque ordinateur, soit localement ou sur un réseau. 
## <a name="syntax"></a>Syntaxe
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/s <computer>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u <Domain>\\<User>|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par l’utilisateur ou domaine\utilisateur. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p <Password>|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/fo { TABLE &#124; list&#124; CSV}|Spécifie le format à utiliser pour la sortie de requête. Les valeurs valides sont **TABLE**, **liste**, et **CSV**. Le format par défaut pour la sortie est **TABLE**.|
|/nh|Supprime l’en-tête de colonne dans la sortie. Valide lorsque le **/fo** paramètre est défini sur **TABLE** ou **CSV**.|
|/v|Spécifie que la sortie affiche des informations détaillées.|
|/?||
## <a name="remarks"></a>Notes
**GETMAC** peut être utile lorsque vous souhaitez entrer l’adresse MAC dans un analyseur réseau ou lorsque vous avez besoin de savoir quels protocoles sont actuellement en cours d’utilisation sur chaque carte réseau d’un ordinateur.
## <a name="BKMK_Examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **getmac** commande :
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
