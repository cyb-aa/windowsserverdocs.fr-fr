---
title: Détection des goulots d’étranglement dans un environnement virtualisé
description: Comment détecter et résoudre les goulots d’étranglement potentiels des performances Hyper-v
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 211f35c151e94bc8b8a11a614edad18053cb18b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851776"
---
# <a name="detecting-bottlenecks-in-a-virtualized-environment"></a>Détection des goulots d’étranglement dans un environnement virtualisé

Cette section doit vous donner des indications sur les éléments à surveiller à l’aide de l’analyseur de performances et sur la manière d’identifier l’endroit où le problème peut se produire lorsque l’ordinateur hôte ou certains des ordinateurs virtuels ne fonctionnent pas comme prévu.

## <a name="processor-bottlenecks"></a>Goulots d’étranglement du processeur

Voici quelques scénarios courants qui peuvent entraîner des goulots d’étranglement du processeur :

-   Un ou plusieurs processeurs logiques sont chargés

-   Un ou plusieurs processeurs virtuels sont chargés

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Utilisation du processeur logique-\\processeur logique de l’hyperviseur Hyper-V (\*)\\% de la durée d’exécution totale

-   Utilisation du processeur virtuel-\\processeur virtuel de l’hyperviseur Hyper-V (\*)\\% de la durée d’exécution totale

-   Utilisation du processeur virtuel racine-\\processeur virtuel racine de l’hyperviseur Hyper-V (\*)\\% de la durée d’exécution totale

Si le **processeur logique de l’hyperviseur Hyper-V (\_total)\\%** du compteur d’exécution total est supérieur à 90%, l’ordinateur hôte est surchargé. Vous devez ajouter de la puissance de traitement ou déplacer des ordinateurs virtuels vers un autre ordinateur hôte.

Si le **processeur virtuel de l’hyperviseur Hyper-V (nom de la machine virtuelle : VP x)\\%** du compteur d’exécution total est supérieur à 90% pour tous les processeurs virtuels, vous devez effectuer les opérations suivantes :

-   Vérifier que l’hôte n’est pas surchargé

-   Déterminer si la charge de travail peut tirer parti d’un plus grand nombre de processeurs virtuels

-   Affecter plus de processeurs virtuels à la machine virtuelle

Si le **processeur virtuel de l’hyperviseur Hyper-V (nom de la machine virtuelle : VP x)\\%** du compteur d’exécution total est supérieur à 90% pour certains processeurs virtuels, mais pas tous, vous devez effectuer les opérations suivantes :

-   Si votre charge de travail reçoit beaucoup de ressources réseau, vous devez envisager d’utiliser vRSS.

-   Si les machines virtuelles n’exécutent pas Windows Server 2012 R2, vous devez ajouter d’autres cartes réseau.

-   Si votre charge de travail est gourmande en stockage, vous devez activer la topologie NUMA virtuelle et ajouter d’autres disques virtuels.

Si le **processeur virtuel de la racine de l’hyperviseur Hyper-V (VP racine x)\\%** du compteur d’exécution total est supérieur à 90% pour certains, toutefois, tous les processeurs virtuels et le compteur **processeur (x)\\% temps d’interruption et processeur (x)\\% temps DPC** s’ajoutent approximativement à la valeur du **processeur virtuel racine (VP racine x)\\% total** du compteur Runtime, vous devez vous assurer que l’installation de la carte réseau est activée sur les cartes réseau.

## <a name="memory-bottlenecks"></a>Goulots d’étranglement de mémoire

Voici quelques scénarios courants qui peuvent provoquer des goulots d’étranglement de mémoire :

-   L’hôte ne répond pas.

-   Impossible de démarrer les machines virtuelles.

-   Les machines virtuelles manquent de mémoire.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Mémoire\\mégaoctets disponibles

-   L’équilibreur de Mémoire dynamique Hyper-V (\*)\\de la mémoire disponible

Vous pouvez utiliser les compteurs de performances suivants à partir de la machine virtuelle :

-   Mémoire\\mégaoctets disponibles

Si la **mémoire\\mégaoctets disponibles** et que l' **équilibreur Hyper-V mémoire dynamique (\*)\\compteurs de mémoire disponibles** sont bas sur l’ordinateur hôte, vous devez arrêter les services non essentiels et migrer un ou plusieurs ordinateurs virtuels vers un autre hôte.

Si la **mémoire\\compteur mégaoctets disponibles** est insuffisante sur la machine virtuelle, vous devez attribuer plus de mémoire à la machine virtuelle. Si vous utilisez Mémoire dynamique, vous devez augmenter le paramètre de mémoire maximale.

## <a name="network-bottlenecks"></a>Goulots d’étranglement du réseau

Voici quelques scénarios courants qui peuvent entraîner des goulots d’étranglement du réseau :

-   L’hôte est lié au réseau.

-   L’ordinateur virtuel est lié au réseau.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Interface réseau (*nom de la carte réseau*)\\octets/s

Vous pouvez utiliser les compteurs de performances suivants à partir de la machine virtuelle :

-   Carte réseau virtuelle Hyper-V (*nom de l’ordinateur virtuel&lt;GUID&gt;* )\\octets/s

Si le compteur octets de la **carte réseau physique/s** est supérieur ou égal à 90% de la capacité, vous devez ajouter des cartes réseau supplémentaires, migrer des ordinateurs virtuels vers un autre ordinateur hôte et configurer la qualité de service réseau.

Si le compteur **octets/s de la carte réseau virtuel Hyper-V** est supérieur ou égal à 250 Mbits/s, vous devez ajouter des cartes réseau associées supplémentaires dans l’ordinateur virtuel, activer vRSS et utiliser SR-IOV.

Si vos charges de travail ne peuvent pas respecter leur latence réseau, activez SR-IOV pour présenter les ressources de carte réseau physique à la machine virtuelle.

## <a name="storage-bottlenecks"></a>Goulots d’étranglement de stockage

Voici quelques scénarios courants qui peuvent entraîner des goulots d’étranglement de stockage :

-   Les opérations de l’hôte et de l’ordinateur virtuel sont lentes ou expirent.

-   L’ordinateur virtuel est lent.

Vous pouvez utiliser les compteurs de performances suivants à partir de l’hôte :

-   Disque physique (*lettre de disque*)\\moyenne disque s/lecture

-   Disque physique (*lettre de disque*)\\moyenne disque s/écriture

-   Disque physique (*lettre de disque*)\\longueur moyenne de file d’attente de lecture sur le disque

-   Disque physique (*lettre de disque*)\\longueur moyenne de file d’attente d’écriture sur le disque

Si les latences sont constamment supérieures à 50 ms, vous devez effectuer les opérations suivantes :

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
