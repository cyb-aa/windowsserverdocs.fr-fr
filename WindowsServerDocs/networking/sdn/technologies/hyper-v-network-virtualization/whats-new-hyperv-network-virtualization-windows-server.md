---
title: Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016
description: Cette rubrique fournit des informations sur les nouvelles fonctionnalités de virtualisation de réseau Hyper-V dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: 0954768944e44848debfbb7fb752a13ca47031c2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit les fonctionnalités de virtualisation de réseau Hyper-V (HNV) qui sont nouvelles ou modifiées dans Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Dans HNV, les mises à jour  
HNV offre une prise en charge améliorée dans les domaines suivants:  
  
|Caractéristiques et fonctionnalités|Nouvelles ou améliorées|Description|  
|--------------------------|-------------------|---------------|  
|[Programmable commutateur Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nouveau|La stratégie HNV est programmable par le biais du contrôleur de réseau Microsoft.|  
|[Prise en charge de l’encapsulation VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nouveau|HNV prend désormais en charge l’encapsulation VXLAN.|  
|[Interopérabilité des logiciels équilibreur de charge (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nouveau|HNV est entièrement intégré à l’équilibrage de charge du logiciel Microsoft.|  
|[En-têtes Ethernet IEEE conformes](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Améliorée|Conforme aux normes Ethernet IEEE|  
  
### <a name="SDN"></a>Programmable commutateur Hyper-V  
HNV est un bloc de construction fondamental de la solution logicielle définie de mise en réseau (SDN) de la mise à jour de Microsoft et est entièrement intégré à la pile SDN.  
  
Nouveau contrôleur de réseau de Microsoft pousse les stratégies HNV vers le bas pour un Agent hôte en cours d’exécution sur chaque ordinateur hôte à l’aide d’ouvrir vSwitch protocole de gestion de base de données (OVSDB) en tant qu’Interface SouthBound (essai). L’Agent hôte stocke cette stratégie à l’aide d’une personnalisation de la [schéma VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) et des programmes de règles de flux complexe en un moteur de flux performante dans le commutateur Hyper-V.  
  
Le moteur de flux à l’intérieur du commutateur Hyper-V est le même moteur utilisé dans Microsoft Azure&trade;, reconnue à une évolutivité dans le cloud public Microsoft Azure. En outre, la pile SDN entière de via le contrôleur de réseau et de fournisseur de ressources réseau (détails bientôt disponible) est cohérente avec Microsoft Azure, garantissant ainsi la puissance du cloud public Microsoft Azure à notre entreprise et l’hébergement clients de fournisseur de service.  
  
> [!NOTE]  
> Pour plus d’informations sur OVSDB, voir [RFC 7047](http://www.rfc-editor.org/info/rfc7047).  
  
Le commutateur Hyper-V prend en charge les deux règles de flux avec et sans état basés sur le moteur de flux simple «correspond à action» au sein de Microsoft.  
 
![Commutateur de Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Prise en charge de l’encapsulation VXLAN  
Virtual eXtensible réseau Local (VXLAN - [7348 RFC](http://www.rfc-editor.org/info/rfc7348)) protocole a été largement adopté sur le marché, avec prise en charge des fournisseurs tels que Cisco, Brocade, Dell, HP et d’autres personnes. HNV prend également en charge ce schéma d’encapsulation l’utilisation du mode de distribution MAC via le contrôleur de réseau Microsoft mappages de programme pour le client superposition adresses IP de réseau (adresse client ou autorité de certification) pour les adresses IP du réseau physique sous-jacent (adresse fournisseur ou PA). NVGRE et la décharge des tâches VXLAN sont pris en charge pour améliorer les performances par le biais des pilotes tiers.  
  
### <a name="SLB"></a>Interopérabilité des logiciels équilibreur de charge (SLB)  
Windows Server 2016 inclut un équilibreur de charge logicielle (SLB) avec prise en charge complète pour le trafic réseau virtuel et une interaction transparente avec HNV. Le SLB est implémentée par le biais du moteur de flux performante dans le commutateur v plan de données et contrôlée par le contrôleur de réseau pour IP virtuelle (VIP) / dynamique mappages IP (DIP).  
  
### <a name="L2"></a>En-têtes Ethernet IEEE conformes  
HNV implémente les en-têtes Ethernet L2 appropriés afin d’assurer l’interopérabilité avec des équipements virtuels et physiques tiers qui dépendent de protocoles standard. Microsoft garantit que tous les paquets transmis des valeurs conformes dans tous les champs pour assurer cette interopérabilité. En outre, la prise en charge pour les trames (MTU > 1780) dans le réseau physique L2 devront prendre en compte pour un paquet surcharge introduit par les protocoles d’encapsulation (NVGRE, VXLAN) tout en garantissant des machines virtuelles attachés à un réseau virtuel HNV maintenir une MTU 1514.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble de la virtualisation de réseau Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Détails techniques sur la virtualisation de réseau Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Logiciel défini de mise en réseau](../../Software-Defined-Networking--SDN-.md)  
  