---
title: WBADMIN start sysrecovery
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e8e0ff114d09d70b9e50e8c4ea6af6330c74128c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847160"
---
# <a name="wbadmin-start-sysrecovery"></a>WBADMIN start sysrecovery



Effectue une récupération du système (récupération) en utilisant les paramètres que vous spécifiez.

> [!NOTE]
> La sous-commande peut être exécutée uniquement à partir de l’environnement de récupération Windows, et il n’est pas répertorié par défaut dans le texte de l’utilisation de **Wbadmin**. Pour plus d’informations, consultez [vue d’ensemble de l’environnement de récupération Windows (Windows RE)](https://technet.microsoft.com/library/hh825173.aspx).

Pour effectuer une récupération du système avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées.

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

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
|-version|Spécifie l’identificateur de version pour la sauvegarde à restaurer dans MM/jj/aaaa-format hh : mm. Si vous ne connaissez pas l’identificateur de version, tapez **wbadmin get versions**.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient l’ou les sauvegardes que vous souhaitez récupérer. Ce paramètre est utile lorsque l’emplacement de stockage est différent du où les sauvegardes de cet ordinateur sont généralement stockés.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez récupérer. Ce paramètre est utile lorsque plusieurs ordinateurs ont été sauvegardés dans le même emplacement. Doit être utilisé lorsque le **- backupTarget** est précisé.|
|-restoreAllVolumes|Récupère tous les volumes à partir de la sauvegarde sélectionnée. Si ce paramètre n’est pas spécifié, les volumes critiques uniquement (les volumes contenant les composants de système d’exploitation et d’état système) sont récupérés. Ce paramètre est utile lorsque vous avez besoin récupérer des volumes non critiques pendant la récupération du système.|
|-recreateDisks|Récupère une configuration de disque à l’état qui existe au moment où la sauvegarde a été créée.</br>Avertissement : Ce paramètre supprime toutes les données sur les volumes utilisés par des composants système d’exploitation hôte. Il peut également supprimer des données à partir de volumes de données.|
|-excludeDisks|Valide uniquement lorsqu’elle est spécifiée avec la **- recreateDisks** paramètre et doit être entrée sous forme de liste délimitée par des virgules d’identificateurs de disque (comme indiqué dans la sortie de **wbadmin get disques**). Disque exclu n’est pas partitionnées ou mis en forme. Ce paramètre permet de conserver les données sur les disques que vous ne souhaitez pas modifiées pendant l’opération de récupération.|
|-skipBadClusterCheck|Ignore la vérification de vos disques de récupération pour les informations de cluster défectueux. Si vous restaurez vers un autre serveur ou du matériel, nous recommandons que vous n’utilisez pas ce paramètre. Vous pouvez exécuter manuellement **chkdsk /b** sur vos disques de récupération à tout moment pour vérifier les clusters défectueux et puis mettez à jour les informations de système de fichiers en conséquence.</br>Avertissement : Jusqu'à ce que vous exécutiez **chkdsk** comme décrit, les clusters défectueux signalés sur votre système récupéré peuvent être inexact.|
|-quiet|Exécute la commande sans invite à l’utilisateur.|

## <a name="BKMK_examples"></a>Exemples

Pour commencer à récupérer les informations à partir de la sauvegarde a été exécutée sur le 31 mars 2013 à 9 h 00, situées sur le lecteur d:, type :
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
Pour commencer à récupérer les informations à partir de la sauvegarde a été exécutée sur le 30 avril 2013 à 9 h 00, situé dans le dossier partagé \\ \\servername\shared : pour server01, tapez :
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) applet de commande