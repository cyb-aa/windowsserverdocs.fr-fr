---
title: Extension des volumes dans les espaces de stockage direct
description: Comment redimensionner des volumes dans espaces de stockage direct à l’aide du centre d’administration Windows et de PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 05/07/2019
ms.openlocfilehash: 20482fe1728b12d4fe56dcfa397352fbb4b4f981
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366090"
---
# <a name="extending-volumes-in-storage-spaces-direct"></a>Extension des volumes dans les espaces de stockage direct
> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des instructions pour le redimensionnement des volumes sur un cluster [espaces de stockage direct](storage-spaces-direct-overview.md) à l’aide du centre d’administration Windows.

Regardez une vidéo rapide sur la façon de redimensionner un volume.

> [!VIDEO https://www.youtube-nocookie.com/embed/hqyBzipBoTI]

## <a name="extending-volumes-using-windows-admin-center"></a>Extension des volumes à l’aide du centre d’administration Windows

1. Dans le centre d’administration Windows, connectez-vous à un cluster espaces de stockage direct, puis sélectionnez **volumes** dans le volet **Outils** .
2. Dans la page volumes, sélectionnez l’onglet **inventaire** , puis sélectionnez le volume que vous souhaitez redimensionner.

    Sur la page Détails du volume, la capacité de stockage du volume est indiquée. Vous pouvez également ouvrir la page de détails des volumes directement à partir du tableau de bord. Dans le tableau de bord, dans le volet alertes, sélectionnez l’alerte, qui vous avertit si un volume ne dispose pas de suffisamment de capacité de stockage, puis sélectionnez **atteindre le volume**.

4. En haut de la page de détails volumes, sélectionnez **Redimensionner**.
5. Entrez une nouvelle taille supérieure, puis sélectionnez **Redimensionner**.

    Dans la page de détails volumes, la plus grande capacité de stockage pour le volume est indiquée, et l’alerte sur le tableau de bord est effacée.

## <a name="extending-volumes-using-powershell"></a>Extension des volumes à l’aide de PowerShell

### <a name="capacity-in-the-storage-pool"></a>Capacité du pool de stockage

Avant de redimensionner un volume, assurez-vous de que le pool de stockage possède une capacité suffisante pour prendre en charge le nouvel encombrement revu à la hausse. Par exemple, lors du redimensionnement d’un volume à miroir triple de 1 To à 2 To, son encombrement passera de 3 à 6 To. Pour que le redimensionnement réussisse, vous devez avoir au moins (6-3) = 3 To de capacité disponible dans le pool de stockage.

### <a name="familiarity-with-volumes-in-storage-spaces"></a>Familiarisation avec les volumes dans les espaces de stockage

Dans les espaces de stockage direct, chaque volume se compose de plusieurs objets empilés : le volume partagé de cluster (CSV), qui est un volume ; la partition ; le disque, qui est un disque virtuel ; et un ou plusieurs niveaux de stockage (le cas échéant). Pour redimensionner un volume, vous devez redimensionner plusieurs de ces objets.

![Volumes dans smapi](media/resize-volumes/volumes-in-smapi.png)

Pour vous familiariser avec ces objets, exécutez la commande **Get -** associée au nom correspondant dans PowerShell.

Exemple :

```PowerShell
Get-VirtualDisk
```

Pour suivre les associations entre objets dans la pile, utilisez une applet de commande **Get -** .

Par exemple, voici comment amener un disque virtuel jusqu'au volume voulu :

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-Disk | Get-Partition | Get-Volume 
```

### <a name="step-1--resize-the-virtual-disk"></a>Étape 1 : redimensionner le disque virtuel

Le disque virtuel peut utiliser ou non des niveaux de stockage, en fonction de la façon dont il a été créé.

Pour le vérifier, exécutez l’applet de commande suivante :

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier 
```

Si l’applet de commande ne renvoie rien, le disque virtuel n’utilise pas de niveaux de stockage.

#### <a name="no-storage-tiers"></a>Aucun niveau de stockage

Si le disque virtuel ne possède pas de niveaux de stockage, vous pouvez le redimensionner directement à l’aide de l'applet de commande **Resize-VirtualDisk**.

Indiquez la nouvelle taille au moyen du paramètre **-Size**.

```PowerShell
Get-VirtualDisk <FriendlyName> | Resize-VirtualDisk -Size <Size>
```

Lorsque vous redimensionnez le **disque virtuel**, il en va automatiquement de même pour le **disque** qui est également redimensionné.

![Resize-VirtualDisk](media/resize-volumes/Resize-VirtualDisk.gif)

#### <a name="with-storage-tiers"></a>Avec niveaux de stockage

Si le disque virtuel utilise des niveaux de stockage, vous pouvez redimensionner chaque niveau séparément à l’aide de l'applet de commande **Resize-StorageTier**.

Obtenez le nom des niveaux de stockage en suivant les associations à partir du disque virtuel.

```PowerShell
Get-VirtualDisk <FriendlyName> | Get-StorageTier | Select FriendlyName
```

Puis, pour chaque niveau, indiquez la nouvelle taille dans le paramètre **-Size**.

```PowerShell
Get-StorageTier <FriendlyName> | Resize-StorageTier -Size <Size>
```

> [!TIP]
> Si vos niveaux correspondent à des types de médias physiques différents (tels que **MediaType = SSD** et **MediaType = HDD**), vous devez vous assurer que vous disposez d’une capacité suffisante de chaque type de média dans le pool de stockage afin de gérer le nouvel encombrement, plus important, de chaque niveau.

Lorsque vous redimensionnez les niveaux **StorageTier**, il en va automatiquement de même pour **VirtualDisk** et **Disk** qui sont également redimensionnés.

![Resize-StorageTier](media/resize-volumes/Resize-StorageTier.gif)

### <a name="step-2--resize-the-partition"></a>Étape 2 : redimensionner la partition

Redimensionnez ensuite la partition à l’aide de l'applet de commande **Resize-Partition**. Le disque virtuel est supposé posséder deux partitions : la première est réservée et ne doit pas être modifiée. Celle que vous devez a pour caractéristiques **PartitionNumber = 2** et **Type = Basic**.

Indiquez la nouvelle taille au moyen du paramètre **-Size**. Nous vous recommandons d’utiliser la taille maximale prise en charge, comme illustré ci-après.

```PowerShell
# Choose virtual disk
$VirtualDisk = Get-VirtualDisk <FriendlyName>

# Get its partition
$Partition = $VirtualDisk | Get-Disk | Get-Partition | Where PartitionNumber -Eq 2

# Resize to its maximum supported size 
$Partition | Resize-Partition -Size ($Partition | Get-PartitionSupportedSize).SizeMax
```

Lorsque vous redimensionnez la **Partition**, il en va automatiquement de même pour **Volume** et **ClusterSharedVolume** qui sont également redimensionnés.

![Resize-Partition](media/resize-volumes/Resize-Partition.gif)

C’est tout !

> [!TIP]
> Vous pouvez vérifier que le volume est bien affecté de sa nouvelle taille en exécutant **Get-Volume**.

## <a name="see-also"></a>Voir aussi

- [espaces de stockage direct dans Windows Server 2016](storage-spaces-direct-overview.md)
- [Planification des volumes dans espaces de stockage direct](plan-volumes.md)
- [Création de volumes dans espaces de stockage direct](create-volumes.md)
- [Suppression de volumes dans espaces de stockage direct](delete-volumes.md)
