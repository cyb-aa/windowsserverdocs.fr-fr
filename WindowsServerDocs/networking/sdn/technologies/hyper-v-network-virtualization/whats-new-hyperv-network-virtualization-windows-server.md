---
title: Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016
description: Cette rubrique fournit des informations sur les nouvelles fonctionnalités de la virtualisation de réseau Hyper-V dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: lizross
author: eross-msft
ms.date: 03/19/2018
ms.openlocfilehash: a91f54d5d4528ad0ee592f40902c856b94a276b7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317122"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit la fonctionnalité de virtualisation de réseau Hyper-V (HNV) qui est nouvelle ou modifiée dans Windows Server 2016.  
  
## <a name="updates-in-hnv"></a><a name="BKMK_IPAM2012R2"></a>Mises à jour dans HNV  
HNV offre une prise en charge améliorée dans les domaines suivants :  
  
|Fonctionnalité/fonction|Nouveauté ou amélioration|Description|  
|--------------------------|-------------------|---------------|  
|[Commutateur Hyper-V programmable](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nouveau|La stratégie HNV est programmable par le biais du contrôleur de réseau Microsoft.|  
|[Prise en charge de l’encapsulation VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nouveau|HNV prend désormais en charge l’encapsulation VXLAN.|  
|[Interopérabilité des Load Balancer logicielles (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nouveau|HNV est entièrement intégré au logiciel Microsoft Load Balancer.|  
|[En-têtes Ethernet IEEE conformes](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Amélioration|Conforme aux normes IEEE Ethernet|  
  
### <a name="programmable-hyper-v-switch"></a><a name="SDN"></a>Commutateur Hyper-V programmable  
HNV est un bloc de construction fondamental de la solution SDN (Software Defined Networking) mise à jour de Microsoft et est entièrement intégré à la pile SDN.  
  
Le nouveau contrôleur de réseau de Microsoft pousse les stratégies de HNV vers un agent hôte s’exécutant sur chaque hôte à l’aide du protocole OVSDB (Open vSwitch Database Management Protocol) en tant qu’interface SouthBound (SBI). L’agent hôte stocke cette stratégie à l’aide d’une personnalisation du [schéma VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) et des règles de Flow complexes dans un moteur de workflow performant du commutateur Hyper-V.  
  
Le moteur de Flow à l’intérieur du commutateur Hyper-V est le même que celui utilisé dans Microsoft Azure&trade;, qui a été prouvé à l’échelle Hyper-V dans le cloud public Microsoft Azure. En outre, l’ensemble des SDN empilés dans le contrôleur de réseau et le fournisseur de ressources réseau (les détails sont bientôt disponibles) sont cohérents avec Microsoft Azure, mettant ainsi la puissance du Microsoft Azure cloud public à notre service d’entreprise et d’hébergement clients du fournisseur.  
  
> [!NOTE]  
> Pour plus d’informations sur OVSDB, consultez la [RFC 7047](https://www.rfc-editor.org/info/rfc7047).  
  
Le commutateur Hyper-V prend en charge les règles de Flow sans État et avec état à l’aide d’une simple « action de correspondance » au sein du moteur de flow de Microsoft.  
 
![Commutateur Hyper-V de Windows Server 2016](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="vxlan-encapsulation-support"></a><a name="VXLAN"></a>Prise en charge de l’encapsulation VXLAN  
Le protocole VXLAN- [RFC 7348](https://www.rfc-editor.org/info/rfc7348)(Virtual extensible Local Area Network) a été largement adopté sur le marché, avec la prise en charge de fournisseurs comme Cisco, Brocade, Dell, HP et d’autres. HNV prend également en charge ce schéma d’encapsulation à l’aide du mode de distribution MAC via le contrôleur de réseau Microsoft pour programmer les mappages des adresses IP de réseau de superposition de locataire (adresse client ou autorité de certification) aux adresses IP du réseau Underlay physique (fournisseur Adresse, ou PA). Les déchargements de tâches NVGRE et VXLAN sont pris en charge pour améliorer les performances par le biais de pilotes tiers.  
  
### <a name="software-load-balancer-slb-interoperability"></a><a name="SLB"></a>Interopérabilité des Load Balancer logicielles (SLB)  
Windows Server 2016 comprend un équilibreur de charge logiciel (SLB) avec une prise en charge complète du trafic de réseau virtuel et une interaction transparente avec HNV. Le SLB est implémenté par le biais du moteur de workflow performant dans le commutateur v du plan de données et contrôlé par le contrôleur de réseau pour les mappages d’adresses IP virtuelles/IP dynamiques (DIP).  
  
### <a name="compliant-ieee-ethernet-headers"></a><a name="L2"></a>En-têtes Ethernet IEEE conformes  
HNV implémente des en-têtes Ethernet L2 corrects pour garantir l’interopérabilité avec des appliances virtuelles et physiques tierces qui dépendent des protocoles standard. Microsoft s’assure que tous les paquets transmis ont des valeurs conformes dans tous les champs pour garantir cette interopérabilité. En outre, la prise en charge des trames Jumbo (MTU > 1780) dans le réseau physique L2 est nécessaire pour prendre en compte la surcharge de paquets introduite par les protocoles d’encapsulation (NVGRE, VXLAN) tout en veillant à ce que les machines virtuelles invitées connectées à un réseau virtuel HNV maintiennent un 1514 MTU.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d'ensemble de la virtualisation de réseau Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Détails techniques sur la virtualisation de réseau Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Mise en réseau SDN (Software Defined Networking)](../../Software-Defined-Networking--SDN-.md)  
  