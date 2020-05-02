---
title: Wbadmin Delete systemstatebackup
description: Rubrique de référence pour Wbadmin Delete systemstatebackup, qui supprime les sauvegardes de l’état du système que vous spécifiez.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12d8ba6ff24e338c6afa5556d7a60e2157156acc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720198"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin Delete systemstatebackup



Supprime les sauvegardes de l’état du système que vous spécifiez. Si le volume spécifié contient des sauvegardes autres que les sauvegardes de l’état du système de votre serveur local, ces sauvegardes ne seront pas supprimées.

> [!NOTE]
> Sauvegarde Windows Server ne sauvegarde pas ou ne récupère pas les ruches des utilisateurs du Registre (HKEY_CURRENT_USER) dans le cadre de la sauvegarde de l’état du système ou de la récupération de l’état du système.

Pour supprimer une sauvegarde de l’état du système avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)



## <a name="syntax"></a>Syntaxe

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> Un et un seul de ces paramètres doivent être spécifiés : **-keepVersions**, **-version**ou **-deleteOldest**.

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-keepVersions|Spécifie le nombre de sauvegardes de l’état du système les plus récentes à conserver. Cette valeur doit être un entier positif. La valeur du paramètre **-keepVersions : 0** supprime toutes les sauvegardes de l’état du système.|
|-version|Spécifie l’identificateur de version de la sauvegarde au format MM/JJ/AAAA-HH : MM. Si vous ne connaissez pas l’identificateur de version, tapez **Wbadmin obtenir des versions**.</br>Les versions qui sont exclusivement des sauvegardes de l’état du système peuvent être supprimées à l’aide de cette commande. Utilisez **Wbadmin obtenir des éléments** pour afficher le type de version.|
|-deleteOldest|Supprime la sauvegarde de l’état du système la plus ancienne.|
|-backupTarget|Spécifie l’emplacement de stockage de la sauvegarde que vous souhaitez supprimer. L’emplacement de stockage des sauvegardes de disques peut être une lettre de lecteur, un point de montage ou un chemin d’accès de volume basé sur un GUID. Cette valeur doit être spécifiée uniquement pour la recherche de sauvegardes qui ne sont pas de l’ordinateur local. Les informations sur les sauvegardes de l’ordinateur local sont disponibles dans le catalogue de sauvegarde de l’ordinateur local.|
|-machine|Spécifie l’ordinateur dont vous souhaitez supprimer la sauvegarde de l’état du système. Utile lorsque plusieurs ordinateurs ont été sauvegardés au même emplacement. Doit être utilisé lorsque le paramètre **-backupTarget** est spécifié.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="examples"></a>Exemples

Pour supprimer la sauvegarde de l’état du système créée le 31 mars 2013 à 10:00 AM, tapez :
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Pour supprimer toutes les sauvegardes de l’état du système, à l’exception des trois types les plus récents :
```
wbadmin delete systemstatebackup -keepVersions:3
```
Pour supprimer la sauvegarde de l’état du système la plus ancienne stockée sur le disque f, tapez :
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)