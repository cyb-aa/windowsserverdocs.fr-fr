---
title: WBADMIN get versions
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4ebbd0d78de0ffbff1ee8c658d6d9811b87df1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813530"
---
# <a name="wbadmin-get-versions"></a>WBADMIN get versions



Fournit des détails sur les sauvegardes disponibles qui sont stockés sur l’ordinateur local ou un autre ordinateur. Lorsque cette sous-commande est utilisée sans paramètre, il répertorie toutes les sauvegardes de l’ordinateur local, même si ces sauvegardes ne sont pas disponibles. Les détails fournis pour une sauvegarde incluent l’heure de sauvegarde, l’emplacement de stockage de sauvegarde, l’identificateur de version (nécessaire pour la **wbadmin obtenir les éléments** sous-commande et effectuer des récupérations) et le type de récupérations vous pouvez effectuer.

Pour obtenir plus d’informations sur les sauvegardes disponibles à l’aide de la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été déléguée approprié autorisations. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges **invite de commandes** puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-backupTarget|Spécifie l’emplacement de stockage contenant les sauvegardes que vous souhaitez que les détails de. Utilisation pour répertorier les sauvegardes stockées à cet emplacement cible. Emplacements de cible de sauvegarde peuvent être des lecteurs de disque connectés localement, volumes, des dossiers partagés distants, les supports amovibles tels que les lecteurs de DVD ou autre support optique. Si **wbadmin get versions** est exécuté sur le même ordinateur où la sauvegarde a été créée, ce paramètre n’est pas nécessaire. Toutefois, ce paramètre est obligatoire pour obtenir des informations sur une sauvegarde créée à partir d’un autre ordinateur.|
|-machine|Spécifie l’ordinateur que vous souhaitez que les détails de la sauvegarde pour. À utiliser lorsque les sauvegardes de plusieurs ordinateurs sont stockées dans le même emplacement. Doit être utilisé lorsque **- backupTarget** est spécifié.|

## <a name="remarks"></a>Notes

Pour répertorier les éléments disponibles pour la récupération à partir d’une sauvegarde spécifique, utilisez **wbadmin obtenir les éléments**.

## <a name="BKMK_examples"></a>Exemples

Pour afficher la liste des sauvegardes disponibles qui sont stockés sur le volume h, tapez :
```
wbadmin get versions -backupTarget:h:
```
Pour afficher la liste des sauvegardes disponibles qui sont stockés dans le dossier partagé distant \\ \\servername\share pour l’ordinateur server01, type :
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx) applet de commande