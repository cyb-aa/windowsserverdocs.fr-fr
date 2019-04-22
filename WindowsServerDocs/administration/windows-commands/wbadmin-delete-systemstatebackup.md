---
title: WBADMIN delete systemstatebackup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6801ca5985af626ccb7f6170fbcd6f8fc8305ba1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813150"
---
# <a name="wbadmin-delete-systemstatebackup"></a>WBADMIN delete systemstatebackup



Supprime les sauvegardes d’état système que vous spécifiez. Si le volume spécifié contient des sauvegardes autres que des sauvegardes d’état système de votre serveur local, ces sauvegardes ne sont pas supprimés.

> [!NOTE]
> Sauvegarde Windows Server ne pas sauvegarder ou récupérer les ruches utilisateur du Registre (HKEY_CURRENT_USER) dans le cadre de la sauvegarde de l’état système ou de récupération de l’état système.

Pour supprimer une sauvegarde de l’état système avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> Un seul et unique de ces paramètres doivent être spécifiés : **- keepVersions**, **-version**, ou **- deleteOldest**.

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-keepVersions|Spécifie le nombre des dernières sauvegardes d’état système à conserver. La valeur doit être un entier positif. La valeur du paramètre **- keepVersions : 0** supprime toutes les sauvegardes d’état système.|
|-version|Spécifie l’identificateur de version de la sauvegarde en MM/jj/aaaa-format hh : mm. Si vous ne connaissez pas l’identificateur de version, tapez **wbadmin get versions**.</br>Les versions qui sont exclusivement les sauvegardes d’état du système peuvent être supprimées à l’aide de cette commande. Utilisez **wbadmin obtenir les éléments** pour afficher le type de version.|
|-deleteOldest|Supprime la sauvegarde d’état système plus ancienne.|
|-backupTarget|Spécifie l’emplacement de stockage pour la sauvegarde que vous souhaitez supprimer. L’emplacement de stockage pour les sauvegardes de disques peut être une lettre de lecteur, un point de montage ou un chemin d’accès basé sur le GUID de volume. Cette valeur doit uniquement être spécifié pour la localisation des sauvegardes qui ne sont pas de l’ordinateur local. Informations sur les sauvegardes de l’ordinateur local seront disponibles dans le catalogue de sauvegarde sur l’ordinateur local.|
|-machine|Spécifie l’ordinateur dont vous souhaitez supprimer la sauvegarde état système. Utile lorsque plusieurs ordinateurs ont été sauvegardées au même emplacement. Doit être utilisé lorsque le **- backupTarget** est précisé.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="BKMK_examples"></a>Exemples

Pour supprimer la sauvegarde de l’état système créée sur le 31 mars 2013 à 10 h 00, tapez :
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Pour supprimer toutes les sauvegardes d’état système, sauf les trois derniers, tapez :
```
wbadmin delete systemstatebackup -keepVersions:3
```
Pour supprimer la sauvegarde d’état système plus ancien stockée sur le disque f, tapez :
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)