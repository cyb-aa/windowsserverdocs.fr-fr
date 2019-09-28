---
title: Wbadmin activer la sauvegarde
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d30136f7eb3ea48b8d9b0a740eb5a77e981ae3f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362381"
---
# <a name="wbadmin-enable-backup"></a>Wbadmin activer la sauvegarde



Crée et active une planification de sauvegarde quotidienne ou modifie une planification de sauvegarde existante. Si aucun paramètre n’est spécifié, les paramètres de sauvegarde actuellement planifiés sont affichés.

Pour configurer ou modifier une planification de sauvegarde quotidienne, vous devez être membre du groupe **administrateurs** ou **opérateurs de sauvegarde** . En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes** , puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples d’utilisation de cette sous-commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

Syntaxe pour Windows Server 2008 :
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
Syntaxe pour Windows Server 2008 R2 :
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-allCritical]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
```
Syntaxe pour Windows Server 2012 et Windows Server 2012 R2 :
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-hyperv:<HyperVComponentsToExclude>]
[-allCritical]
[-systemState] 
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet] 
[-allowDeleteOldBackups]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-addtarget|Pour Windows Server 2008, spécifie l’emplacement de stockage pour les sauvegardes. Vous oblige à spécifier une destination pour les sauvegardes en tant qu’identificateur de disque (consultez la section Notes). Le disque est formaté avant d’être utilisé, et toutes les données existantes qu’il contient sont effacées définitivement.</br>Pour Windows Server 2008 R2 et versions ultérieures, spécifie l’emplacement de stockage pour les sauvegardes. Exige que vous spécifiiez l’emplacement en tant que chemin d’accès de disque, de volume ou UNC (Universal Naming Convention) à\\un dossier\<partagé distant\)(\\\<servername > nom_partage >. Par défaut, la sauvegarde est enregistrée à l’adresse \\suivante : \< \\ <servername>nom_partage\<> \WindowsImageBackup ComputerBackedUp >\. Si vous spécifiez un disque, le disque est formaté avant utilisation et toutes les données existantes qu’il contient sont effacées définitivement. Si vous spécifiez un dossier partagé, vous ne pouvez pas ajouter d’autres emplacements. Vous ne pouvez spécifier qu’un dossier partagé en tant qu’emplacement de stockage à la fois.</br>Important : Si vous enregistrez une sauvegarde dans un dossier partagé distant, cette sauvegarde sera remplacée si vous utilisez le même dossier pour sauvegarder à nouveau le même ordinateur. En outre, si l’opération de sauvegarde échoue, vous risquez de vous retrouver sans sauvegarde, car l’ancienne sauvegarde sera remplacée, mais la sauvegarde la plus récente ne sera pas utilisable. Vous pouvez éviter cela en créant des sous-dossiers dans le dossier partagé distant pour organiser vos sauvegardes. Si vous procédez ainsi, les sous-dossiers auront besoin de deux fois plus d’espace que le dossier parent.</br>Un seul emplacement peut être spécifié dans une seule commande. Vous pouvez ajouter plusieurs emplacements de stockage des sauvegardes de volumes et des disques en exécutant à nouveau la commande.|
|-removetarget|Spécifie l’emplacement de stockage que vous souhaitez supprimer de la planification de sauvegarde existante. Requiert que vous spécifiiez l’emplacement en tant qu’identificateur de disque (consultez la section Notes).|
|-planification|Spécifie les heures de la journée pour créer une sauvegarde, au format HH : MM et délimité par des virgules.|
|-inclure|Pour Windows Server 2008, spécifie la liste, délimitée par des virgules, des lettres de lecteur de volume, des points de montage de volume ou des noms de volumes basés sur le GUID à inclure dans la sauvegarde.</br>Pour Windows Server 2008 R2and plus tard, spécifie la liste délimitée par des virgules des éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\)oblique inverse (. Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier.|
|-nonRecurseInclude|Pour Windows Server 2008 R2 et versions ultérieures, spécifie la liste non récursive, délimitée par des virgules, des éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\)oblique inverse (. Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le paramètre-backupTarget est utilisé.|
|-Exclude|Pour Windows Server 2008 R2 et versions ultérieures, spécifie la liste délimitée par des virgules des éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, des dossiers ou des volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\)oblique inverse (. Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier.|
|-nonRecurseExclude|Pour Windows Server 2008 R2 et versions ultérieures, spécifie la liste non récursive, délimitée par des virgules, des éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, des dossiers ou des volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom de volume basé sur le GUID, il doit se terminer par une barre\)oblique inverse (. Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lors de la spécification d’un chemin d’accès à un fichier.|
|-HyperV|Spécifie la liste délimitée par des virgules des composants à inclure dans la sauvegarde. L’identificateur peut être un nom de composant ou un GUID de composant (avec ou sans accolades).|
|-systemState|Pour Windows ° 7 et Windows Server 2008 R2 et versions ultérieures, crée une sauvegarde qui inclut l’état du système en plus de tous les autres éléments que vous avez spécifiés avec le paramètre **-include** . L’état du système contient les fichiers de démarrage (Boot. ini, NDTLDR, NTDetect.com), le Registre Windows, y compris les paramètres COM, le SYSVOL (stratégies de groupe et scripts d’ouverture de session), le Active Directory et NTDS. DIT sur les contrôleurs de domaine et, si le service de certificats est installé, le magasin de certificats. Si le rôle de serveur Web est installé sur votre serveur, le méta-annuaire IIS sera inclus. Si le serveur fait partie d’un cluster, les informations de service de cluster sont également incluses.|
|-allCritical|Spécifie que tous les volumes critiques (volumes qui contiennent l’état du système d’exploitation) sont inclus dans les sauvegardes. Ce paramètre est utile si vous créez une sauvegarde pour la récupération complète du système ou de l’état du système. Elle doit être utilisée uniquement lorsque-backupTarget est spécifié. sinon, la commande échoue. Peut être utilisé avec l’option **-include** .</br>Conseil : Le volume cible d’une sauvegarde de volume critique peut être un lecteur local, mais il ne peut pas être l’un des volumes inclus dans la sauvegarde.|
|-vssFull|Pour Windows Server 2008 R2 et versions ultérieures, effectue une sauvegarde complète à l’aide du Service VSS (VSS). Tous les fichiers sont sauvegardés, l’historique de chaque fichier est mis à jour pour refléter la sauvegarde et les journaux des sauvegardes précédentes peuvent être tronqués. Si ce paramètre n’est pas utilisé, Wbadmin start Backup effectue une sauvegarde de copie, mais l’historique des fichiers sauvegardés n’est pas mis à jour.</br>Avertissement : N’utilisez pas ce paramètre si vous utilisez un produit autre que Sauvegarde Windows Server pour sauvegarder des applications qui se trouvent sur les volumes inclus dans la sauvegarde en cours. Cela peut potentiellement rompre le type incrémentiel, différentiel ou autre de sauvegarde créé par l’autre produit de sauvegarde en raison de l’historique sur lequel il repose pour déterminer la quantité de données à sauvegarder qui peut être manquante et qui peut effectuer une sauvegarde complète. inutilement.|
|-vssCopy|Pour Windows Server 2008 R2 et versions ultérieures, effectue une sauvegarde de copie à l’aide de VSS. Tous les fichiers sont sauvegardés, mais l’historique des fichiers en cours de sauvegarde n’est pas mis à jour, ce qui vous permet de conserver toutes les informations sur les fichiers qui ont été modifiés, supprimés, etc., ainsi que tous les fichiers journaux des applications. L’utilisation de ce type de sauvegarde n’affecte pas la séquence de sauvegardes incrémentielles et différentielles qui peuvent se produire indépendamment de cette sauvegarde de copie. Valeur par défaut.</br>Avertissement : Une sauvegarde de copie ne peut pas être utilisée pour des sauvegardes incrémentielles ou différentielles ou des restaurations.|
|-utilisateur|Pour Windows Server 2008 R2 et versions ultérieures, spécifie l’utilisateur disposant d’une autorisation d’écriture sur la destination de stockage de sauvegarde (s’il s’agit d’un dossier partagé distant). L’utilisateur doit être membre du groupe administrateurs ou opérateurs de sauvegarde sur l’ordinateur qui est en cours de sauvegarde.|
|-Password|Pour Windows Server 2008 R2 et versions ultérieures, spécifie le mot de passe pour le nom d’utilisateur fourni par le paramètre-User.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|
|-allowDeleteOldBackups|Remplace toutes les sauvegardes effectuées avant la mise à niveau de l’ordinateur.|

## <a name="remarks"></a>Notes

Pour afficher la valeur de l’identificateur de disque de vos disques, tapez **Wbadmin obtenir des disques**.

## <a name="BKMK_examples"></a>Illustre

Les exemples suivants montrent comment la commande **Wbadmin enable backup** peut être utilisée dans différents scénarios de sauvegarde :

#1 de scénario
- Planifier des sauvegardes de lecteurs de disque dur e :, d:\mountpoint \\et \\? \Volume{cc566d14-44A0-11d9-9d93-806e6f6e6963}\
- Enregistrer les fichiers sur le disque
- Exécuter les sauvegardes tous les jours à 9:00 h 00 et 6:00 P.M.
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  #2 de scénario
- Planifier des sauvegardes du dossier d:\Documents à l’emplacement \\ \\réseau backupshare\backup1
- Utilisez les informations d’identification réseau pour l’administrateur de sauvegarde Aaren Ekelund (aekel), qui est membre du CONTOSOEAST de domaine pour authentifier l’accès au partage réseau. Le mot de passe de Aaren est *$3hM9 ^ 5LP*.
- Exécuter les sauvegardes tous les jours à 12:00 h 00 et 7:00 P.M.
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  #3 de scénario
- Planifiez les sauvegardes du volume t : et du dossier d:\Documents sur le lecteur h :, mais excluez le dossier d:\Documents\~tmp
- Effectuez une sauvegarde complète à l’aide de l’Service VSS.
- Exécuter les sauvegardes tous les jours à 1:00 h 00
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)