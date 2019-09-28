---
title: Wbadmin start SYSRECOVERY
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c653fa52a2a56267d6f0df169f8f9924f2aa94d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362280"
---
# <a name="wbadmin-start-sysrecovery"></a>Wbadmin start SYSRECOVERY



Effectue une récupération système (récupération complète) à l’aide des paramètres que vous spécifiez.

> [!NOTE]
> Cette sous-commande ne peut être exécutée qu’à partir de l’environnement de récupération Windows et n’est pas listée par défaut dans le texte d’utilisation de **Wbadmin**. Pour plus d’informations, consultez [vue d’ensemble de l’environnement de récupération Windows (Windows RE)](https://technet.microsoft.com/library/hh825173.aspx).

Pour effectuer une récupération du système avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée.

Pour obtenir des exemples d’utilisation de cette sous-commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie l’identificateur de version de la sauvegarde à récupérer au format MM/JJ/AAAA-HH : MM. Si vous ne connaissez pas l’identificateur de version, tapez **Wbadmin obtenir des versions**.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient la ou les sauvegardes que vous souhaitez récupérer. Ce paramètre est utile lorsque l’emplacement de stockage est différent de celui où les sauvegardes de cet ordinateur sont généralement stockées.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez récupérer. Ce paramètre est utile lorsque plusieurs ordinateurs ont été sauvegardés au même emplacement. Doit être utilisé lorsque le paramètre **-backupTarget** est spécifié.|
|-restoreAllVolumes|Récupère tous les volumes de la sauvegarde sélectionnée. Si ce paramètre n’est pas spécifié, seuls les volumes critiques (volumes qui contiennent l’état du système et les composants du système d’exploitation) sont récupérés. Ce paramètre est utile lorsque vous devez récupérer des volumes non critiques pendant la récupération du système.|
|-recreateDisks|Récupère une configuration de disque à l’État qui existait lors de la création de la sauvegarde.</br>Avertissement : Ce paramètre supprime toutes les données sur les volumes qui hébergent les composants du système d’exploitation. Il peut également supprimer des données des volumes de données.|
|-excludeDisks|Valide uniquement lorsqu’il est spécifié avec le paramètre **-recreateDisks** et doit être entré sous la forme d’une liste d’identificateurs de disque délimités par des virgules (comme indiqué dans la sortie de **Wbadmin obtenir des disques**). Les disques exclus ne sont pas partitionnés ou formatés. Ce paramètre permet de conserver les données sur les disques que vous ne souhaitez pas modifier pendant l’opération de récupération.|
|-skipBadClusterCheck|Ignore la vérification de la présence d’informations de cluster erronées sur vos disques de récupération. Si vous effectuez une restauration sur un autre serveur ou matériel, nous vous recommandons de ne pas utiliser ce paramètre. Vous pouvez exécuter manuellement **chkdsk/b** sur vos disques de récupération à tout moment pour les vérifier pour les clusters défectueux, puis mettre à jour les informations du système de fichiers en conséquence.</br>Avertissement : Tant que vous n’exécutez pas **chkdsk** comme décrit, les clusters incorrects signalés sur votre système récupéré peuvent ne pas être exacts.|
|-quiet|Exécute la commande sans invite à l’utilisateur.|

## <a name="BKMK_examples"></a>Illustre

Pour démarrer la récupération des informations à partir de la sauvegarde qui a été exécutée le 31 mars 2013 à 9:00 A.M., situé sur le lecteur d :, tapez :
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Pour démarrer la récupération des informations à partir de la sauvegarde qui a été exécutée le 30 avril 2013 à 9:00 A.M., situé dans le dossier partagé \\ @ no__t-1servername\shared : pour Serveur01, tapez :
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx)