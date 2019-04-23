---
title: Réglage des performances du sous-système réseau
description: Cette rubrique fait partie du guide de réglage de performances du sous-système de réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0706c6ddbb678eacd3e609cfad3ccdda943fbd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857410"
---
# <a name="network-subsystem-performance-tuning"></a>Réglage des performances du sous-système réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour une vue d’ensemble du sous-système réseau et les liens vers d’autres rubriques dans ce guide.

>[!NOTE]
>Outre cette rubrique, les sections suivantes de ce guide fournissent des recommandations de réglage des performances pour les périphériques réseau et la pile réseau.
> - [Choix d’une carte réseau](net-sub-choose-nic.md)
> - [Configurer l’ordre des Interfaces réseau](net-sub-interface-metric.md)
> - [Cartes réseau réglage de performances](net-sub-performance-tuning-nics.md)
> - [Compteurs de performances liés au réseau](net-sub-performance-counters.md)
> - [Outils de performances de charges de travail réseau](net-sub-performance-tools.md)

Le sous-système de réseau, en particulier pour les charges de travail intensives, le réglage des performances peuvent impliquer chaque couche de l’architecture réseau, également appelé la pile réseau. Ces couches sont divisées selon les sections suivantes.

1. **Interface réseau**. Ceci est la couche la plus basse dans la pile réseau et contient le pilote réseau qui communique directement avec la carte réseau.

2. **Network Driver Interface Specification (NDIS)**. NDIS expose des interfaces pour le pilote en dessous et pour les couches au-dessus de lui, telles que la pile de protocole.
  
3. **Pile de protocole**. La pile de protocole implémente les protocoles tels que TCP/IP et UDP/IP. Ces couches exposent l’interface de couche de transport pour les couches situées au-dessus.
  
4. **Les pilotes système**. Il s’agit en général, les clients qui utilisent une extension de données de transport (TDX) ou l’interface Winsock Kernel (WSK) pour exposer des interfaces pour les applications en mode utilisateur. L’interface WSK a été introduite dans Windows Server 2008 et Windows&reg; Vista et il est exposé par le paramètre AFD.sys. L’interface améliore les performances en éliminant le passage en mode utilisateur et mode noyau.
  
5. **Applications en Mode utilisateur**. Il s’agit généralement des solutions de Microsoft ou des applications personnalisées.

Le tableau ci-dessous fournit une illustration verticale des couches de la pile réseau, y compris des exemples d’éléments qui s’exécutent dans chaque couche.  

![Couches de pile de réseau](../../media/Network-Subsystem/network-layers.jpg)

