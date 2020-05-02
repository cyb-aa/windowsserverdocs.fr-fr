---
title: versions d’extraction Wbadmin
description: Rubrique de référence pour Wbadmin obtenir des versions, qui répertorie des détails sur les sauvegardes disponibles stockées sur l’ordinateur local ou sur un autre ordinateur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 542f65b7d87eacb102f64fb4103e6c684df4faa5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720153"
---
# <a name="wbadmin-get-versions"></a>versions d’extraction Wbadmin



Répertorie des informations sur les sauvegardes disponibles qui sont stockées sur l’ordinateur local ou sur un autre ordinateur. Lorsque cette sous-commande est utilisée sans paramètres, elle répertorie toutes les sauvegardes de l’ordinateur local, même si ces sauvegardes ne sont pas disponibles. Les détails fournis pour une sauvegarde incluent l’heure de la sauvegarde, l’emplacement de stockage de la sauvegarde, l’identificateur de version (nécessaire pour la sous-commande **Wbadmin obtenir des éléments** et pour effectuer des récupérations), ainsi que le type de récupération que vous pouvez effectuer.

Pour obtenir des détails sur les sauvegardes disponibles à l’aide de cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou les autorisations appropriées doivent vous avoir été déléguées. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de **commandes** avec élévation de privilèges, puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-backupTarget|Spécifie l’emplacement de stockage qui contient les sauvegardes pour lesquelles vous souhaitez obtenir des détails. Utilisez pour répertorier les sauvegardes stockées à cet emplacement cible. Les emplacements des cibles de sauvegarde peuvent être des lecteurs de disque, des volumes, des dossiers partagés distants, des supports amovibles, tels que des lecteurs de DVD ou d’autres supports optiques. Si **Wbadmin Run versions** est exécuté sur le même ordinateur que celui sur lequel la sauvegarde a été créée, ce paramètre n’est pas nécessaire. Toutefois, ce paramètre est requis pour obtenir des informations sur une sauvegarde créée à partir d’un autre ordinateur.|
|-machine|Spécifie l’ordinateur pour lequel vous souhaitez obtenir les détails de la sauvegarde. À utiliser lorsque les sauvegardes de plusieurs ordinateurs sont stockées au même emplacement. Doit être utilisé lorsque **-backupTarget** est spécifié.|

## <a name="remarks"></a>Notes 

Pour répertorier les éléments disponibles pour la récupération à partir d’une sauvegarde spécifique, utilisez **Wbadmin obtenir des éléments**.

## <a name="examples"></a>Exemples

Pour afficher la liste des sauvegardes disponibles stockées sur le volume h, tapez :
```
wbadmin get versions -backupTarget:h:
```
Pour afficher la liste des sauvegardes disponibles stockées dans le dossier \\ \\partagé distant servername\share pour l’ordinateur Serveur01, tapez :
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx)