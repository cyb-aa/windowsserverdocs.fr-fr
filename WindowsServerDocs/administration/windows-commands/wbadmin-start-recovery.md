---
title: Wbadmin start Recovery
description: La rubrique commandes Windows pour Wbadmin start Recovery, qui exécute une opération de récupération basée sur les paramètres que vous spécifiez.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b5a65e67e7a34ca5263c85c1038820e0a4fc1ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829612"
---
# <a name="wbadmin-start-recovery"></a>Wbadmin start Recovery



Exécute une opération de récupération en fonction des paramètres que vous spécifiez.

Pour effectuer une récupération avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples d’utilisation de cette sous-commande, consultez [exemples](#BKMK_Examples).

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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-version|Spécifie l’identificateur de version de la sauvegarde à récupérer au format MM/JJ/AAAA-HH : MM. Si vous ne connaissez pas l’identificateur de version, tapez **Wbadmin obtenir des versions**.|
|-éléments|Spécifie une liste délimitée par des virgules de volumes, d’applications, de fichiers ou de dossiers à récupérer.</br>-Si **-ItemType** est un **volume**, vous ne pouvez spécifier qu’un seul volume, en fournissant la lettre de lecteur du volume, le point de montage du volume ou le nom du volume basé sur le GUID.</br>-Si **-ItemType** est **application**, vous ne pouvez spécifier qu’une seule application. Pour être récupérés, l’application doit être inscrite auprès de Sauvegarde Windows Server. Vous pouvez également utiliser la valeur **ADIFM** pour récupérer une installation de Active Directory. Pour plus d’informations, consultez la section Notes dans.</br>-Si **-ItemType est un** **fichier**, vous pouvez spécifier des fichiers ou des dossiers, mais ils doivent faire partie du même volume et ils doivent se trouver dans le même dossier parent.|
|-ItemType|Spécifie le type des éléments à récupérer. Il doit s’agir d’un **volume**, d’une **application**ou d’un **fichier**.|
|-backupTarget|Spécifie l’emplacement de stockage qui contient la sauvegarde que vous souhaitez récupérer. Ce paramètre est utile lorsque l’emplacement est différent de l’emplacement où les sauvegardes de cet ordinateur sont généralement stockées.|
|-machine|Spécifie le nom de l’ordinateur pour lequel vous souhaitez récupérer la sauvegarde. Ce paramètre est utile lorsque plusieurs ordinateurs ont été sauvegardés au même emplacement. Elle doit être utilisée lorsque le paramètre **-backupTarget** est spécifié.|
|-recoveryTarget|Spécifie l’emplacement vers lequel effectuer la restauration. Ce paramètre est utile si cet emplacement est différent de l’emplacement précédemment sauvegardé. Il peut également être utilisé pour les restaurations de volumes, de fichiers ou d’applications. Si vous restaurez un volume, vous pouvez spécifier la lettre de lecteur du volume de l’autre volume. Si vous restaurez un fichier ou une application, vous pouvez spécifier un autre emplacement de récupération.|
|-récursif|Valide uniquement lors de la récupération de fichiers. Récupère les fichiers dans les dossiers et tous les fichiers subordonnés aux dossiers spécifiés. Par défaut, seuls les fichiers qui résident directement dans les dossiers spécifiés sont récupérés.|
|-remplacer|Valide uniquement lors de la récupération de fichiers. Spécifie l’action à entreprendre lorsqu’un fichier récupéré existe déjà au même emplacement.</br>-   **Skip** empêche sauvegarde Windows Server d’ignorer le fichier existant et de poursuivre la récupération du fichier suivant.</br>-   **CreateCopy** fait en sorte que sauvegarde Windows Server crée une copie du fichier existant afin que le fichier existant ne soit pas modifié.</br>-   **remplacement** sauvegarde Windows Server entraîne le remplacement du fichier existant par le fichier à partir de la sauvegarde.|
|-notRestoreAcl|Valide uniquement lors de la récupération de fichiers. Spécifie de ne pas restaurer les listes de contrôle d’accès (ACL) de sécurité des fichiers en cours de récupération à partir de la sauvegarde. Par défaut, les listes de contrôle d’accès de sécurité sont restaurées (la valeur par défaut est **true)** . Si ce paramètre est utilisé, les listes de contrôle d’accès des fichiers restaurés sont héritées de l’emplacement dans lequel les fichiers sont restaurés.|
|-skipBadClusterCheck|Valide uniquement lors de la récupération de volumes. Ignore la vérification des disques sur lesquels vous effectuez la récupération pour obtenir des informations de cluster incorrectes. Si vous effectuez la récupération vers un autre serveur ou matériel, nous vous recommandons de ne pas utiliser ce paramètre. Vous pouvez exécuter manuellement la commande **chkdsk/b** sur ces disques à tout moment pour les vérifier pour les clusters défectueux, puis mettre à jour les informations du système de fichiers en conséquence.</br>Important : tant que vous n’avez pas exécuté **chkdsk** comme décrit, les clusters incorrects signalés sur votre système récupéré peuvent ne pas être précis.|
|-noRollForward|Valide uniquement lors de la récupération d’applications. Permet une récupération jusqu’à une date et heure antérieure d’une application si la version la plus récente des sauvegardes est sélectionnée. Pour les autres versions de l’application qui ne sont pas la dernière version, la récupération jusqu’à une date et heure précédente est effectuée par défaut.|
|-quiet|Exécute la sous-commande sans invite à l’utilisateur.|

## <a name="remarks"></a>Notes

-   Pour afficher la liste des éléments disponibles pour la récupération à partir d’une version de sauvegarde spécifique, utilisez **Wbadmin obtenir des éléments**. Si un volume n’a pas de point de montage ou de lettre de lecteur au moment de la sauvegarde, cette sous-commande retourne un nom de volume basé sur le GUID qui doit être utilisé pour récupérer le volume.
-   Lorsque l’option **-ItemType** est **app**, vous pouvez utiliser la valeur **ADIFM** pour **-Item** pour effectuer une opération d’installation à partir du support pour récupérer toutes les données associées nécessaires pour Active Directory Domain Services. **ADIFM** crée une copie de la base de données Active Directory, du Registre et de l’État SYSVOL, puis enregistre ces informations à l’emplacement spécifié par **-recoveryTarget**. Utilisez ce paramètre uniquement quand **-recoveryTarget** est spécifié.

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Pour exécuter une récupération de la sauvegarde à partir du 31 mars 2013, à 9:00 h 00, du volume d :, tapez :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
Pour exécuter une récupération sur le lecteur d de la sauvegarde à partir du 31 mars 2013, pris à 9:00 h 00, du Registre, tapez :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
Pour exécuter une récupération de la sauvegarde à partir du 31 mars, de 2013 à 9:00 h 00, des d:\folder et des dossiers subordonnés à d:\folder, tapez :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
Pour exécuter une récupération de la sauvegarde à partir du 31 mars 2013, à 9:00 A.M., du volume \\\\? \Volume{cc566d14-44A0-11d9-9d93-806e6f6e6963}\, type :
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
Pour exécuter une récupération de la sauvegarde à partir du 30 avril 2013, prise à 9:00 h 00, du dossier partagé \\\\servername\share de Serveur01, tapez :
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande Start-WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx)
-   Applet [de commande Start-WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx)
-   Applet [de commande Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx)
-   Applet [de commande Start-WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx)