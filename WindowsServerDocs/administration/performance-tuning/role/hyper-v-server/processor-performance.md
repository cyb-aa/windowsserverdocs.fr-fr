---
title: Performances du processeur de Hyper-V
description: Considérations sur les performances de processeur de réglage des performances d’Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 2a49fdaba89a01c8daf6483f72dbc88daa91452b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843240"
---
# <a name="hyper-v-processor-performance"></a>Performances du processeur de Hyper-V


## <a name="virtual-machine-integration-services"></a>Services d’intégration de machine virtuelle

Les Services d’intégration de Machine virtuelle incluent des pilotes compatibles pour les périphériques d’e/s spécifiques à Hyper-V, ce qui réduit considérablement UC surcharge d’e/s par rapport aux périphériques émulés. Vous devez installer la dernière version de la Machine virtuelle des Services d’intégration sur chaque machine virtuelle pris en charge. La diminution de services invités de l’utilisation du processeur des invités, du mode inactif à largement utilisé invités et améliore le débit d’e/s. Il s’agit de la première étape de réglage des performances dans un serveur exécutant Hyper-V. Pour obtenir la liste des systèmes d’exploitation invités pris en charge, consultez [vue d’ensemble d’Hyper-V](https://technet.microsoft.com/library/hh831531.aspx).

## <a name="virtual-processors"></a>Processeurs virtuels

Hyper-V dans Windows Server 2016 prend en charge un maximum de 240 processeurs virtuels par machine virtuelle. Machines virtuelles qui ont des charges qui ne sont pas beaucoup de ressources processeur doivent être configurés pour utiliser un processeur virtuel. Il s’agit en raison de la charge supplémentaire qui est associée à plusieurs processeurs virtuels, telles que les coûts de synchronisation supplémentaire dans le système d’exploitation invité.

Augmenter le nombre de processeurs virtuels si l’ordinateur virtuel nécessite plusieurs processeurs de traitement sous les pics de charge.

## <a name="background-activity"></a>Activité en arrière-plan

Réduction de l’activité d’arrière-plan dans les machines virtuelles inactives libère des cycles de processeur qui peuvent être utilisées ailleurs par d’autres machines virtuelles. Les invités Windows utilisent généralement inférieure à 1 % d’un processeur lorsqu’ils sont inactifs. Voici plusieurs bonnes pratiques pour réduire l’utilisation du processeur d’une machine virtuelle en arrière-plan :

-   Installez la dernière version de la Machine virtuelle Integration Services.

-   Supprimer la carte réseau émulée via la boîte de dialogue de paramètres de machine virtuelle (adaptateur d’utiliser Microsoft Hyper-V-spécifiques).

-   Supprimer les périphériques inutilisés tels que le port COM et de CD-ROM, ou déconnecter leur support.

-   Le système d’exploitation Windows sur l’écran de connexion quand il n’est pas utilisé et désactivez l’économiseur d’écran.

-   Passez en revue les tâches planifiées et les services qui sont activées par défaut.

-   Passez en revue les fournisseurs de suivi ETW sont activées par défaut en exécutant **logman.exe interroger - ets**

-   Améliorer les applications de serveur afin de réduire l’activité périodique (par exemple, les minuteurs).

-   Fermez le Gestionnaire de serveur sur les systèmes d’exploitation hôte et invité.

-   Ne laissez pas le Gestionnaire Hyper-V en cours d’exécution dans la mesure où il actualise constamment la miniature de la machine virtuelle.

Voici les meilleures pratiques supplémentaires pour configurer un *version du client* de Windows sur une machine virtuelle afin de réduire l’utilisation globale du processeur :

-   Désactivez les services d’arrière-plan telles que SuperFetch et de Windows Search.

-   Désactiver les tâches planifiées telles que la défragmentation planifiée.

## <a name="virtual-numa"></a>NUMA virtuel

Pour activer la virtualisation de grandes charges de travail de monter en puissance, Hyper-V dans Windows Server 2016 développé des limites de mise à l’échelle de machine virtuelle. Une seule machine virtuelle peut avoir jusqu'à 240 processeurs virtuels et 12 To de mémoire. Lorsque vous créez ces machines virtuelles de grande taille, la mémoire à partir de plusieurs nœuds NUMA sur le système hôte sera probablement utilisée. Dans cette configuration de machine virtuelle, si la mémoire et les processeurs virtuels ne sont pas allouées à partir du même nœud NUMA, charges de travail peuvent avoir des mauvaises performances en raison de l’incapacité à tirer parti des optimisations pour NUMA.

Dans Windows Server 2016, Hyper-V présente une topologie NUMA virtuelle aux machines virtuelles. Par défaut, cette topologie NUMA virtuelle est optimisée pour correspondre à la topologie NUMA de l'ordinateur hôte sous-jacent. Le fait d'exposer une topologie NUMA virtuelle sur une machine virtuelle permet au système d'exploitation hôte et à toute application orientée NUMA exécutée dessus de tirer parti des optimisations de performances NUMA, exactement comme s'ils s'exécutaient sur un ordinateur physique.

Il n'y a aucune distinction entre une topologie NUMA virtuelle et physique du point de vue de la charge de travail. Sur une machine virtuelle, quand une charge de travail alloue de la mémoire locale pour des données et qu'elle accède à ces données sur le même nœud NUMA, l'accès à la mémoire locale est plus rapide sur le système physique sous-jacent. Aucune pénalité de performances due à l'accès mémoire à distance n'est constatée. Seules les applications orientées NUMA peuvent bénéficier de vNUMA.

Microsoft SQL Server est un exemple d’application prenant en charge NUMA. Pour plus d’informations, consultez [compréhension Non-uniform Memory Access](https://technet.microsoft.com/library/ms178144.aspx).

Les fonctionnalités de topologie NUMA virtuelle et de mémoire dynamique ne peuvent pas être utilisées simultanément. Une machine virtuelle sur laquelle la mémoire dynamique est activée n'a qu'un seul nœud NUMA virtuel et aucune topologie NUMA ne lui est présentée, quels que soient les paramètres de NUMA virtuelle.

Pour plus d’informations sur la topologie NUMA virtuelle, consultez [Hyper-V Virtual NUMA Overview](https://technet.microsoft.com/library/dn282282.aspx).

##<a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Stockage Hyper-V les performances d’e/s](storage-io-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Machines virtuelles Linux](linux-virtual-machine-considerations.md)
