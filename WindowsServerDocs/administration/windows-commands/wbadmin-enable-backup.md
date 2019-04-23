---
title: Activer la sauvegarde WBADMIN
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6fd0bea5da83ca9351d5ea1028c94392bdb40422
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845540"
---
# <a name="wbadmin-enable-backup"></a>Activer la sauvegarde WBADMIN



Crée et permet une planification de sauvegarde quotidienne ou modifie une planification de sauvegarde existante. Sans aucun paramètre n’est spécifié, il affiche les paramètres de sauvegarde planifiés.

Pour configurer ou modifier une planification de sauvegarde quotidienne, vous devez être un de ses membres le **administrateurs** ou **opérateurs de sauvegarde** groupe. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes** puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

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
|-addtarget|Pour Windows Server 2008, spécifie l’emplacement de stockage pour les sauvegardes. Vous devez spécifier une destination pour les sauvegardes comme un identificateur de disque (voir Remarques). Le disque est formaté avant leur utilisation, et toutes les données existantes sur celui-ci sont définitivement effacées.</br>Pour Windows Server 2008 R2 et versions ultérieures, spécifie l’emplacement de stockage pour les sauvegardes. Vous devez spécifier l’emplacement comme un disque, le volume ou le chemin d’accès UNC Universal Naming Convention () vers un dossier partagé distant (\\\\\<nom_serveur >\<nom_partage >\). Par défaut, la sauvegarde sera enregistrée à : \\ \\ <servername> \<nom_partage > \WindowsImageBackup\<ComputerBackedUp >\. Si vous spécifiez un disque, le disque sera formaté avant leur utilisation, et toutes les données existantes sur celui-ci sont effacées définitivement. Si vous spécifiez un dossier partagé, vous ne pouvez pas ajouter plusieurs emplacements. Vous pouvez uniquement spécifier un dossier partagé comme emplacement de stockage à la fois.</br>Important : Si vous enregistrez une sauvegarde vers un dossier partagé distant, cette sauvegarde sera remplacée si vous utilisez le même dossier pour sauvegarder le même ordinateur à nouveau. En outre, si l’opération de sauvegarde échoue, vous pourrez retrouver sans sauvegarde, car la sauvegarde antérieure sera remplacée, mais la plus récente sauvegarde ne sera pas utilisable. Vous pouvez éviter cela en créant des sous-dossiers dans le dossier partagé distant pour organiser vos sauvegardes. Si vous procédez ainsi, les sous-dossiers devez double de l’espace du dossier parent.</br>Emplacement qu’une seule peut être spécifié dans une seule commande. Plusieurs emplacements de stockage de sauvegarde de volume et le disque peuvent être ajoutés en exécutant la commande à nouveau.|
|-removetarget|Spécifie l’emplacement de stockage que vous souhaitez supprimer de la planification de sauvegarde existante. Vous devez spécifier l’emplacement comme un identificateur de disque (voir Remarques).|
|-schedule|Spécifie les heures de la journée pour créer une sauvegarde, au format hh : mm et délimité par des virgules.|
|-include|Pour Windows Server 2008, spécifie la liste délimitée par des virgules des lettres de lecteur de volume, les points de montage de volume ou les noms de volumes GUID à inclure dans la sauvegarde.</br>Pour Windows Server 2008 R2and plus tard, spécifie la liste délimitée par des virgules des éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\). Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier.|
|-nonRecurseInclude|Pour Windows Server 2008 R2 et versions ultérieures, spécifie la non récursif, une liste délimitée par des virgules d’éléments à inclure dans la sauvegarde. Vous pouvez inclure plusieurs fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\). Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier. Doit être utilisé uniquement lorsque le paramètre - backupTarget est utilisé.|
|-exclude|Pour Windows Server 2008 R2 et versions ultérieures, spécifie la liste délimitée par des virgules des éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\). Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier.|
|-nonRecurseExclude|Pour Windows Server 2008 R2 et versions ultérieures, spécifie la non récursif, une liste délimitée par des virgules d’éléments à exclure de la sauvegarde. Vous pouvez exclure des fichiers, dossiers ou volumes. Les chemins d'accès aux volumes peuvent être spécifiés à l'aide de lettres de lecteur de volume, de points de montage de volumes ou de noms de volumes GUID. Si vous utilisez un nom basé sur le GUID de volume, il doit s’arrêter avec une barre oblique inverse (\). Vous pouvez utiliser le caractère générique (*) dans le nom de fichier lorsque vous spécifiez un chemin d’accès à un fichier.|
|-hyperv|Spécifie la liste délimitée par des virgules des composants à inclure dans la sauvegarde. L’identificateur peut être un nom de composant ou le GUID du composant (avec ou sans accolades).|
|-systemState|Pour Windows 7 et Windows Server 2008 R2 et versions ultérieures, crée une sauvegarde qui inclut l’état du système en plus de tous les autres éléments que vous avez spécifié avec la **-inclure** paramètre. L’état du système contient les fichiers de démarrage (Boot.ini, NDTLDR, NTDetect.com), le Registre Windows, y compris les paramètres de COM, le dossier SYSVOL (stratégies de groupe et les Scripts d’ouverture de session), Active Directory et NTDS. DIT sur les contrôleurs de domaine et, si le service de certificats est installé, le certificat Store. Si le rôle de serveur Web installé sur votre serveur, le méta-annuaire IIS sera inclus. Si le serveur fait partie d’un cluster, les informations de service de Cluster sont également incluses.|
|-allCritical|Spécifie que tous les volumes critiques (volumes qui contiennent l’état du système d’exploitation) inclus dans les sauvegardes. Ce paramètre est utile si vous créez une sauvegarde complète du système ou de récupération de l’état système. Il doit être utilisé uniquement lorsque - backupTarget est spécifié, sinon la commande échoue. Peut être utilisé avec le **-inclure** option.</br>Astuce : Le volume cible pour une sauvegarde sur le volume critique peut être un lecteur local, mais il ne peut pas être un des volumes qui sont inclus dans la sauvegarde.|
|-vssFull|Pour Windows Server 2008 R2 et versions ultérieures, effectue un intégral sauvegarder à l’aide de Volume Shadow Copy Service (VSS). Tous les fichiers sont sauvegardés, l’historique de chaque fichier est mis à jour qu’elle a été sauvegardée, et les journaux des sauvegardes antérieures peuvent être tronquées. Si ce paramètre n’est pas utilisé wbadmin start sauvegarde effectue une copie de sauvegarde, mais l’historique des fichiers en cours de sauvegarde n’est pas mis à jour.</br>Avertissement : N’utilisez pas ce paramètre si vous utilisez un produit autre que la sauvegarde Windows Server pour sauvegarder des applications qui se trouvent sur les volumes inclus dans la sauvegarde actuelle. Cette opération donc risque de perturber l’incrémentiel, différentielle ou autres type des sauvegardes qui crée le produit de sauvegarde, car l’historique qui ils estiment que pour déterminer la quantité de données à sauvegarder peut-être être manquante et ils peuvent effectuer complète de sauvegarde inutilement.|
|-vssCopy|Pour Windows Server 2008 R2 et versions ultérieures, effectue une sauvegarde de copie à l’aide de VSS. Tous les fichiers sont sauvegardés, mais l’historique des fichiers en cours de sauvegarde n’est pas mis à jour afin de conserver toutes les informations sur les fichiers où modifié, supprimé, etc., ainsi que les fichiers journaux d’application. À l’aide de ce type de sauvegarde n’affecte pas la séquence des sauvegardes incrémentielles et différentielles qui peuvent se produire indépendamment de cette sauvegarde de copie. Valeur par défaut.</br>Avertissement : Une sauvegarde de copie ne peut pas être utilisée pour les restaurations ou sauvegardes incrémentielles ou différentielles.|
|-utilisateur|Pour Windows Server 2008 R2 et versions ultérieures, spécifie l’utilisateur avec autorisation d’écriture vers la destination de stockage de sauvegarde (s’il est un dossier partagé distant). L’utilisateur doit être membre du groupe Administrateurs ou du groupe Opérateurs de sauvegarde sur l’ordinateur qui est en cours de sauvegarde.|
|-password|Pour Windows Server 2008 R2 et versions ultérieures, spécifie le mot de passe pour le nom d’utilisateur fourni par le paramètre-utilisateur.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|
|-allowDeleteOldBackups|Remplace toutes les sauvegardes effectuées avant que l’ordinateur a été mis à niveau.|

