---
title: Performances d’e/s de réseau Hyper-V
description: Considérations sur les performances d’e/s de réglage des performances d’Hyper-V du réseau
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9f576963a93c8c0b9d6c05f406cc3331c407ceb9
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811518"
---
# <a name="hyper-v-network-io-performance"></a>Performances d’e/s de réseau Hyper-V

Server 2016 contient plusieurs améliorations et nouvelles fonctionnalités pour optimiser les performances réseau sous Hyper-V.  Documentation sur la façon d’optimiser les performances réseau sera incluse dans une future version de cet article.

## <a name="live-migration"></a>Migration dynamique

Migration dynamique vous permet en toute transparence déplacer les machines virtuelles en cours d’exécution d’un nœud d’un cluster de basculement vers un autre nœud dans le même cluster sans une connexion réseau ou d’une indisponibilité perceptible.

> [!NOTE]
> Le Clustering de basculement nécessite un stockage partagé pour les nœuds de cluster.

Le processus de déplacement d’une machine virtuelle en cours d’exécution peut être divisé en deux phases principales. La première phase copie la mémoire de la machine virtuelle à partir de l’hôte actuel vers le nouvel hôte. La deuxième phase transfère l’état de la machine virtuelle à partir de l’hôte actuel vers le nouvel hôte. Les durées des deux phases est largement déterminée par la vitesse à laquelle les données peuvent être transférées à partir de l’hôte actuel vers le nouvel hôte.

En fournissant un réseau dédié pour la migration en direct le trafic permet de réduire le temps nécessaire pour effectuer une migration dynamique et elle garantit que les temps de migration cohérent.

![exemple de configuration de migration dynamique hyper-v](../../media/perftune-guide-live-migration.png)

En outre, augmentation du nombre d’envoi et réception des mémoires tampons de chaque réseau adaptateur est impliqué dans la migration peut améliorer les performances de migration.

Windows Server 2012 R2 a introduit une option pour accélérer la Migration dynamique par la compression de la mémoire avant de le transférer sur le réseau ou utiliser l’accès à distance directe mémoire (RDMA), si votre matériel le prend en charge.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
