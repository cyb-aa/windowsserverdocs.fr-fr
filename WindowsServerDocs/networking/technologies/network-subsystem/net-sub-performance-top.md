---
title: Réglage des performances du sous-système réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: 66f4401235e8eccfe0ab6e10f86cddb15386621c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854752"
---
# <a name="network-subsystem-performance-tuning"></a>Réglage des performances du sous-système réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble du sous-système réseau et des liens vers d’autres rubriques de ce guide.

>[!NOTE]
>En plus de cette rubrique, les sections suivantes de ce guide fournissent des recommandations en matière de réglage des performances pour les périphériques réseau et la pile réseau.
> - [Choix d’une carte réseau](net-sub-choose-nic.md)
> - [Configurer l’ordre des interfaces réseau](net-sub-interface-metric.md)
> - [Réglage des performances des cartes réseau](net-sub-performance-tuning-nics.md)
> - [Compteurs de performance liés au réseau](net-sub-performance-counters.md)
> - [Outils d’analyse des performances pour les charges de travail de réseau](net-sub-performance-tools.md)

Le réglage des performances du sous-système réseau, en particulier pour les charges de travail gourmandes en réseau, peut impliquer chaque couche de l’architecture réseau, également appelée pile réseau. Ces couches sont généralement réparties dans les sections suivantes.

1. **Interface réseau**. Il s’agit de la couche la plus basse dans la pile réseau, qui contient le pilote réseau qui communique directement avec la carte réseau.

2. **NDIS (Network Driver Interface Specification)** . NDIS expose des interfaces pour le pilote situé sous celui-ci et pour les couches qui le précèdent, telles que la pile de protocoles.
  
3. **Pile de protocoles**. La pile de protocoles implémente des protocoles tels que TCP/IP et UDP/IP. Ces couches exposent l’interface de la couche de transport pour les couches supérieures.
  
4. **Pilotes système**. Il s’agit généralement de clients qui utilisent une interface TDX (transport Data extension) ou Winsock kernel (WSK) pour exposer des interfaces à des applications en mode utilisateur. L’interface WSK a été introduite dans Windows Server 2008 et Windows&reg; Vista, et elle est exposée par AFD. sys. L’interface améliore les performances en éliminant le basculement entre le mode utilisateur et le mode noyau.
  
5. **Applications en mode utilisateur**. Il s’agit généralement de solutions Microsoft ou d’applications personnalisées.

Le tableau ci-dessous fournit une illustration verticale des couches de la pile réseau, y compris des exemples d’éléments qui s’exécutent dans chaque couche.  

![Couches de pile réseau](../../media/Network-Subsystem/network-layers.jpg)

