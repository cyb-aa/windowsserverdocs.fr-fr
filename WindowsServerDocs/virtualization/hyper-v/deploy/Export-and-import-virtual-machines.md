---
title: Exporter et importer des machines virtuelles
description: Montre comment exporter et importer des ordinateurs virtuels à l’aide du Gestionnaire Hyper-V ou de Windows PowerShell.
ms.prod: windows-server
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 6e130ee8a040cd5b56908d77d91bf196a60de6f7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392976"
---
>S’applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Exporter et importer des machines virtuelles

Cet article explique comment exporter et importer une machine virtuelle, ce qui est un moyen rapide de les déplacer ou de les copier. Cet article aborde également certains des choix à effectuer lors de l’exportation ou de l’importation.

## <a name="export-a-virtual-machine"></a>Exporter une machine virtuelle

Une exportation rassemble tous les fichiers requis dans une unité : les fichiers de disque dur virtuel, les fichiers de configuration d’ordinateur virtuel et les fichiers de point de contrôle. Vous pouvez le faire sur un ordinateur virtuel qui est dans un état Démarré ou arrêté.

### <a name="using-hyper-v-manager"></a>À l’aide du Gestionnaire Hyper-V

Pour créer une exportation de machine virtuelle :

1. Dans le Gestionnaire Hyper-V, cliquez avec le bouton droit sur l’ordinateur virtuel, puis sélectionnez **Exporter**.

2. Choisissez l’emplacement de stockage des fichiers exportés, puis cliquez sur **Exporter**.

Une fois l’exportation terminée, vous pouvez voir tous les fichiers exportés sous l’emplacement d’exportation.

### <a name="using-powershell"></a>À l'aide de PowerShell

Ouvrez une session en tant qu’administrateur et exécutez une commande semblable à la suivante après avoir remplacé \<nom de la machine virtuelle\> et \<chemin\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Pour plus d’informations, consultez [Exporter-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importer une machine virtuelle 

Lors de l’importation d’une machine virtuelle, celle-ci est inscrite auprès de l’hôte Hyper-V. Vous pouvez effectuer une réimportation dans l’hôte ou un nouvel hôte. Si vous effectuez l’importation sur le même hôte, vous n’avez pas besoin d’exporter l’ordinateur virtuel en premier, car Hyper-V tente de recréer l’ordinateur virtuel à partir des fichiers disponibles. L’importation d’un ordinateur virtuel l’enregistre afin qu’il puisse être utilisé sur l’hôte Hyper-V.

L’Assistant importation d’ordinateur virtuel vous aide également à corriger les incompatibilités qui peuvent exister lors du passage d’un ordinateur hôte à un autre. Il s’agit généralement de différences de matériel physique, telles que la mémoire, les commutateurs virtuels et les processeurs virtuels.

### <a name="import-using-hyper-v-manager"></a>Importer à l’aide du Gestionnaire Hyper-V

Pour importer une machine virtuelle :

1. Dans le menu **actions** du Gestionnaire Hyper-V, cliquez sur **Importer un ordinateur virtuel**.

2. Cliquez sur **Suivant**.

3. Sélectionnez le dossier qui contient les fichiers exportés, puis cliquez sur **suivant**.

4. Sélectionnez la machine virtuelle à importer.

5. Choisissez le type d’importation, puis cliquez sur **suivant**. (Pour obtenir des descriptions, consultez [types d’importation](#import-types), ci-dessous.)

6. Cliquez sur **Terminer**.

### <a name="import-using-powershell"></a>Importer à l’aide de PowerShell

Utilisez l’applet de commande **Import-VM** , en suivant l’exemple correspondant au type d’importation souhaité. Pour obtenir une description des types, consultez [types d’importation](#import-types), ci-dessous. 

#### <a name="register-in-place"></a>Inscrire sur place

Ce type d’importation utilise les fichiers dans lesquels ils sont stockés au moment de l’importation et conserve l’ID de la machine virtuelle. La commande suivante affiche un exemple de fichier d’importation. Exécutez une commande similaire avec vos propres valeurs.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Restaurer

Pour importer la machine virtuelle en spécifiant votre propre chemin d’accès pour les fichiers de l’ordinateur virtuel, exécutez une commande comme celle-ci, en remplaçant les exemples par vos valeurs :

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importer en tant que copie

Pour effectuer une importation de copie et déplacer les fichiers de l’ordinateur virtuel vers l’emplacement Hyper-V par défaut, exécutez une commande comme celle-ci, en remplaçant les exemples par vos valeurs :

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Pour plus d’informations, consultez [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Types d’importation

Hyper-V propose trois types d’importation :

- **Inscrire sur place** : ce type suppose que les fichiers d’exportation se trouvent à l’emplacement où vous allez stocker et exécuter l’ordinateur virtuel. La machine virtuelle importée a le même ID qu’au moment de l’exportation. Pour cette raison, si la machine virtuelle est déjà inscrite auprès d’Hyper-V, elle doit être supprimée avant le fonctionnement de l’importation. Une fois l’importation terminée, les fichiers d’exportation deviennent les fichiers d’État en cours d’exécution et ne peuvent pas être supprimés.

- **Restaurer l’ordinateur virtuel** : restaurez l’ordinateur virtuel à un emplacement de votre choix ou utilisez la valeur par défaut pour Hyper-V. Ce type d’importation crée une copie des fichiers exportés et les déplace vers l’emplacement sélectionné. Lors de l’importation, l’ID de la machine virtuelle est le même qu’au moment de l’exportation. Pour cette raison, si la machine virtuelle est déjà en cours d’exécution dans Hyper-V, elle doit être supprimée pour que l’importation puisse être effectuée. Une fois l’importation terminée, les fichiers exportés restent intacts et peuvent être supprimés ou réimportés.

- **Copier l’ordinateur virtuel** : ce type de restauration est similaire au type de restauration dans lequel vous sélectionnez un emplacement pour les fichiers. La différence réside dans le fait que la machine virtuelle importée possède un nouvel ID unique, ce qui signifie que vous pouvez importer l’ordinateur virtuel sur le même hôte plusieurs fois.

