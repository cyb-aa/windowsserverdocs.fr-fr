---
title: Wbadmin obtient les éléments
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c3cc532381321655bbd3d5549b3c9b1896b9280
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362414"
---
# <a name="wbadmin-get-items"></a>Wbadmin obtient les éléments



Répertorie les éléments inclus dans une sauvegarde spécifique.

Pour utiliser cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes** , puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples d’utilisation de cette sous-commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie la version de la sauvegarde au format MM/JJ/AAAA-HH : MM. Si vous ne connaissez pas les informations de version, tapez **Wbadmin obtenir des versions**.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient les sauvegardes pour lesquelles vous souhaitez obtenir des détails. Utilisez pour répertorier les sauvegardes stockées à cet emplacement cible. Les emplacements des cibles de sauvegarde peuvent être un lecteur de disque connecté localement ou un dossier partagé distant. Si **Wbadmin obten items**est exécuté sur le même ordinateur que celui sur lequel la sauvegarde a été créée, ce paramètre n’est pas nécessaire. Toutefois, ce paramètre est requis pour obtenir des informations sur une sauvegarde créée à partir d’un autre ordinateur.|
|-machine|Spécifie le nom de l’ordinateur pour lequel vous souhaitez obtenir les détails de la sauvegarde. Utile lorsque plusieurs ordinateurs ont été sauvegardés au même emplacement. Doit être utilisé lorsque **-backupTarget** est spécifié.|

## <a name="BKMK_examples"></a>Illustre

Pour répertorier les éléments de la sauvegarde qui ont été exécutés le 31 mars 2013 à 9:00 A.M., tapez :
```
wbadmin get items -version:03/31/2013-09:00
```
Pour répertorier les éléments de la sauvegarde de SERVEUR01 qui a été exécutée le 30 avril 2013 à 9:00 h 00 et stockés sur \\\\servername\share, tapez :
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx)