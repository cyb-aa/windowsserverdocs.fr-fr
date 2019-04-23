---
title: Exporter et importer des machines virtuelles
description: Vous montre comment exporter et importer des machines virtuelles à l’aide du Gestionnaire Hyper-V ou Windows PowerShell.
ms.prod: windows-server-threshold
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: b326fe8d7ff05ba73fc94225fa38921b42eb3fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844320"
---
>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Exporter et importer des machines virtuelles

Cet article vous montre comment exporter et importer un ordinateur virtuel, qui est un moyen rapide de déplacer ou de les copier. Cet article décrit également certains choix à effectuer lorsque vous effectuez une exportation ou d’importation.

## <a name="export-a-virtual-machine"></a>Exporter une machine virtuelle

Une exportation rassemble tous les fichiers requis dans une unité--fichiers de disque dur virtuel, les fichiers de configuration de machine virtuelle et les fichiers de point de contrôle. Vous pouvez le faire sur un ordinateur virtuel qui se trouve dans un état démarré ou arrêté.

### <a name="using-hyper-v-manager"></a>À l’aide du Gestionnaire Hyper-V

Pour créer une exportation de machine virtuelle :

1. Dans le Gestionnaire Hyper-V, cliquez sur la machine virtuelle et sélectionnez **exporter**.

2. Choisir l’emplacement où stocker les fichiers exportés, puis cliquez sur **exporter**.

Lorsque l’exportation est terminée, vous pouvez voir tous les fichiers exportés sous l’emplacement d’exportation.

### <a name="using-powershell"></a>À l'aide de PowerShell

Ouvrez une session en tant qu’administrateur et exécutez une commande semblable à ce qui suit, après remplacement \<nom de machine virtuelle\> et \<chemin d’accès\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Pour plus d’informations, consultez [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importer une machine virtuelle 

Lors de l’importation d’une machine virtuelle, celle-ci est inscrite auprès de l’hôte Hyper-V. Vous pouvez importer dans l’hôte ou un nouvel hôte. Si vous importez vers le même hôte, vous n’avez pas besoin d’exporter la machine virtuelle tout d’abord, étant donné que Hyper-V tente de recréer la machine virtuelle à partir de fichiers disponibles. L’importation d’une machine virtuelle, il inscrit afin qu’il peut être utilisé sur l’ordinateur hôte Hyper-V.

L’Assistant Importation d’ordinateur virtuel vous permet également de résoudre les incompatibilités qui peuvent exister lors du déplacement d’un hôte vers un autre. Il s’agit généralement de différences de matériel physique, comme la mémoire, les commutateurs virtuels et les processeurs virtuels.

### <a name="import-using-hyper-v-manager"></a>Importation à l’aide du Gestionnaire Hyper-V

Pour importer un ordinateur virtuel :

1. À partir de la **Actions** menu du Gestionnaire Hyper-V, cliquez sur **importer l’ordinateur virtuel**.

2. Cliquez sur **Suivant**.

3. Sélectionnez le dossier qui contient les fichiers exportés, puis cliquez sur **suivant**.

4. Sélectionnez la machine virtuelle à importer.

5. Choisissez le type d’importation, puis cliquez sur **suivant**. (Pour obtenir une description, consultez [importer des types](#import-types), ci-dessous.)

6. Cliquez sur **Terminer**.

### <a name="import-using-powershell"></a>Importer à l’aide de PowerShell

Utilisez le **Import-VM** applet de commande, suivant l’exemple pour le type d’importation que vous souhaitez. Pour obtenir une description des types, consultez [importer des types](#import-types), ci-dessous. 

#### <a name="register-in-place"></a>Inscrire en place

Ce type d’importation utilise les fichiers où ils sont stockés au moment de l’importation et conserve l’ID de. l’ordinateur virtuel La commande suivante montre un exemple de fichier d’importation. Exécuter une commande similaire avec vos propres valeurs.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Restaurer

Pour importer l’ordinateur virtuel en spécifiant votre propre chemin d’accès pour les fichiers de machine virtuelle, exécutez une commande similaire à celle-ci, en remplaçant les exemples par vos valeurs :

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importer en tant que copie

Pour effectuer une importation de copie et déplacer les fichiers de machine virtuelle à l’emplacement de Hyper-V par défaut, exécutez une commande similaire à celle-ci, en remplaçant les exemples par vos valeurs :

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Pour plus d’informations, consultez [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Importer des types

Hyper-V propose trois types d’importation :

- **Inscrire sur place** – part du principe de ce type sont des fichiers d’exportation à l’emplacement où vous allez stocker et exécuter la machine virtuelle. L’ordinateur virtuel importé a le même ID qu’au moment de l’exportation. Pour cette raison, si la machine virtuelle est déjà inscrite avec Hyper-V, elle doit être supprimée avant l’importation fonctionne. Une fois l’importation terminée, les fichiers d’exportation deviennent l’exécution fichiers d’état et ne peut pas être supprimé.

- **Restaurer l’ordinateur virtuel** : restaurer la machine virtuelle à un emplacement de votre choix, ou utiliser la valeur par défaut pour Hyper-V. Ce type d’importation crée une copie des fichiers exportés et les déplace vers l’emplacement sélectionné. Lors de l’importation, l’ID de la machine virtuelle est le même qu’au moment de l’exportation. Pour cette raison, si la machine virtuelle est déjà en cours d’exécution dans Hyper-V, elle doit être supprimée avant la fin de l’importation. Une fois l’importation terminée, les fichiers exportés restent intacts et peuvent être supprimés ou importés à nouveau.

- **Copier l’ordinateur virtuel** : cela est similaire au type de restauration dans la mesure où vous sélectionnez un emplacement pour les fichiers. La différence est que l’ordinateur virtuel importé a un nouvel ID unique, ce qui signifie que vous pouvez importer l’ordinateur virtuel vers le même hôte plusieurs fois.

