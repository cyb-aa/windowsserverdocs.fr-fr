---
title: Wbadmin obtient les éléments
description: Rubrique relative aux commandes Windows pour Wbadmin obtenir des éléments, qui répertorie les éléments inclus dans une sauvegarde spécifique.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 781057bb450ab4b692c793f67d807e008efcf5d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829752"
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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie la version de la sauvegarde au format MM/JJ/AAAA-HH : MM. Si vous ne connaissez pas les informations de version, tapez **Wbadmin obtenir des versions**.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient les sauvegardes pour lesquelles vous souhaitez obtenir des détails. Utilisez pour répertorier les sauvegardes stockées à cet emplacement cible. Les emplacements des cibles de sauvegarde peuvent être un lecteur de disque connecté localement ou un dossier partagé distant. Si **Wbadmin obten items**est exécuté sur le même ordinateur que celui sur lequel la sauvegarde a été créée, ce paramètre n’est pas nécessaire. Toutefois, ce paramètre est requis pour obtenir des informations sur une sauvegarde créée à partir d’un autre ordinateur.|
|-machine|Spécifie le nom de l’ordinateur pour lequel vous souhaitez obtenir les détails de la sauvegarde. Utile lorsque plusieurs ordinateurs ont été sauvegardés au même emplacement. Doit être utilisé lorsque **-backupTarget** est spécifié.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour répertorier les éléments de la sauvegarde qui ont été exécutés le 31 mars 2013 à 9:00 A.M., tapez :
```
wbadmin get items -version:03/31/2013-09:00
```
Pour répertorier les éléments de la sauvegarde de SERVEUR01 qui a été exécutée le 30 avril 2013 à 9:00 h 00 et stockés sur \\\\servername\share, tapez :
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx)