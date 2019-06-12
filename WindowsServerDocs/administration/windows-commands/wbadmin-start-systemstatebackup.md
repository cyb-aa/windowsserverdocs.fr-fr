---
title: WBADMIN start systemstatebackup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d98ba295b2a76baf98e85a01a02677d57922877d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440261"
---
# <a name="wbadmin-start-systemstatebackup"></a>WBADMIN start systemstatebackup



Crée une sauvegarde de l’état système de l’ordinateur local et les stocke sur l’emplacement spécifié.

> [!NOTE]
> Sauvegarde Windows Server ne pas sauvegarder ou récupérer les ruches utilisateur du Registre (HKEY_CURRENT_USER) dans le cadre de la sauvegarde de l’état système ou de récupération de l’état système.

Pour effectuer une sauvegarde de l’état système avec la sous-commande, vous devez être membre du **opérateurs de sauvegarde** groupe ou le **administrateurs** groupe, ou vous devez vous avoir été délégué des autorisations appropriées. En outre, vous devez exécuter **wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir un invite de commandes avec élévation de privilèges de droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

Pour obtenir des exemples montrant comment utiliser cette sous-commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

## <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                                                                                                                      Description                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Spécifie l’emplacement où vous souhaitez stocker la sauvegarde. L’emplacement de stockage nécessite une lettre de lecteur ou un volume en fonction du GUID du format : \\ \\? \Volume {*GUID*}.</br>Une sauvegarde d’état système sur un dossier réseau partagé n’est pas pris en charge sur un ordinateur exécutant Windows Server 2008. Si votre serveur exécute Windows Server 2008 R2 ou version ultérieure, vous pouvez utiliser la commande **- backuptarget :\\\\servername\sharedFolder\\**  pour stocker les sauvegardes d’état système. |
|    -quiet     |                                                                                                                                                                                                   Exécute la sous-commande sans invite à l’utilisateur.                                                                                                                                                                                                    |

## <a name="remarks"></a>Notes

Pour plus d’informations sur l’enregistrement d’une sauvegarde de l’état système sur un volume qui, à son tour, contient des fichiers d’état système, consultez l’article 944530 dans la Base de connaissances Microsoft ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)).

## <a name="BKMK_examples"></a>Exemples

Pour créer une sauvegarde de l’état système et le stocker sur le volume f, tapez :
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBBackup](https://technet.microsoft.com/library/jj902459.aspx) applet de commande