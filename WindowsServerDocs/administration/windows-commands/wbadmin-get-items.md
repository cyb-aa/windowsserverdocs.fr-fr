---
title: WBADMIN get éléments
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: eeb7c29ff552f968b4785612f626a86baf154ad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842880"
---
# <a name="wbadmin-get-items"></a>WBADMIN get éléments



Répertorie les éléments inclus dans une sauvegarde spécifique.

Pour utiliser cette sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes** puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

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
|-version|Spécifie la version de la sauvegarde en MM/jj/aaaa-format hh : mm. Si vous ne connaissez pas les informations de version, tapez **wbadmin get versions**.|
|-backupTarget|Spécifie l’emplacement de stockage contenant les sauvegardes dont vous voulez les détails. Utilisation pour répertorier les sauvegardes stockées à cet emplacement cible. Emplacements de cible de sauvegarde peuvent être un lecteur de disque attaché localement ou un dossier partagé distant. Si **wbadmin obtenir les éléments**est exécuté sur le même ordinateur où la sauvegarde a été créée, ce paramètre n’est pas nécessaire. Toutefois, ce paramètre est obligatoire pour obtenir des informations sur une sauvegarde créée à partir d’un autre ordinateur.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez les informations de sauvegarde pour. Utile lorsque plusieurs ordinateurs ont été sauvegardés dans le même emplacement. Doit être utilisé lorsque **- backupTarget** est spécifié.|

## <a name="BKMK_examples"></a>Exemples

Pour répertorier les éléments à partir de la sauvegarde a été exécutée sur le 31 mars 2013 à 9 h 00, type :
```
wbadmin get items -version:03/31/2013-09:00
```
Pour répertorier les éléments à partir de la sauvegarde de server01 qui a été exécuté sur le 30 avril 2013 à 9 h 00 et stocké dans \\ \\servername\share, type :
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) applet de commande