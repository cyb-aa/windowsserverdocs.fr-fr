---
title: Performances des e/s réseau Hyper-V
description: Considérations sur les performances des e/s réseau dans le réglage des performances d’Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 17551da6cd270f05cf2d6b1a8147958f82b2c9b3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851802"
---
# <a name="hyper-v-network-io-performance"></a>Performances des e/s réseau Hyper-V

Le serveur 2016 contient plusieurs améliorations et de nouvelles fonctionnalités permettant d’optimiser les performances du réseau sous Hyper-V.  La documentation sur l’optimisation des performances du réseau sera incluse dans une future version de cet article.

## <a name="live-migration"></a>Migration dynamique

Migration dynamique vous permet de déplacer de façon transparente des ordinateurs virtuels en cours d’exécution d’un nœud d’un cluster de basculement vers un autre nœud du même cluster sans perte de connexion réseau ou de temps d’arrêt perçu.

> [!NOTE]
> Le clustering de basculement nécessite un stockage partagé pour les nœuds de cluster.

Le processus de déplacement d’un ordinateur virtuel en cours d’exécution peut être divisé en deux phases principales. La première phase copie la mémoire de l’ordinateur virtuel de l’hôte actuel vers le nouvel hôte. La deuxième phase transfère l’état de l’ordinateur virtuel de l’hôte actuel vers le nouvel hôte. Les durées des deux phases sont déterminées en fonction de la vitesse à laquelle les données peuvent être transférées de l’hôte actuel vers le nouvel hôte.

Le fait de fournir un réseau dédié pour le trafic de migration dynamique contribue à réduire le temps nécessaire à la réalisation d’une migration dynamique et garantit des temps de migration cohérents.

![exemple de configuration de la migration dynamique Hyper-v](../../media/perftune-guide-live-migration.png)

En outre, l’augmentation du nombre de tampons d’envoi et de réception sur chaque carte réseau impliquée dans la migration peut améliorer les performances de la migration.

Windows Server 2012 R2 a introduit une option pour accélérer Migration dynamique en compressant la mémoire avant de la transférer sur le réseau ou d’utiliser l’accès direct à la mémoire à distance (RDMA) si votre matériel la prend en charge.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
