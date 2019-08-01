---
title: Détection des goulots d’étranglement dans un environnement virtualisé
description: Comment détecter et résoudre les goulots d’étranglement potentiels des performances Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/31/2019
ms.locfileid: "63722879"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Détection des goulots d’étranglement dans un environnement virtualisé

Cette section doit vous donner des indications sur les éléments à surveiller à l’aide de l’analyseur de performances et sur la manière d’identifier l’endroit où le problème peut se produire lorsque l’ordinateur hôte ou certains des ordinateurs virtuels ne fonctionnent pas comme prévu.

## <a name="processor-bottlenecks"></a>Goulots d’étranglement du processeur

Voici quelques scénarios courants qui peuvent entraîner des goulots d’étranglement du processeur:

-   Un ou plusieurs processeurs logiques sont chargés

-   Un ou plusieurs processeurs virtuels sont chargés

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte:

-   Utilisation du processeur logique \\-processeur logique hyperviseur hyper\*-\\V ()% durée totale d’exécution

-   Utilisation du processeur virtuel \\-processeur virtuel de l’hyperviseur\\Hyper-V (\*)% de la durée d’exécution totale

-   Utilisation du processeur virtuel racine \\-processeur virtuel racine de l’hyperviseur\\Hyper-V (\*)% de la durée d’exécution totale

Si le compteur d' **exécution total du processeur logique\_de l'\\hyperviseur Hyper-V (total)% Total** est supérieur à 90%, l’ordinateur hôte est surchargé. Vous devez ajouter de la puissance de traitement ou déplacer des ordinateurs virtuels vers un autre ordinateur hôte.

Si le compteur de processeur virtuel de l’hyperviseur **Hyper-V (\\nom de la machine virtuelle: VP x)% total du Runtime** est supérieur à 90% pour tous les processeurs virtuels, vous devez effectuer les opérations suivantes:

-   Vérifier que l’hôte n’est pas surchargé

-   Déterminer si la charge de travail peut tirer parti d’un plus grand nombre de processeurs virtuels

-   Affecter plus de processeurs virtuels à la machine virtuelle

Si le processeur virtuel de l’hyperviseur **Hyper-V (nom\\de la machine virtuelle: VP x)%** du compteur d’exécution total est supérieur à 90% pour certains processeurs virtuels, mais pas tous, vous devez effectuer les opérations suivantes:

-   Si votre charge de travail reçoit beaucoup de ressources réseau, vous devez envisager d’utiliser vRSS.

-   Si les machines virtuelles n’exécutent pas Windows Server 2012 R2, vous devez ajouter d’autres cartes réseau.

-   Si votre charge de travail est gourmande en stockage, vous devez activer la topologie NUMA virtuelle et ajouter d’autres disques virtuels.

Si le compteur de l' **exécution du processeur virtuel racine de l’hyperviseur Hyper-V (VP racine x)\\% total d’exécution** est supérieur à 90% pour certains processeurs virtuels, mais pas tous, et le processeur **(x)\\% du temps d’interruption et du processeur (x)\\% du temps DPC** le compteur augmente approximativement la valeur du compteur de **processeur virtuel racine (VP racine x)\\% total du runtime** . vous devez donc vous assurer que l’installation de la carte réseau est activée sur les cartes réseau.

## <a name="memory-bottlenecks"></a>Goulots d’étranglement de mémoire

Voici quelques scénarios courants qui peuvent provoquer des goulots d’étranglement de mémoire:

-   L’hôte ne répond pas.

-   Impossible de démarrer les machines virtuelles.

-   Les machines virtuelles manquent de mémoire.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte:

-   Mémoire\\disponible en mégaoctets

-   Mémoire disponible de l’équilibreur de\*mémoire dynamique\\Hyper-V ()

Vous pouvez utiliser les compteurs de performances suivants à partir de la machine virtuelle:

-   Mémoire\\disponible en mégaoctets

Si la **\\** mémoire disponible en mégaoctets et les compteurs de mémoire disponibles pour l’équilibreur de **mémoire dynamique hyper-\*V ()\\** sont faibles sur l’hôte, vous devez arrêter les services non essentiels et migrer un ou plusieurs serveurs virtuels ordinateurs vers un autre hôte.

Si le **compteur\\mégaoctets disponibles en mémoire** est insuffisant sur la machine virtuelle, vous devez attribuer plus de mémoire à la machine virtuelle. Si vous utilisez Mémoire dynamique, vous devez augmenter le paramètre de mémoire maximale.

## <a name="network-bottlenecks"></a>Goulots d’étranglement du réseau

Voici quelques scénarios courants qui peuvent entraîner des goulots d’étranglement du réseau:

-   L’hôte est lié au réseau.

-   L’ordinateur virtuel est lié au réseau.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte:

-   Interface réseau (*nom*de la carte\\réseau), octets/s

Vous pouvez utiliser les compteurs de performances suivants à partir de la machine virtuelle:

-   Carte réseau virtuelle Hyper-V (nom de l'*ordinateur&lt;virtuel&gt;GUID*),\\octets/s

Si le compteur octets de la **carte réseau physique/s** est supérieur ou égal à 90% de la capacité, vous devez ajouter des cartes réseau supplémentaires, migrer des ordinateurs virtuels vers un autre ordinateur hôte et configurer la qualité de service réseau.

Si le compteur **octets/s de la carte réseau virtuel Hyper-V** est supérieur ou égal à 250 Mbits/s, vous devez ajouter des cartes réseau associées supplémentaires dans l’ordinateur virtuel, activer vRSS et utiliser SR-IOV.

Si vos charges de travail ne peuvent pas respecter leur latence réseau, activez SR-IOV pour présenter les ressources de carte réseau physique à la machine virtuelle.

## <a name="storage-bottlenecks"></a>Goulots d’étranglement de stockage

Voici quelques scénarios courants qui peuvent entraîner des goulots d’étranglement de stockage:

-   Les opérations de l’hôte et de l’ordinateur virtuel sont lentes ou expirent.

-   L’ordinateur virtuel est lent.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte:

-   Moyenne disque s/lecture du\\disque physique (*lettre du disque*)

-   Disque physique (*lettre*de disque\\) moyenne disque s/écriture

-   Longueur moyenne de file d’attente\\de lecture sur le disque physique (*lettre du disque*)

-   Longueur moyenne de file d’attente\\d’écriture sur le disque physique (*lettre du disque*)

Si les latences sont constamment supérieures à 50 ms, vous devez effectuer les opérations suivantes:

-   Répartir les machines virtuelles sur un stockage supplémentaire

-   Envisagez d’acheter un stockage plus rapide

-   Envisagez Tiered Storage espaces, qui a été introduit dans Windows Server 2012 R2

-   Envisagez d’utiliser la qualité de service de stockage qui a été introduite dans Windows Server 2012 R2

-   Utiliser VHDX

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
