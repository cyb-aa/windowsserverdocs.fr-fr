---
title: Storage Spaces Direct en mémoire cache de lecture
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850550"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>À l’aide d’espaces de stockage Direct avec les CSV en mémoire cache de lecture
> S'applique à : Windows Server 2016, Windows Server 2019

Cette rubrique décrit l’utilisation de mémoire système pour améliorer les performances des [espaces de stockage Direct](storage-spaces-direct-overview.md).

Espaces de stockage Direct est compatible avec la Volume partagé de Cluster (CSV) en mémoire cache de lecture. À l’aide de la mémoire système pour les lectures du cache peut améliorer les performances pour les applications telles que Hyper-V, qui utilise des e/s sans tampon pour accéder aux fichiers VHD ou VHDX. (E/s sans tampon sont toutes les opérations qui ne sont pas mises en cache par le Gestionnaire de Cache de Windows).

Étant donné que le cache en mémoire est un serveur local, il améliore la localité des données pour les déploiements d’espaces de stockage Direct hyperconvergé : récentes lectures sont mises en cache en mémoire sur le même hôte que celui dans lequel la machine virtuelle est en cours d’exécution, ce qui réduit la fréquence à laquelle lectures établies via le réseau. Cela entraîne une latence plus faible et de meilleures performances de stockage.

## <a name="planning-considerations"></a>Considérations de planification

La mémoire de lecture cache est plus efficace pour les charges de travail de lecture intensive, telles que Virtual Desktop Infrastructure (VDI). À l’inverse, si la charge de travail est extrêmement gourmandes en écriture, le cache peut introduire une charge plus importante que la valeur et doit être désactivé.

Vous pouvez utiliser jusqu'à 80 % de la mémoire physique totale pour la volume partagé de cluster en mémoire cache de lecture.

  > [!TIP]
  > Pour les déploiements hyperconvergés, où le calcul et stockage s’exécutent sur les mêmes serveurs, veillez à laisser suffisamment de mémoire pour vos machines virtuelles. Pour les déploiements de serveur de fichiers avec montée en puissance (parallèle SoFS) convergées, avec une baisse des contentions de mémoire, cela ne s’applique pas.

  > [!NOTE]
  > Certains outils microbenchmarking comme DISKSPD et [flotte de machine virtuelle](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) peut produire des résultats pires avec le fichier CSV en mémoire lire cache activé que sans lui. Par défaut flotte de machine virtuelle crée un VHDX par machine virtuelle : environ 1 TIO de 10 Gio total pour 100 machines virtuelles – et effectue ensuite *aléatoires uniformément* lit et écrit dans les. Contrairement aux charges de travail réelles, les lectures ne suivent pas un modèle prévisible ou répétitif, afin que le cache en mémoire n’est pas efficace et simplement entraîne une surcharge.

## <a name="configuring-the-in-memory-read-cache"></a>Configuration de la mémoire cache de lecture

La volume partagé de cluster en mémoire lire le cache est disponible dans Windows Server 2016 et Windows Server 2019 avec la même fonctionnalité. Dans Windows Server 2016, elle est désactivée par défaut. Dans Windows Server 2019, il est activé par défaut avec 1 Go allouée.

| Version du système d'exploitation          | Taille de cache de volume partagé de cluster par défaut |
|---------------------|------------------------|
| Windows Server 2016 | 0 (désactivé)           |
| Windows Server 2019 | 1 GiB                   |

Pour afficher la quantité de mémoire est allouée à l’aide de PowerShell, exécutez :

```PowerShell
(Get-Cluster).BlockCacheSize
```

La valeur retournée est dans mebibytes (MiB) par serveur. Par exemple, `1024` représente 1 gibioctet (Gio).

Pour modifier la quantité de mémoire est allouée, modifiez cette valeur à l’aide de PowerShell. Par exemple, pour allouer 2 Go par serveur, exécutez :

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Pour les modifications prennent effet immédiatement, suspendre, puis reprendre vos volumes partagés de cluster ou les déplacement entre les serveurs. Par exemple, utiliser ce fragment de PowerShell pour déplacer chaque CSV vers un autre nœud de serveur, puis à nouveau :

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
