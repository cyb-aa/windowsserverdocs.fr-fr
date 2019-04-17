---
title: Performances du sous-système réseau réglage
description: Cette rubrique fait partie du guide de réglage des performances sous-système réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7ef50335a6dcc7dc5187cc30ff1b2dc2c5cdfed
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-subsystem-performance-tuning"></a>Performances du sous-système réseau réglage

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble du sous-système réseau et les liens vers d’autres rubriques dans ce guide.

>[!NOTE]
>Outre cette rubrique, les sections suivantes de ce guide fournissent des recommandations de réglage des performances pour les périphériques réseau et la pile réseau.
> - [Choix d’une carte réseau](net-sub-choose-nic.md)
> - [Configurer l’ordre des Interfaces réseau](net-sub-interface-metric.md)
> - [Cartes réseau réglage de performance](net-sub-performance-tuning-nics.md)
> - [Compteurs de Performance liés au réseau](net-sub-performance-counters.md)
> - [Outils de performance pour les charges de travail de réseau](net-sub-performance-tools.md)

Le sous-système réseau, en particulier pour les charges de travail intensives réseau, le réglage des performances peuvent impliquer chaque couche de l’architecture de réseau, également appelé la pile réseau. Ces couches sont divisées dans les sections suivantes.

1. **Interface réseau**. Cela correspond à la couche la plus basse de la pile réseau et contient le pilote réseau qui communique directement avec la carte réseau.

2. **Network Driver Interface Specification (NDIS)**. NDIS expose des interfaces pour le pilote au-dessous de lui et pour les couches dessus, telles que la pile de protocole.
  
3. **Pile de protocole**. La pile de protocole implémente les protocoles tels que TCP/IP et UDP/IP. Ces couches exposent l’interface de couche de transport pour les couches supérieures.
  
4. **Pilotes système**. Il s’agit généralement de clients qui utilisent une extension de données de transport (TDX) ou l’interface Winsock Kernel (WSK) pour exposer des interfaces d’applications en mode utilisateur. L’interface WSK a été introduite dans Windows Server 2008 et Windows&reg; Vista et il est exposé par celui-ci. L’interface améliore les performances en éliminant le basculement entre le mode utilisateur et en mode noyau.
  
5. **Les Applications en Mode utilisateur**. Il s’agit généralement de solutions Microsoft ou des applications personnalisées.

Le tableau ci-dessous fournit une illustration verticale des couches de la pile réseau, y compris des exemples d’éléments qui s’exécutent dans chaque couche.  

![Couches de pile réseau](../../media/Network-Subsystem/network-layers.jpg)

