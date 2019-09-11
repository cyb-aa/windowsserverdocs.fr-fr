---
title: Performances du processeur Hyper-V
description: Considérations sur les performances du processeur dans le réglage des performances Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 232141758032a8e21eca50ddb73ac9bc3cf6af56
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866548"
---
# <a name="hyper-v-processor-performance"></a>Performances du processeur Hyper-V


## <a name="virtual-machine-integration-services"></a>Services d’intégration de machines virtuelles

La machine virtuelle Integration Services inclure des pilotes compatibles pour les périphériques d’e/s spécifiques à Hyper-V, ce qui réduit considérablement la surcharge du processeur pour les e/s par rapport aux appareils émulés. Vous devez installer la dernière version de la machine virtuelle Integration Services sur chaque ordinateur virtuel pris en charge. Les services diminuent l’utilisation du processeur des invités, des invités inactifs aux invités très utilisés et améliorent le débit d’e/s. Il s’agit de la première étape de réglage des performances sur un serveur exécutant Hyper-V. Pour obtenir la liste des systèmes d’exploitation invités pris en charge, consultez [vue d’ensemble d’Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Processeurs virtuels

Hyper-V dans Windows Server 2016 prend en charge un maximum de 240 processeurs virtuels par ordinateur virtuel. Les machines virtuelles dont les charges ne sont pas gourmandes en processeur doivent être configurées pour utiliser un processeur virtuel. Cela est dû à la surcharge supplémentaire qui est associée à plusieurs processeurs virtuels, tels que des coûts de synchronisation supplémentaires dans le système d’exploitation invité.

Augmentez le nombre de processeurs virtuels si l’ordinateur virtuel requiert plus d’un processeur de traitement dans les pics de charge.

## <a name="background-activity"></a>Activité en arrière-plan

La réduction de l’activité en arrière-plan dans les machines virtuelles inactives libère des cycles d’UC qui peuvent être utilisés ailleurs par d’autres machines virtuelles. Les invités Windows utilisent généralement moins d’un pour cent d’un processeur lorsqu’ils sont inactifs. Voici plusieurs méthodes conseillées pour réduire l’utilisation de l’UC en arrière-plan d’une machine virtuelle :

-   Installez la dernière version de la machine virtuelle Integration Services.

-   Supprimez la carte réseau émulée via la boîte de dialogue Paramètres de l’ordinateur virtuel (utilisez l’adaptateur spécifique à Microsoft Hyper-V).

-   Supprimer les appareils inutilisés, tels que le CD-ROM et le port COM, ou déconnecter leur média.

-   Conservez le système d’exploitation invité Windows sur l’écran de connexion lorsqu’il n’est pas utilisé et désactivez l’écran de veille.

-   Passez en revue les tâches et les services planifiés qui sont activés par défaut.

-   Passez en revue les fournisseurs de suivi ETW qui sont activés par défaut en exécutant la **requête logman. exe-ETS**

-   Améliorez les applications serveur pour réduire les activités périodiques (telles que les minuteurs).

-   Fermez Gestionnaire de serveur à la fois sur l’hôte et sur les systèmes d’exploitation invités.

-   Ne laissez pas le Gestionnaire Hyper-V s’exécuter, car il actualise constamment la miniature de la machine virtuelle.

Voici les meilleures pratiques supplémentaires pour configurer une *version cliente* de Windows dans une machine virtuelle afin de réduire l’utilisation globale du processeur :

-   Désactivez les services d’arrière-plan, tels que SuperFetch et Windows Search.

-   Désactivez les tâches planifiées telles que la défragmentation planifiée.

## <a name="virtual-numa"></a>NUMA virtuel

Pour permettre la virtualisation de charges de travail à grande échelle, Hyper-V dans Windows Server 2016 a développé des limites de mise à l’échelle de machine virtuelle. Une seule machine virtuelle peut recevoir jusqu’à 240 processeurs virtuels et 12 to de mémoire. Lors de la création de ces machines virtuelles volumineuses, la mémoire de plusieurs nœuds NUMA sur le système hôte sera probablement utilisée. Dans une telle configuration de machine virtuelle, si les processeurs virtuels et la mémoire ne sont pas alloués à partir du même nœud NUMA, les charges de travail peuvent présenter des performances médiocres en raison de l’impossibilité de tirer parti des optimisations NUMA.

Dans Windows Server 2016, Hyper-V présente une topologie NUMA virtuelle aux machines virtuelles. Par défaut, cette topologie NUMA virtuelle est optimisée pour correspondre à la topologie NUMA de l'ordinateur hôte sous-jacent. Le fait d'exposer une topologie NUMA virtuelle sur une machine virtuelle permet au système d'exploitation hôte et à toute application orientée NUMA exécutée dessus de tirer parti des optimisations de performances NUMA, exactement comme s'ils s'exécutaient sur un ordinateur physique.

Il n’existe aucune distinction entre une topologie NUMA virtuelle et physique du point de vue de la charge de travail. Sur une machine virtuelle, quand une charge de travail alloue de la mémoire locale pour des données et qu'elle accède à ces données sur le même nœud NUMA, l'accès à la mémoire locale est plus rapide sur le système physique sous-jacent. Aucune pénalité de performances due à l'accès mémoire à distance n'est constatée. Seules les applications compatibles NUMA peuvent bénéficier de vNUMA.

Microsoft SQL Server est un exemple d’application prenant en charge NUMA. Pour plus d’informations, consultez [Présentation de l’accès mémoire non uniforme](https://technet.microsoft.com/library/ms178144.aspx).

Les fonctionnalités de topologie NUMA virtuelle et de mémoire dynamique ne peuvent pas être utilisées simultanément. Une machine virtuelle sur laquelle la mémoire dynamique est activée n'a qu'un seul nœud NUMA virtuel et aucune topologie NUMA ne lui est présentée, quels que soient les paramètres de NUMA virtuelle.

Pour plus d’informations sur la topologie NUMA virtuelle, consultez [vue d’ensemble de la topologie NUMA virtuelle Hyper-V](https://technet.microsoft.com/library/dn282282.aspx).

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
