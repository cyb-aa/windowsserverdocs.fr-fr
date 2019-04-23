---
title: catalogue de restauration WBADMIN
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5876a44b178025baac7ee5901cdc32c1b5d33dad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851710"
---
# <a name="wbadmin-restore-catalog"></a>catalogue de restauration WBADMIN



Récupère un catalogue de sauvegarde pour l’ordinateur local à partir d’un emplacement de stockage que vous spécifiez.

Pour récupérer un catalogue de sauvegarde avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-backupTarget|Spécifie l’emplacement du catalogue de sauvegarde du système telle qu’elle était au point une fois la sauvegarde a été créée.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez récupérer le catalogue de sauvegarde pour. À utiliser lorsque les sauvegardes pour plusieurs ordinateurs ont été stockés au même emplacement. Doit être utilisé lorsque **- backupTarget** est spécifié.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="remarks"></a>Notes

Si l’emplacement où vous stockez vos sauvegardes (disque, un DVD ou un dossier partagé distant) est endommagée ou perdue et ne peut pas être utilisé pour restaurer le catalogue de sauvegarde, utilisez **wbadmin delete catalogue** pour supprimer le catalogue endommagé. Dans ce cas, vous devez créer une nouvelle sauvegarde une fois que votre catalogue de sauvegarde est supprimé.

## <a name="BKMK_examples"></a>Exemples

Pour restaurer un catalogue à partir d’une sauvegarde stockée sur le disque d:, tapez :
```
wbadmin restore catalog -backupTarget:d
```
Pour restaurer un catalogue à partir d’une sauvegarde stockée dans le dossier partagé \\ \\servername\share de server01, type :
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Restauration-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) applet de commande