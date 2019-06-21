---
title: Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016
description: Cette rubrique fournit des informations sur les nouvelles fonctionnalités de virtualisation de réseau Hyper-V dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0254275a-0a77-40a9-b68a-1029284c03fe
ms.author: pashort
author: shortpatti
ms.date: 03/19/2018
ms.openlocfilehash: c87ccfba7b9ccc77646f58ade2853766524e67b1
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284041"
---
# <a name="whats-new-in-hyper-v-network-virtualization-in-windows-server-2016"></a>Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les fonctionnalités de virtualisation de réseau Hyper-V (HNV) qui sont nouvelles ou modifiées dans Windows Server 2016.  
  
## <a name="BKMK_IPAM2012R2"></a>Dans HNV, les mises à jour  
HNV offre une prise en charge améliorée dans les domaines suivants :  
  
|Fonctionnalité/fonction|Nouveauté ou amélioration|Description|  
|--------------------------|-------------------|---------------|  
|[Programmable commutateur Hyper-V](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SDN)|Nouveau|Stratégie HNV est programmable via le contrôleur de réseau Microsoft.|  
|[Prise en charge de l’encapsulation VXLAN](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#VXLAN)|Nouveau|HNV prend désormais en charge l’encapsulation de VXLAN.|  
|[Interopérabilité des logiciels Load Balancer (SLB)](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#SLB)|Nouveau|HNV est entièrement intégré à l’équilibrage de charge du logiciel Microsoft.|  
|[En-têtes IEEE Ethernet conformes](../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/../../../sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md#L2)|Amélioration|Conforme aux normes IEEE Ethernet|  
  
### <a name="SDN"></a>Programmable commutateur Hyper-V  
HNV est un bloc de construction fondamental de la solution de mise en réseau SDN (Software Defined) de la mise à jour de Microsoft et est entièrement intégré à la pile SDN.  
  
Nouveau contrôleur de réseau de Microsoft exécute un push de stratégies HNV jusqu'à un Agent hôte en cours d’exécution sur chaque hôte à l’aide de commutateur virtuel Open protocole de gestion de base de données (OVSDB) en tant qu’Interface SouthBound (essai). L’Agent hôte stocke cette stratégie à l’aide d’une personnalisation de la [schéma VTEP](https://github.com/openvswitch/ovs/blob/master/vtep/vtep.ovsschema) et des programmes de règles de flux complexes en un moteur de flux performantes dans le commutateur Hyper-V.  
  
Le moteur de flux à l’intérieur du commutateur Hyper-V est le même moteur utilisé dans Microsoft Azure&trade;, ce qui a fait ses preuves à très grande échelle, dans le cloud public Microsoft Azure. En outre, la pile SDN entière par le biais du contrôleur de réseau et le fournisseur de ressources réseau (détails bientôt disponible) est cohérente avec Microsoft Azure, ce qui assure la puissance du cloud public Microsoft Azure pour notre entreprise et le service d’hébergement clients du fournisseur.  
  
> [!NOTE]  
> Pour plus d’informations sur OVSDB, consultez [RFC 7047](https://www.rfc-editor.org/info/rfc7047).  
  
Le commutateur Hyper-V prend en charge les deux règles de flux avec et sans état basés sur le moteur de flux simple 'correspond à action » au sein de Microsoft.  
 
![Commutateur de Windows Server 2016 Hyper-V](../../../media/what-s-new-in-hyper-v-network-virtualization-in-windows-server/HNVOverview.png)  
  
### <a name="VXLAN"></a>Prise en charge de l’encapsulation VXLAN  
Le Virtual eXtensible réseau Local (VXLAN - [RFC 7348](https://www.rfc-editor.org/info/rfc7348)) protocole a été largement adopté sur le marché, avec prise en charge des fournisseurs comme Cisco, Brocade, Dell, HP et autres. HNV prend également en charge ce schéma d’encapsulation à l’aide du mode de distribution MAC via le contrôleur de réseau Microsoft pour les mappages de programme de locataire superposition réseau adresses IP (adresse client ou autorité de certification) pour les adresses IP du réseau physique sous-jacent (fournisseur Adresse ou PA). NVGRE et la décharge des tâches VXLAN sont pris en charge pour améliorer les performances par le biais des pilotes tiers.  
  
### <a name="SLB"></a>Interopérabilité des logiciels Load Balancer (SLB)  
Windows Server 2016 inclut un équilibreur de charge logiciel (SLB) avec prise en charge complète pour le trafic de réseau virtuel et une interaction transparente avec HNV. L’équilibreur SLB est implémenté via le moteur de flux dans le commutateur v plan de données performante et contrôlé par le contrôleur de réseau pour l’adresse IP virtuelle (VIP) / dynamique mappages d’adresses IP (DIP).  
  
### <a name="L2"></a>En-têtes IEEE Ethernet conformes  
HNV implémente les bons en-têtes L2 Ethernet pour assurer l’interopérabilité avec les appliances physiques et virtuels tiers qui dépendent des protocoles standard. Microsoft garantit que tous les paquets transmis ont des valeurs conformes dans tous les champs pour vous assurer cette interopérabilité. En outre, la prise en charge pour les trames Jumbo (MTU 1780 >) dans le réseau physique de L2 seront nécessaires pour refléter les paquets surcharge introduit par les protocoles d’encapsulation (NVGRE, VXLAN) tout en garantissant des invités à mettre à jour les Machines virtuelles attachées à un réseau virtuel HNV un 1514 MTU.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d'ensemble de la virtualisation de réseau Hyper-V](hyperv-network-virtualization-overview-windows-server.md)  
  
-   [Détails techniques sur la virtualisation de réseau Hyper-V](hyperv-network-virtualization-technical-details-windows-server.md)  
  
-   [Mise en réseau SDN (Software Defined Networking)](../../Software-Defined-Networking--SDN-.md)  
  