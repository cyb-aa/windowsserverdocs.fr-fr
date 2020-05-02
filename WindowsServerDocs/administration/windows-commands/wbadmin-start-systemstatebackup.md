---
title: Wbadmin start systemstatebackup
description: Rubrique de référence pour Wbadmin start systemstatebackup, qui crée une sauvegarde de l’état du système de l’ordinateur local et le stocke à l’emplacement spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c4ca390d910a5a38919d60421091264aa56de33
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725902"
---
# <a name="wbadmin-start-systemstatebackup"></a>Wbadmin start systemstatebackup



Crée une sauvegarde de l’état du système de l’ordinateur local et le stocke à l’emplacement spécifié.

> [!NOTE]
> Sauvegarde Windows Server ne sauvegarde pas ou ne récupère pas les ruches des utilisateurs du Registre (HKEY_CURRENT_USER) dans le cadre de la sauvegarde de l’état du système ou de la récupération de l’état du système.

Pour effectuer une sauvegarde de l’état du système avec cette sous-commande, vous devez être membre du groupe **opérateurs de sauvegarde** ou **administrateurs** , ou l’autorisation appropriée doit vous avoir été déléguée. En outre, vous devez exécuter **Wbadmin** à partir d’une invite de commandes avec élévation de privilèges. (Pour ouvrir une invite de commandes avec élévation de privilèges, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.)

## <a name="syntax"></a>Syntaxe

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                                                                                                                      Description                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | Spécifie l’emplacement où vous souhaitez stocker la sauvegarde. L’emplacement de stockage requiert une lettre de lecteur ou un volume GUID au format : \\ \\? \Volume{*GUID*}.</br>Une sauvegarde de l’état du système dans un dossier réseau partagé n’est pas prise en charge sur un ordinateur exécutant Windows Server 2008. Si votre serveur exécute Windows Server 2008 R2 ou une version ultérieure, vous pouvez utiliser la commande **-\\\\backupTarget\\ : servername\sharedFolder** pour stocker les sauvegardes de l’état du système. |
|    -quiet     |                                                                                                                                                                                                   Exécute la sous-commande sans invite à l’utilisateur.                                                                                                                                                                                                    |

## <a name="remarks"></a>Notes 

Pour plus d’informations sur l’enregistrement d’une sauvegarde de l’état du système sur un volume qui, à son tour, contient des fichiers d’État du[https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)système, voir l’article 944530 de la base de connaissances Microsoft ().

## <a name="examples"></a>Exemples

Pour créer une sauvegarde de l’état du système et la stocker sur le volume f, tapez :
```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Applet [de commande Start-WBBackup](https://technet.microsoft.com/library/jj902459.aspx)