---
title: bootcfg query
description: Rubrique de commandes de Windows pour **bootcfg requête** -requêtes et affiche le [chargeur de démarrage] et [systèmes d’exploitation] section entrées à partir du fichier Boot.ini.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4cacfd1-10a6-4a11-b0c5-f8abde72bfc8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6886fbdfe541b39e530d2b7f71d4fc40909a2a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862930"
---
# <a name="bootcfg-query"></a>bootcfg query

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Interroge et affiche le [chargeur de démarrage] et les entrées de Boot.ini de section [operating systems].

## <a name="syntax"></a>Syntaxe
```
bootcfg /query [/s <computer> [/u <Domain>\<User> /p <Password>]]
```
## <a name="parameters"></a>Paramètres
|Terme|Définition|
|----|-------|
|/s <computer>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u <Domain>\\<User>|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User>ou <Domain> \\ <User>. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p <Password>|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/?|Affiche l'aide à l'invite de commandes.|
##### <a name="remarks"></a>Notes
-   Voici un exemple de **bootcfg /query** sortie :
    ```
    Boot Loader Settings
    ----------
    timeout: 30
    default: multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    Boot Entries
    ------
    Boot entry ID:   1
    Friendly Name:   ""
    path:            multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    OS Load Options: /fastdetect /debug /debugport=com1:
    ```
-   La partie paramètres du chargeur de démarrage de la **bootcfg requête** sortie affiche chaque entrée dans la section [boot loader] du fichier Boot.ini.
-   La partie des entrées de démarrage de la **bootcfg requête** sortie affiche les informations suivantes pour chaque entrée de système d’exploitation dans la section [operating systems] du fichier Boot.ini : ID d’entrée de démarrage, nom convivial, chemin d’accès et les Options de chargement du système d’exploitation.
## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /query** commande :
```
bootcfg /query
bootcfg /query /s srvmain /u maindom\hiropln /p p@ssW23
bootcfg /query /u hiropln /p p@ssW23
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
