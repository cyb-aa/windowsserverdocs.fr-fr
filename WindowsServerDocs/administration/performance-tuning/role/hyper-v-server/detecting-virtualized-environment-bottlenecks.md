---
title: Détecter les goulots d’étranglement dans un environnement virtualisé
description: Comment détecter et résoudre les éventuels goulots d’étranglement de performances Hyper-v
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cdad5f0cc3b0e49ae46e975e3acc2c48a18e5f70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867530"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Détecter les goulots d’étranglement dans un environnement virtualisé

Cette section devrait vous donner quelques conseils sur les éléments à analyser à l’aide de l’Analyseur de performances et comment identifier où le problème peut être lors de l’hôte ou certaines des machines virtuelles n’effectuent pas comme vous vous attendiez.

## <a name="processor-bottlenecks"></a>Goulots d’étranglement de processeur

Voici quelques scénarios courants pouvant entraîner des goulots d’étranglement du processeur :

-   Un ou plusieurs processeurs logiques sont chargés.

-   Un ou plusieurs processeurs virtuels sont chargés.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Utilisation du processeur logique - \\processeur logique hyperviseur Hyper-V (\*)\\Total de % temps d’exécution

-   Utilisation du processeur virtuel - \\processeur virtuel hyperviseur Hyper-V (\*)\\Total de % temps d’exécution

-   Racine de l’utilisation du processeur virtuel - \\processeur virtuel racine d’hyperviseur Hyper-V (\*)\\Total de % temps d’exécution

Si le **processeur logique de l’hyperviseur Hyper-V (\_Total)\\% temps Total d’exécution** compteur est plus de 90 %, l’hôte est surchargé. Vous devez ajouter davantage de puissance de traitement ou déplacer des machines virtuelles vers un hôte différent.

Si le **Hyper-V hyperviseur virtuel Processor(VM Name:VP x)\\% temps Total d’exécution** compteur est plus de 90 % pour tous les processeurs virtuels, vous devez procédez comme suit :

-   Vérifiez que l’hôte n’est pas surchargé

-   Déterminer si la charge de travail peut tirer parti de plusieurs processeurs virtuels

-   Affecter plusieurs processeurs virtuels à la machine virtuelle

Si **Hyper-V hyperviseur virtuel Processor(VM Name:VP x)\\% temps Total d’exécution** compteur est plus de 90 % pour certains, mais pas tout, des processeurs virtuels, vous devez procédez comme suit :

-   Si votre charge de travail est reçu de réseau intensives, vous devez envisager d’utiliser vRSS.

-   Si les machines virtuelles n’exécutent pas Windows Server 2012 R2, vous devez ajouter plus de cartes réseau.

-   Si votre charge de travail est grande consommatrice du stockage, activez la topologie NUMA virtuelle et ajouter plus de disques virtuels.

Si le **processeur virtuel racine de l’hyperviseur Hyper-V (VP de racine x)\\% temps Total d’exécution** compteur est supérieure à 90 % pour certains, mais pas tous les processeurs virtuels et le **processeur (x)\\% temps d’interruption et Processeur (x)\\% temps DPC** compteur environ additionne la valeur de la **Processor(Root VP x) virtuel racine\\% temps Total d’exécution** compteur, vous devez vous assurer activer VMQ sur les cartes réseau.

## <a name="memory-bottlenecks"></a>Goulots d’étranglement de mémoire

Voici quelques scénarios courants pouvant entraîner des goulots d’étranglement de mémoire :

-   L’hôte ne répond pas.

-   Impossible de démarrer les machines virtuelles.

-   Machines virtuelles suffisamment de mémoire.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Mémoire\\mégaoctets disponibles

-   Équilibrage de la mémoire dynamique Hyper-V (\*)\\mémoire disponible

Vous pouvez utiliser les compteurs de performances suivants à partir de la machine virtuelle :

-   Mémoire\\mégaoctets disponibles

Si le **mémoire\\mégaoctets disponibles** et **Hyper-V Dynamic Memory Balancer (\*)\\mémoire disponible** compteurs sont faibles sur l’ordinateur hôte, vous devez arrêter les utilisations non vitales de services et migrer un ou plusieurs ordinateurs virtuels vers un autre hôte.

Si le **mémoire\\mégaoctets disponibles** compteur est faible sur l’ordinateur virtuel, vous devez attribuer davantage de mémoire à la machine virtuelle. Si vous utilisez la mémoire dynamique, vous devez augmenter le paramètre de mémoire maximale.

## <a name="network-bottlenecks"></a>Goulots d’étranglement du réseau

Voici quelques scénarios courants pouvant entraîner des goulots d’étranglement du réseau :

-   L’hôte est réseau lié.

-   La machine virtuelle est lié de réseau.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Interface réseau (*nom de l’adaptateur réseau*)\\octets/s

Vous pouvez utiliser les compteurs de performances suivants à partir de la machine virtuelle :

-   Carte de réseau virtuel Hyper-V (*nom de machine virtuelle&lt;GUID&gt;*)\\octets/s

Si le **octets carte réseau physique par seconde** compteur est supérieur ou égale à 90 % de la capacité, vous devez ajouter des cartes réseau supplémentaires, migrer des machines virtuelles vers un autre hôte et configurer la QoS du réseau.

Si le **octets/s de carte de réseau virtuel Hyper-V** compteur est supérieur ou égale à 250 Mbits/s, vous devez ajouter les adaptateurs de cartes réseau supplémentaires dans la machine virtuelle, activez vRSS et utiliser SR-IOV.

Si vos charges de travail ne peut pas répondre à leur latence du réseau, activez SR-IOV présenter les ressources de la carte réseau physique à la machine virtuelle.

## <a name="storage-bottlenecks"></a>Goulots d’étranglement de stockage

Voici quelques scénarios courants pouvant entraîner des goulots d’étranglement de stockage :

-   L’hôte et la machine virtuelle sont des opérations lent ou délai d’attente.

-   La machine virtuelle est lente.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Disque physique (*lettre de disque*)\\moyenne disque s/lecture

-   Disque physique (*lettre de disque*)\\moyenne disque s/écriture

-   Disque physique (*lettre de disque*)\\longueur de file d’attente moy.

-   Disque physique (*lettre de disque*)\\longueur de file d’attente moy.

Si les latences sont régulièrement supérieures à 50 ms, vous devez procédez comme suit :

-   Répartir les machines virtuelles sur un stockage supplémentaire

-   Envisagez l’achat de stockage plus rapide

-   Prendre en compte les espaces de stockage hiérarchisé, qui a été introduite dans Windows Server 2012 R2

-   Envisagez d’utiliser la QoS de stockage, ce qui a été introduite dans Windows Server 2012 R2

-   Utiliser VHDX

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances du processeur de Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Stockage Hyper-V les performances d’e/s](storage-io-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Machines virtuelles Linux](linux-virtual-machine-considerations.md)
