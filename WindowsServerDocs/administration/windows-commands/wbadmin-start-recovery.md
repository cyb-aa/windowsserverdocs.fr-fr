---
title: WBADMIN start recovery
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f24c9dfeb0ce87474e58d3bd2bce8b68e31cb63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823280"
---
# <a name="wbadmin-start-recovery"></a>WBADMIN start recovery



Exécute une opération de récupération en fonction des paramètres que vous spécifiez.

Pour effectuer une restauration avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin start recovery
-version:<VersionIdentifier>
-items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>}
-itemtype:{Volume | App | File}
[-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}]
[-recursive]
[-overwrite:{Overwrite | CreateCopy | Skip}]
[-notRestoreAcl]
[-skipBadClusterCheck]
[-noRollForward]
[-quiet]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie l’identificateur de version de la sauvegarde à restaurer dans MM/jj/aaaa-format hh : mm. Si vous ne connaissez pas l’identificateur de version, tapez **wbadmin get versions**.|
|-items|Spécifie une liste délimitée par des virgules des volumes, des applications, des fichiers ou dossiers à récupérer.</br>-If **- itemtype** est **Volume**, vous pouvez spécifier un seul volume, en fournissant la lettre de lecteur de volume, le point de montage de volume ou le nom de volume basée sur GUID.</br>-If **- itemtype** est **application**, vous pouvez spécifier qu’une seule application. Pour être récupérés, l’application doit avoir inscrite avec sauvegarde Windows Server. Vous pouvez également utiliser la valeur **ADIFM** pour récupérer une installation d’Active Directory. Pour plus d’informations, consultez la section Notes dans.</br>-If **- itemtype** est **fichier**, vous pouvez spécifier des fichiers ou dossiers, mais ils doivent faire partie du même volume, et ils doivent être sous le même dossier parent.|
|-itemtype|Spécifie le type des éléments à récupérer. Doit être **Volume**, **application**, ou **fichier**.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient la sauvegarde que vous souhaitez récupérer. Ce paramètre est utile lorsque l’emplacement est différent du où les sauvegardes de cet ordinateur sont généralement stockés.|
|-machine|Spécifie le nom de l’ordinateur que vous souhaitez restaurer la sauvegarde pour. Ce paramètre est utile lorsque plusieurs ordinateurs ont été sauvegardés dans le même emplacement. Il doit être utilisé lorsque le **- backupTarget** est précisé.|
|-recoveryTarget|Spécifie l’emplacement à restaurer. Ce paramètre est utile si cet emplacement est différent de celui qui a été précédemment sauvegardées. Il peut également être utilisé pour les restaurations de volumes, des fichiers ou des applications. Si vous restaurez un volume, vous pouvez spécifier la lettre de lecteur de volume de l’autre volume. Si vous restaurez un fichier ou une application, vous pouvez spécifier un autre emplacement de récupération.|
|-récursif|Valide uniquement lors de la récupération de fichiers. Récupère les fichiers dans les dossiers et tous les fichiers subordonnés dans les dossiers spécifiés. Par défaut, seuls les fichiers qui se trouvent directement dans les dossiers spécifiés sont récupérées.|
|-overwrite|Valide uniquement lors de la récupération de fichiers. Spécifie l’action à entreprendre lorsqu’un fichier qui est en cours de récupération déjà existe dans le même emplacement.</br>-   **Skip** provoque la sauvegarde de Windows Server ignorer le fichier existant et de poursuivre la récupération du fichier suivant.</br>-   **CreateCopy** provoque la sauvegarde de Windows Server créer une copie du fichier existant afin que le fichier existant ne soit pas modifié.</br>-   **Remplacer** provoque la sauvegarde de Windows Server remplacer le fichier existant par le fichier à partir de la sauvegarde.|
|-notRestoreAcl|Valide uniquement lors de la récupération de fichiers. Spécifie les listes de contrôle d’accès (ACL) de sécurité des fichiers en cours de récupération à partir de la sauvegarde à restaurer. Par défaut, les ACL de sécurité sont restaurés (la valeur par défaut est **true)**. Si ce paramètre est utilisé, les listes ACL pour les fichiers restaurés seront héritées à partir de l’emplacement auquel les fichiers sont en cours de restauration.|
|-skipBadClusterCheck|Valide uniquement lorsque la récupération de volumes. Ignore la vérification des disques que vous effectuez la récupération pour les informations de cluster défectueux. Si vous récupérez vers un autre serveur ou du matériel, nous recommandons que vous n’utilisez pas ce paramètre. Vous pouvez exécuter manuellement la commande **chkdsk /b** sur ces disques à tout moment pour vérifier les clusters défectueux et puis mettez à jour les informations de système de fichiers en conséquence.</br>Important : Jusqu'à ce que vous exécutiez **chkdsk** comme décrit, les clusters défectueux signalés sur votre système récupéré peuvent être inexact.|
|-noRollForward|Valide uniquement lors de la récupération des applications. Permet de récupération précédent point-à-temps d’une application si la dernière version à partir des sauvegardes est sélectionnée. Pour d’autres versions de l’application qui ne sont pas la récupération de point-à-temps plus tard, précédente est effectuée en tant que la valeur par défaut.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="remarks"></a>Notes

-   Pour afficher une liste d’éléments qui sont disponibles pour la récupération à partir d’une version de sauvegarde spécifique, utilisez **wbadmin obtenir les éléments**. Si un volume n’avait pas d’une lettre de lecteur ou le point de montage au moment de la sauvegarde, la sous-commande retournerait un nom basé sur le GUID de volume qui doit être utilisé pour récupérer le volume.
-   Lorsque le **- itemtype** est **application**, vous pouvez utiliser une valeur de **ADIFM** pour **-élément** pour effectuer une installation à partir de l’opération de média pour récupérer tous les le données associées nécessaires pour les Services de domaine Active Directory. **ADIFM** crée une copie de la base de données Active Directory, le Registre et l’état SYSVOL, puis enregistre ces informations dans l’emplacement spécifié par **- recoveryTarget**. Utilisez ce paramètre uniquement lorsque **- recoveryTarget** est spécifié.

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>Exemples

Pour exécuter une récupération de la sauvegarde depuis le 31 mars 2013, pris à 9 h 00, du volume d:, tapez :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Pour exécuter une récupération sur lecteur d de la sauvegarde depuis le 31 mars 2013, pris à 9 h 00, du Registre, tapez :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Pour exécuter une récupération de la sauvegarde depuis le 31 mars 2013, pris à 9 h 00, d:\folder et les dossiers subordonnés à d:\folder, tapez :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Pour exécuter une récupération de la sauvegarde à partir du 31 mars 2013, pris à 9 h 00, du volume \\ \\? \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\, type :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Pour exécuter une récupération de la sauvegarde à partir du 30 avril 2013, pris à 9 h 00, du dossier partagé \\ \\servername\share de server01, type :
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) applet de commande
-   [Start-WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) applet de commande
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) applet de commande
-   [Start-WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) applet de commande