## <a name="remarks"></a>Notes

Pour afficher la valeur d’identificateur de disque pour vos disques, tapez **wbadmin get disques**.

## <a name="BKMK_examples"></a>Exemples

Les exemples suivants montrent comment la **wbadmin activer la sauvegarde** commande peut être utilisée dans différents scénarios de sauvegarde :

Scénario #1
-   Planifier des sauvegardes de disques durs e:, d:\mountpoint, et \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
-   Enregistrer les fichiers sur le disque DiskID
-   Exécuter des sauvegardes tous les jours à 9 h 00 et 18 h 00.
```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Scénario #2
-   Planifier des sauvegardes du dossier d:\documents à l’emplacement réseau \\ \\backupshare\backup1
-   Utilisez les informations d’identification réseau pour l’administrateur de sauvegarde Aaren Ekelund (aekel), qui est membre du domaine CONTOSOEAST pour authentifier l’accès au partage réseau. Mot de passe de Aaren *$3 hM 9 ^ 5lp*.
-   Exécuter des sauvegardes tous les jours à 12 h 00 et 7 h 00.
```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```
Scénario #3
-   Planifier des sauvegardes de volume des d:\documents t: et de dossier pour le lecteur h:, mais exclure le dossier d:\documents\~tmp
-   Effectuer une sauvegarde complète à l’aide du Service de cliché instantané de Volume.
-   Exécution de sauvegardes quotidiennement à 1 h 00
```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)