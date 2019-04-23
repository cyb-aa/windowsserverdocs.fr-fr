---
title: Terminologie Hyper-V
description: Terminologie Hyper-v utile de réglage des performances d’Hyper-V
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bc970633ff24827207eb3a27e282656f2486a6eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841140"
---
# <a name="hyper-v-terminology"></a>Terminologie Hyper-V
Cette section récapitule la terminologie clé spécifique à la technologie de machine virtuelle qui est utilisée tout au long de cette rubrique d’optimisation des performances :

| Terme        | Définition           |
| ------------- |:------------|
|*partition enfant* | N’importe quel ordinateur virtuel qui est créé par la partition racine.|
|*virtualisation de l’appareil* | Un mécanisme qui permet un matériel ressources être abstraits et partagés entre plusieurs consommateurs.|
|*APPAREIL émulé*|Un appareil virtualisé qui simule un appareil matériel physique afin que les invités peuvent utiliser les pilotes classiques pour ce périphérique matériel.|
|*enlightenment*|Une optimisation pour un système d’exploitation pour la rendre prenant en charge des environnements d’ordinateur virtuel et ajuster son comportement pour les machines virtuelles.|
|*guest*|Logiciel qui s’exécute dans une partition. Il peut être un système d’exploitation complet ou un noyau de petite taille, à usage spécial. L’hyperviseur est indépendant de l’invité.|
|*hypervisor*|Une couche logicielle qui se trouve au-dessus du matériel et au-dessous d’un ou plusieurs systèmes d’exploitation. Son travail principal consiste à fournir des environnements d’exécution isolés appelés partitions. Chaque partition possède son propre ensemble de ressources d’un matériel virtualisé (unité centrale ou du processeur, mémoire et des périphériques). L’hyperviseur contrôle et régit l’accès au matériel sous-jacent.|
|*processeur logique*| Une unité de traitement qui gère un thread d’exécution (flux d’instructions). Il peut y avoir un ou plusieurs processeurs logiques par cœur de processeur et un ou plusieurs noyaux par socket de processeur.|
| *accès au disque de transfert direct*|Une représentation sous forme d’un disque physique entier comme un disque virtuel au sein de l’invité. Les données et les commandes sont transmises sur le disque physique (via la pile de stockage natif de la partition racine) avec aucun traitement concerné par la pile virtuelle.|
|*partition racine*|La partition racine qui est créée en premier et possède toutes les ressources que l’hyperviseur n’effectue pas, y compris la plupart des appareils et la mémoire système. La partition racine héberge la pile de virtualisation et crée et gère les partitions enfants.|
|*Appareil de spécifique à Hyper-V*|Un appareil virtualisé avec aucun analogique du matériel physique, c’est le cas invités peuvent devoir un pilote (client de service de virtualisation) cet appareil spécifique à Hyper-V. Le pilote peut utiliser virtual machine bus VMBus pour communiquer avec le logiciel d’appareil virtualisé dans la partition racine.|
|*machine virtuelle*|Un ordinateur virtuel qui a été créé par émulation logicielle et a les mêmes caractéristiques que réel de l’ordinateur.|
| *commutateur de réseau virtuel*|(également appelée un commutateur virtuel) Une version virtuelle d’un commutateur réseau physique. Il est possible de configurer un réseau virtuel pour fournir à un ou plusieurs ordinateurs virtuels un accès à des ressources locales ou réseau externes.|
|*processeur virtuel*|Une abstraction virtuelle d’un processeur qui est planifiée pour s’exécuter sur un processeur logique. Une machine virtuelle peut avoir un ou plusieurs processeurs virtuels.|
|*client de service de virtualisation (VSC)*|Un module logiciel un invité se charge pour utiliser une ressource ou un service. Pour les périphériques d’e/s, le client de service de virtualisation peut être un pilote de périphérique qui charge le noyau du système d’exploitation.|
| *fournisseur de services de virtualisation (VSP)*|  Un fournisseur est exposé par la pile de virtualisation dans la partition racine qui fournit des ressources ou des services tels que des e/s à une partition enfant.|
| *pile de virtualisation*|Une collection de composants logiciels dans la partition racine qui fonctionnent ensemble pour prendre en charge des machines virtuelles. La pile de virtualisation fonctionne avec et se trouve au-dessus de l’hyperviseur. Il fournit également des fonctionnalités de gestion.|
|*VMBus*|Mécanisme de communication basée sur le canal utilisé pour l’énumération de communication et l’appareil entre les partitions sur les systèmes avec plusieurs partitions virtualisées actives. Le VMBus est installé avec les Services d’intégration Hyper-V.|

## <a name="see-also"></a>Voir aussi

-   [Architecture Hyper-V](architecture.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances du processeur de Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Stockage Hyper-V les performances d’e/s](storage-io-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Machines virtuelles Linux](linux-virtual-machine-considerations.md)
