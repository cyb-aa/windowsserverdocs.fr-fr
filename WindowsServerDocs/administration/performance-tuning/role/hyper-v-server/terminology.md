---
title: Terminologie Hyper-V
description: Terminologie Hyper-v utile dans le réglage des performances d’Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 88aaebaac9161849fefe8116a1115eb628bcbf9e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851762"
---
# <a name="hyper-v-terminology"></a>Terminologie Hyper-V
Cette section résume la terminologie clé propre à la technologie des machines virtuelles utilisée dans cette rubrique de réglage des performances :

| Terme        | Définition           |
| ------------- |:------------|
|*partition enfant* | Tout ordinateur virtuel créé par la partition racine.|
|*virtualisation des appareils* | Mécanisme qui permet à une ressource matérielle d’être abstraite et partagée entre plusieurs consommateurs.|
|*appareil émulé*|Appareil virtualisé qui imite un périphérique matériel physique réel afin que les invités puissent utiliser les pilotes typiques pour ce périphérique matériel.|
|*révération*|Optimisation d’un système d’exploitation invité pour qu’il prenne en charge les environnements de machines virtuelles et ajuste son comportement pour les ordinateurs virtuels.|
|*courriels*|Logiciel qui s’exécute dans une partition. Il peut s’agir d’un système d’exploitation complet ou d’un petit noyau à usage spécial. L’hyperviseur est indépendant de l’invité.|
|*technologie*|Couche de logiciels qui se trouve au-dessus du matériel et au-dessous d’un ou de plusieurs systèmes d’exploitation. Son travail principal consiste à fournir des environnements d’exécution isolés appelés partitions. Chaque partition possède son propre ensemble de ressources matérielles virtualisées (unité centrale de traitement ou UC, mémoire et périphériques). L’hyperviseur contrôle et régit l’accès au matériel sous-jacent.|
|*processeur logique*| Unité de traitement qui gère un thread d’exécution (flux d’instructions). Il peut y avoir un ou plusieurs processeurs logiques par cœur de processeur et un ou plusieurs cœurs par socket de processeur.|
| *accès direct au disque*|Représentation d’un disque physique entier en tant que disque virtuel au sein de l’invité. Les données et les commandes sont transmises au disque physique (via la pile de stockage native de la partition racine) sans traitement intermédiaire par la pile virtuelle.|
|*partition racine*|La partition racine qui est créée en premier et possède toutes les ressources qui ne le sont pas, y compris la plupart des appareils et de la mémoire système. La partition racine héberge la pile de virtualisation et crée et gère les partitions enfants.|
|*Appareil spécifique à Hyper-V*|Un appareil virtualisé sans matériel physique analogique, de sorte que les invités peuvent avoir besoin d’un pilote (client du service de virtualisation) sur cet appareil spécifique à Hyper-V. Le pilote peut utiliser le bus VMBus pour communiquer avec le logiciel de l’appareil virtualisé dans la partition racine.|
|*ordinateur virtuel*|Ordinateur virtuel créé par l’émulation de logiciel et ayant les mêmes caractéristiques qu’un ordinateur réel.|
| *commutateur de réseau virtuel*|(également appelé commutateur virtuel) Version virtuelle d’un commutateur de réseau physique. Il est possible de configurer un réseau virtuel pour fournir à un ou plusieurs ordinateurs virtuels un accès à des ressources locales ou réseau externes.|
|*processeur virtuel*|Abstraction virtuelle d’un processeur qui est planifiée pour s’exécuter sur un processeur logique. Un ordinateur virtuel peut avoir un ou plusieurs processeurs virtuels.|
|*client du service de virtualisation (VSC)*|Module logiciel qu’un invité charge pour consommer une ressource ou un service. Pour les périphériques d’e/s, le client du service de virtualisation peut être un pilote de périphérique chargé par le noyau du système d’exploitation.|
| *fournisseur de services de virtualisation (VSP)*|  Fournisseur exposé par la pile de virtualisation dans la partition racine qui fournit des ressources ou des services tels que des e/s à une partition enfant.|
| *pile de virtualisation*|Collection de composants logiciels dans la partition racine qui fonctionnent ensemble pour prendre en charge les ordinateurs virtuels. La pile de virtualisation fonctionne avec et se trouve au-dessus de l’hyperviseur. Il fournit également des fonctionnalités de gestion.|
|*VMBus*|Mécanisme de communication basé sur le canal utilisé pour la communication entre les partitions et l’énumération des appareils sur les systèmes avec plusieurs partitions virtualisées actives. Le VMBus est installé avec les Services d’intégration Hyper-V.|

## <a name="see-also"></a>Voir aussi

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
