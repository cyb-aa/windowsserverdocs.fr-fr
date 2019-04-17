---
title: Stockage espaces Direct en mémoire cache de lecture
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122052"
---
# L’utilisation d’espaces de stockage Direct avec la volume partagé de cluster en mémoire cache de lecture
> S’applique à: Windows Server 2016, Windows Server 2019

Cette rubrique décrit comment utiliser la mémoire système pour améliorer les performances des [Espaces de stockage Direct](storage-spaces-direct-overview.md).

Espaces de stockage Direct est compatible avec la Volume partagé de Cluster (CSV) en mémoire cache de lecture. À l’aide de la mémoire système en cache les lectures peut améliorer les performances pour les applications telles que Hyper-V, qui utilise des e/s sans tampon pour accéder aux fichiers VHD ou VHDX. (IOs sans tampon sont des opérations qui ne sont pas mis en cache par le Gestionnaire de Cache Windows).

Dans la mesure où le cache en mémoire est de serveur local, il améliore localité des données pour les déploiements d’espaces de stockage Direct hyperconvergé: récentes lectures sont mises en cache dans la mémoire sur le même hôte dans lequel la machine virtuelle est en cours d’exécution, en réduisant la fréquence à laquelle les lectures emprunter le réseau. Cela aboutit à latence plus faible et de meilleures performances de stockage.

## Considérations relatives à la planification

La lecture en mémoire pour le cache s’avère plus efficace pour les charges de travail gourmandes en lecture, par exemple, Virtual Desktop Infrastructure (VDI). À l’inverse, si la charge de travail est extrêmement gourmandes en écriture, le cache peut entraîner une surcharge plus que valeur et doit être désactivé.

Vous pouvez utiliser jusqu'à 80 % de mémoire physique totale pour la volume partagé de cluster en mémoire cache de lecture.

  > [!TIP]
  > Pour les déploiements hyperconvergés, où compute et stockage s’exécuter sur les mêmes serveurs, veillez à laisser suffisamment de mémoire pour les machines virtuelles. Pour les déploiements de serveur de fichiers avec montée en (en puissance) convergés, moins conflit d’utilisation de la mémoire, cela ne s’applique pas.

  > [!NOTE]
  > Activée que sans ce dernier de cache de lecture de certains outils microbenchmarking comme DISKSPD et [Flotte de machine virtuelle](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) peuvent produire pire des résultats avec la volume partagé de cluster en mémoire. Par défaut flotte de machine virtuelle crée un talon 10 VHDX par machine virtuelle – TiB environ 1 total pour les ordinateurs 100 virtuels – puis effectue des lectures *uniformément aléatoire* et écrit sur ces derniers. Contrairement aux charges de travail réelles, les lectures ne suivent pas n’importe quel modèle prévisible ou répétitive, afin que le cache en mémoire n’est pas efficace et simplement entraîne une surcharge.

## Configuration de la mémoire dans le cache de lecture

La volume partagé de cluster lecture en mémoire pour le cache est disponible dans Windows Server 2016 et Windows Server 2019 avec les mêmes fonctionnalités. Dans Windows Server 2016, elle est désactivée par défaut. Dans Windows Server 2019, elle est activée par défaut avec 1 Go allouée.

| Version du système d'exploitation          | Taille de cache de volume partagé de cluster par défaut |
|---------------------|------------------------|
| Windows Server2016 | 0 (désactivé)           |
| Windows Server2019 | 1 talon                   |

Pour afficher la quantité de mémoire allouée à l’aide de PowerShell, exécutez:

```PowerShell
(Get-Cluster).BlockCacheSize
```

La valeur renvoyée est dans mebibytes (Mio) par serveur. Par exemple, `1024` représente 1 gibibyte (talon).

Pour modifier la quantité de mémoire allouée, modifier cette valeur à l’aide de PowerShell. Par exemple, pour allouer 2 talon par serveur, exécutez:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Les modifications prennent effet immédiatement, suspendre, puis reprendre vos volumes partagés de cluster ou les déplacement entre les serveurs. Par exemple, utiliser ce fragment PowerShell pour déplacer chaque volume partagé de cluster vers un autre nœud de serveur et vice versa:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## Voir également

- [Présentation des espaces de stockage direct](storage-spaces-direct-overview.md)
