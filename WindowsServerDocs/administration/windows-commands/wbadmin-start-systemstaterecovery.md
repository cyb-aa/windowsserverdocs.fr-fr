---
title: Wbadmin start systemstaterecovery
description: La rubrique commandes Windows pour Wbadmin start systemstaterecovery, qui effectue une récupération de l’état du système vers un emplacement, et à partir d’une sauvegarde, que vous spécifiez.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 581ad6fe3591e549c3f89e4c95d2f8ab0cde059c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829492"
---
# <a name="wbadmin-start-systemstaterecovery"></a>Wbadmin start systemstaterecovery



Effectue une récupération de l’état du système vers un emplacement et à partir d’une sauvegarde que vous spécifiez.

> [!NOTE]
> Sauvegarde Windows Server ne sauvegarde pas ou ne récupère pas les ruches des utilisateurs du Registre (HKEY_CURRENT_USER) dans le cadre de la sauvegarde de l’état du système ou de la récupération de l’état du système.

Pour effectuer une récupération de l’état du système avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie l’identificateur de version de la sauvegarde à récupérer au format MM/JJ/AAAA-HH : MM. Si vous ne connaissez pas l’identificateur de version, tapez **Wbadmin obtenir des versions**.|
|-showsummary|Indique le résumé de la dernière récupération de l’état du système (après le redémarrage requis pour terminer l’opération). Ce paramètre ne peut pas être accompagné d’autres paramètres.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient la ou les sauvegardes à récupérer. Ce paramètre est utile lorsque l’emplacement de stockage est différent de celui dans lequel les sauvegardes de cet ordinateur sont généralement stockées.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez récupérer. Ce paramètre est utile lorsque plusieurs ordinateurs ont été sauvegardés au même emplacement. Doit être utilisé lorsque le paramètre **-backupTarget** est spécifié.|
|-recoveryTarget|Spécifie le répertoire dans lequel effectuer la restauration. Ce paramètre est utile si la sauvegarde est restaurée vers un autre emplacement.|
|-authsysvol|S’il est utilisé, effectue une restauration faisant autorité de SYSVOL (répertoire partagé du volume système).|
|-redémarrage|Spécifie de redémarrer le système à la fin de l’opération de récupération de l’état du système. Ce paramètre est valide uniquement pour une récupération à l’emplacement d’origine. Nous vous déconseillons d’utiliser ce paramètre si vous devez effectuer des étapes après l’opération de récupération.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

- Pour effectuer une récupération de l’état du système de la sauvegarde à partir de 03/31/2013 à 9:00 A.M., tapez :  
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```  
- Pour effectuer une récupération de l’état du système de la sauvegarde à partir de 04/30/2013 à 9:00 h 00 qui est stockée sur la ressource partagée \\\\servername\share pour Serveur01, tapez :  
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx)