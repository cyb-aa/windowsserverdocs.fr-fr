---
title: WBADMIN RESTORE CATALOG
description: Rubrique de référence pour WBADMIN RESTORE CATALOG, qui récupère un catalogue de sauvegarde de l’ordinateur local à partir d’un emplacement de stockage que vous spécifiez.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de9ce6b64f996e50fb85a8c612104bc6851ebdfd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720142"
---
# <a name="wbadmin-restore-catalog"></a>WBADMIN RESTORE CATALOG

Récupère un catalogue de sauvegarde de l’ordinateur local à partir d’un emplacement de stockage que vous spécifiez.

Pour récupérer un catalogue de sauvegarde avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-backupTarget|Spécifie l’emplacement du catalogue de sauvegarde du système tel qu’il était au moment où la sauvegarde a été créée.|
|-machine|Spécifie le nom de l’ordinateur pour lequel vous souhaitez récupérer le catalogue de sauvegarde. À utiliser lorsque les sauvegardes de plusieurs ordinateurs ont été stockées au même emplacement. Doit être utilisé lorsque **-backupTarget** est spécifié.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="remarks"></a>Notes 

Si l’emplacement (disque, DVD ou dossier partagé distant) dans lequel vous stockez vos sauvegardes est endommagé ou perdu et ne peut pas être utilisé pour restaurer le catalogue de sauvegarde, utilisez **Wbadmin Delete Catalog** pour supprimer le catalogue endommagé. Dans ce cas, vous devez créer une nouvelle sauvegarde une fois votre catalogue de sauvegarde supprimé.

## <a name="examples"></a>Exemples

Pour restaurer un catalogue à partir d’une sauvegarde stockée sur le disque d :, tapez :
```
wbadmin restore catalog -backupTarget:d
```
Pour restaurer un catalogue à partir d’une sauvegarde stockée dans \\ \\le dossier partagé servername\share de Serveur01, tapez :
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande Restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx)