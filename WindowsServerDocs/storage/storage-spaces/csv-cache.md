---
title: Cache de lecture en mémoire espaces de stockage direct
ms.prod: windows-server
ms.author: eldenc
manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: d9ebc40b69373dafbebdb87f2abe624a5a7a4375
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858952"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Utilisation de espaces de stockage direct avec le cache de lecture en mémoire CSV
> S’applique à : Windows Server 2016, Windows Server 2019

Cette rubrique explique comment utiliser la mémoire système pour améliorer les performances de [espaces de stockage direct](storage-spaces-direct-overview.md).

Espaces de stockage direct est compatible avec le cache de lecture en mémoire de Volume partagé de cluster (CSV). L’utilisation de la mémoire système pour le cache des lectures peut améliorer les performances des applications comme Hyper-V, qui utilisent des e/s non mises en mémoire tampon pour accéder aux fichiers VHD ou VHDX. (Les e/s non mises en mémoire tampon sont des opérations qui ne sont pas mises en cache par le gestionnaire de cache Windows.)

Étant donné que le cache en mémoire est un serveur local, il améliore la localité des données pour les déploiements de espaces de stockage direct hyper-convergé : les lectures récentes sont mises en cache en mémoire sur le même hôte où l’ordinateur virtuel est exécuté, ce qui réduit la fréquence de lecture sur le réseau. Cela entraîne une latence plus faible et de meilleures performances de stockage.

## <a name="planning-considerations"></a>Considérations de planification

Le cache de lecture en mémoire est plus efficace pour les charges de travail nécessitant beaucoup de lectures, telles que VDI (Virtual Desktop Infrastructure). À l’inverse, si la charge de travail est très gourmande en écriture, le cache peut introduire une surcharge supérieure à la valeur et doit être désactivé.

Vous pouvez utiliser jusqu’à 80% de la mémoire physique totale pour le cache de lecture en mémoire du volume partagé de cluster.

  > [!TIP]
  > Pour les déploiements hyper-convergent, où le calcul et le stockage s’exécutent sur les mêmes serveurs, veillez à conserver suffisamment de mémoire pour vos machines virtuelles. Pour les déploiements de Serveur de fichiers avec montée en puissance parallèle convergés (SoFS), avec moins de contention de mémoire, cela ne s’applique pas.

  > [!NOTE]
  > Certains outils de microtest tels que DISKSPD et la [flotte de machines virtuelles](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) peuvent produire de pires résultats avec le cache de lecture en mémoire du volume partagé de cluster activé. Par défaut, la flotte de machine virtuelle crée 1 10 Gio VHDX par machine virtuelle : environ 1 Tio total pour 100 machines virtuelles, puis effectue des lectures et des écritures *uniformément aléatoires* . Contrairement aux charges de travail réelles, les lectures ne suivent pas un modèle prévisible ou répétitif, de sorte que le cache en mémoire n’est pas efficace et n’est qu’une surcharge.

## <a name="configuring-the-in-memory-read-cache"></a>Configuration du cache de lecture en mémoire

Le cache de lecture en mémoire CSV est disponible dans Windows Server 2016 et Windows Server 2019 avec la même fonctionnalité. Dans Windows Server 2016, il est désactivé par défaut. Dans Windows Server 2019, il est activé par défaut avec 1 Go alloué.

| Version du système d'exploitation          | Taille du cache CSV par défaut |
|---------------------|------------------------|
| Windows Server 2016 | 0 (désactivé)           |
| Windows Server 2019 | 1 Gio                   |

Pour voir la quantité de mémoire allouée à l’aide de PowerShell, exécutez :

```PowerShell
(Get-Cluster).BlockCacheSize
```

La valeur retournée est dans mebibytes (MiB) par serveur. Par exemple, `1024` représente 1 « Gibioctet » (Gio).

Pour modifier la quantité de mémoire allouée, modifiez cette valeur à l’aide de PowerShell. Par exemple, pour allouer 2 Gio par serveur, exécutez :

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Pour que les modifications prennent effet immédiatement, interrompez-les, puis reprenez vos volumes CSV ou déplacez-les entre les serveurs. Par exemple, utilisez ce fragment PowerShell pour déplacer chaque volume partagé de cluster vers un autre nœud de serveur, puis revenez en arrière :

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Voir aussi

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
