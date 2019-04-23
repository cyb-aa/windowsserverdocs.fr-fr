---
title: Architecture Hyper-V
description: Condsiderations architecture Hyper-v pour le réglage des performances
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fcc87b04698a44e115c8f49150fe33443f8e6a88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890280"
---
# <a name="hyper-v-architecture"></a>Architecture Hyper-V

Hyper-V propose une architecture basée sur hyperviseur de Type 1. L’hyperviseur virtualise les processeurs et la mémoire et fournit des mécanismes de la pile de virtualisation dans la partition racine pour gérer des partitions enfants (machines virtuelles) et exposer des services tels que les périphériques d’e/s pour les machines virtuelles.

La partition racine possède et a un accès direct aux périphériques d’e/s physiques. La pile de virtualisation dans la partition racine fournit un gestionnaire de mémoire pour les machines virtuelles, les API de gestion et les périphériques d’e/s virtualisés. Il implémente également des périphériques émulés tels que le contrôleur de disque integrated device electronics (IDE) et port du périphérique d’entrée PS/2 et prend en charge les périphériques synthétiques spécifiques à Hyper-V pour améliorer les performances et réduction des coûts.

![architecture d’hyperviseur Hyper-v](../../media/perftune-guide-hyperv-arch.png)

L’architecture d’e/s spécifiques à Hyper-V se compose des fournisseurs de services de virtualisation (vsp) dans les racine partition et la virtualisation clients du service (VSC) dans la partition enfant. Chaque service est exposé en tant qu’appareil via VMBus, qui agit comme un bus d’e/s et permet une communication hautes performances entre les machines virtuelles qui utilisent des mécanismes tels que la mémoire partagée. Le Gestionnaire de Plug-and-Play du système d’exploitation invité énumère ces appareils, y compris VMBus et charge les pilotes de périphériques appropriés (clients de service virtuel). Services autres que les e/s sont également exposées via cette architecture.

À compter de Windows Server 2008, les enlightments de fonctionnalités de système d’exploitation pour optimiser son comportement lorsqu’il s’exécute sur des machines virtuelles. Les avantages incluent ce qui réduit le coût de la virtualisation de mémoire, amélioration de l’évolutivité multicœur et la diminution de l’arrière-plan de l’utilisation du processeur du système d’exploitation invité.

Les sections suivantes vous suggérons de meilleures pratiques qui génèrent des performances accrues sur les serveurs exécutant le rôle Hyper-V.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances du processeur de Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Stockage Hyper-V les performances d’e/s](storage-io-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Machines virtuelles Linux](linux-virtual-machine-considerations.md)
