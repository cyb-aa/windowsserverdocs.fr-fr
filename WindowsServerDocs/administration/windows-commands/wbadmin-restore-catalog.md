---
title: WBADMIN RESTORE CATALOG
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b0d646440ca9b30f9fa30fb1ac3ff08458b8e44d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362326"
---
# <a name="wbadmin-restore-catalog"></a>WBADMIN RESTORE CATALOG



Récupère un catalogue de sauvegarde de l’ordinateur local à partir d’un emplacement de stockage que vous spécifiez.

Pour récupérer un catalogue de sauvegarde avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples d’utilisation de cette sous-commande, consultez [exemples](#BKMK_examples).

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
|-backupTarget|Spécifie l’emplacement du catalogue de sauvegarde du système tel qu’il était au moment où la sauvegarde a été créée.|
|-machine|Spécifie le nom de l’ordinateur pour lequel vous souhaitez récupérer le catalogue de sauvegarde. À utiliser lorsque les sauvegardes de plusieurs ordinateurs ont été stockées au même emplacement. Doit être utilisé lorsque **-backupTarget** est spécifié.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="remarks"></a>Notes

Si l’emplacement (disque, DVD ou dossier partagé distant) dans lequel vous stockez vos sauvegardes est endommagé ou perdu et ne peut pas être utilisé pour restaurer le catalogue de sauvegarde, utilisez **Wbadmin Delete Catalog** pour supprimer le catalogue endommagé. Dans ce cas, vous devez créer une nouvelle sauvegarde une fois votre catalogue de sauvegarde supprimé.

## <a name="BKMK_examples"></a>Illustre

Pour restaurer un catalogue à partir d’une sauvegarde stockée sur le disque d :, tapez :
```
wbadmin restore catalog -backupTarget:d
```
Pour restaurer un catalogue à partir d’une sauvegarde stockée dans le dossier partagé \\\\servername\share de Serveur01, tapez :
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande Restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx)