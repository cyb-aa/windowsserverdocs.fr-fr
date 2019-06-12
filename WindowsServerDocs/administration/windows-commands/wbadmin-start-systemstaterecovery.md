---
title: WBADMIN start systemstaterecovery
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4282da2011c39daec0315a7f3836d5517f29debb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440207"
---
# <a name="wbadmin-start-systemstaterecovery"></a>WBADMIN start systemstaterecovery



Effectue une récupération de l’état système vers un emplacement et à partir d’une sauvegarde que vous spécifiez.

> [!NOTE]
> Sauvegarde Windows Server ne pas sauvegarder ou récupérer les ruches utilisateur du Registre (HKEY_CURRENT_USER) dans le cadre de la sauvegarde de l’état système ou de récupération de l’état système.

Pour effectuer une récupération de l’état système avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

Syntaxe pour Windows Server 2008 :
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
Syntaxe pour Windows Server 2008 R2 ou version ultérieure :
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie l’identificateur de version pour la sauvegarde à restaurer dans MM/jj/aaaa-format hh : mm. Si vous ne connaissez pas l’identificateur de version, tapez **wbadmin get versions**.|
|-showsummary|Signale le résumé de la dernière récupération de l’état système (après le redémarrage est nécessaire pour terminer l’opération). Ce paramètre ne peut pas être accompagné de tous les autres paramètres.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient l’ou les sauvegardes que vous souhaitez récupérer. Ce paramètre est utile lorsque l’emplacement de stockage est différent du où les sauvegardes de cet ordinateur sont généralement stockés.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez récupérer. Ce paramètre est utile lorsque plusieurs ordinateurs ont été sauvegardés dans le même emplacement. Doit être utilisé lorsque le **- backupTarget** est précisé.|
|-recoveryTarget|Spécifie le répertoire à restaurer. Ce paramètre est utile si la sauvegarde est restaurée vers un autre emplacement.|
|-authsysvol|Si vous, effectue une restauration faisant autorité de SYSVOL (le répertoire partagé du Volume système).|
|-autoReboot|Spécifie pour redémarrer le système à la fin de l’opération de récupération d’état système. Ce paramètre est valide uniquement pour une récupération à l’emplacement d’origine. Nous vous déconseillons de que vous utilisez ce paramètre si vous avez besoin effectuer les étapes après l’opération de récupération.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="BKMK_examples"></a>Exemples

- Pour effectuer une récupération de l’état système de la sauvegarde à partir du 31/03/2013 à 9 h 00, tapez :  
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```  
- Pour effectuer une récupération de l’état système de la sauvegarde à partir du 30/04/2013 à 9 h 00 qui est stocké sur la ressource partagée \\ \\servername\share pour server01, type :  
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) applet de commande