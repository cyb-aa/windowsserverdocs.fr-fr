---
title: Technologies SDN
description: Les rubriques de cette section fournissent une vue d’ensemble et des informations techniques sur les technologies de mise en réseau définies par logiciel qui sont incluses dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: 040568dd696c4dee665de415d23ce9a7cdc8a711
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870066"
---
# <a name="sdn-technologies"></a>Technologies SDN

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Les rubriques de cette section fournissent une vue d’ensemble et des informations techniques sur les technologies de mise en réseau définies par logiciel qui sont incluses dans Windows Server 2016.  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[Contrôleur de réseau](network-controller/Network-Controller.md)

Le contrôleur de réseau fournit un point d’automatisation centralisé et programmable pour gérer, configurer, surveiller et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de donnes. Avec le contrôleur de réseau, vous pouvez automatiser la configuration de l’infrastructure réseau au lieu d’effectuer une configuration manuelle des appareils et des services réseau. 

Le contrôleur de réseau est un serveur hautement disponible et évolutif et fournit deux interfaces de programmation d’applications (API) :

1. **API Southbound** : permet au contrôleur de réseau de communiquer avec le réseau.
2. **API Northbound** : vous permet de communiquer avec le contrôleur de réseau.

Vous pouvez utiliser Windows PowerShell, l’API REST, ou une application de gestion pour gérer l’infrastructure réseau physique et virtuelle suivante :

- Ordinateurs virtuels Hyper-V et commutateurs virtuels 
- Commutateurs de réseau physique 
- Routeurs de réseau physique 
- Logiciel pare-feu 
- Passerelles VPN, y compris les passerelles mutualisées du service d’accès à distance (RAS) 
- Équilibreurs de charge 
  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualisation de réseau Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

La virtualisation de réseau Hyper-V (HNV) vous permet d’abstraire vos applications et charges de travail du réseau physique à l’aide de réseaux virtuels. Les réseaux virtuels fournissent l’isolation mutualisée nécessaire moyennant une exécution sur une structure réseau physique partagée, ce qui augmente l’utilisation des ressources. Pour que vous puissiez transférer vos investissements existants, vous pouvez configurer des réseaux virtuels sur des engrenages de mise en réseau existants. En outre, les réseaux virtuels sont compatibles avec les réseaux locaux virtuels (VLAN).
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutateur virtuel Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

Le commutateur virtuel Hyper-V est un commutateur réseau Ethernet de couche 2, qui est disponible dans le Gestionnaire Hyper-V après l’installation du rôle de serveur Hyper-V. Le commutateur offre des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique à la fois. En outre, le commutateur virtuel Hyper-V assure l’application de la stratégie pour la sécurité, l’isolation et les niveaux de service.
  
Vous pouvez également déployer le commutateur virtuel Hyper-V avec switch Embedded Teaming (SET) et l’accès direct à la mémoire à distance (RDMA). Pour plus d’informations, consultez la section [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) dans cette rubrique.

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[Service DNS interne (iDNS) pour SDN](Idns-for-Sdn.md)

Les machines virtuelles (VM) et les applications hébergées requièrent le DNS pour communiquer au sein de leurs réseaux et avec des ressources externes sur Internet. Avec iDNS, vous pouvez fournir aux locataires des services de résolution de noms DNS pour les ressources d’espace de noms local et Internet isolées. 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualisation de fonction réseau](network-function-virtualization/Network-Function-Virtualization.md)

Les appliances matérielles, telles que les équilibreurs de charge, les pare-feu, les routeurs et les commutateurs, deviennent de plus en plus des appliances virtuelles. Microsoft possède des réseaux virtualisés, des commutateurs, des passerelles, des NAT, des équilibrages de charge et des pare-feu. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux. Les appliances virtuelles sont rapidement émergentes et créent un nouveau marché. Ils continuent à générer un intérêt et à gagner de l’énergie dans les plateformes de virtualisation et les services Cloud. 
  
Les technologies de virtualisation de fonction réseau suivantes sont disponibles.  
  
-   **Les load balancer logicielles (SLB) et la traduction d’adresses réseau (NAT)** . Améliorez le débit en prenant en charge le retour au serveur direct dans lequel le trafic réseau de retour peut contourner le multiplexeur d’équilibrage de charge. Pour plus d’informations, consultez [équilibrage de la charge logicielle/(SLB/) pour SDN](network-function-virtualization/software-load-balancing-for-sdn.md).
  
-   **Pare-feu de centre de centres**. Fournissez des listes de contrôle d’accès (ACL) granulaires, vous permettant d’appliquer des stratégies de pare-feu au niveau de l’interface de la machine virtuelle ou du sous-réseau. Pour plus d’informations, consultez [vue d’ensemble du pare-feu de centre](network-function-virtualization/Datacenter-Firewall-Overview.md)de données.
  
-   **Passerelle RAS pour SDN**. Acheminer le trafic réseau entre le réseau physique et les ressources du réseau de machines virtuelles, quel que soit l’emplacement. Vous pouvez acheminer le trafic réseau au même emplacement physique ou à de nombreux emplacements différents. Pour plus d’informations, consultez [passerelle RAS pour SDN](network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>RDMA (Remote Direct Memory Access) et SET (Switch Embedded Teaming)  
Dans Windows Server 2016, vous pouvez activer RDMA sur les cartes réseau qui sont liées à un commutateur virtuel Hyper-V avec ou sans l’Association incorporée de commutateur (SET). Cela vous permet d’utiliser moins de cartes réseau lorsque vous souhaitez utiliser RDMA et définir en même temps.  
  
SET est une autre solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile SDN (Software Defined Networking) dans Windows Server 2016. SET intègre certaines fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V.  
  
L’ensemble vous permet de grouper entre une et huit cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelles basées sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.  
Les cartes réseau de membre de jeu doivent toutes être installées dans le même hôte Hyper-V physique à placer dans une équipe.  
  
En outre, vous pouvez utiliser les commandes Windows PowerShell pour activer Data Center Bridging (DCB), créer un commutateur virtuel Hyper-V avec une carte réseau virtuelle RDMA (carte réseau virtuelle) et créer un commutateur virtuel Hyper-V avec SET et RDMA cartes réseau virtuelles. Pour plus d’informations, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[Protocole BGP (Border Gateway Protocol)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
Border Gateway Protocol (BGP) est un protocole de routage dynamique qui apprend automatiquement les itinéraires entre les sites qui utilisent des connexions VPN de site à site. Par conséquent, le protocole BGP réduit la configuration manuelle des routeurs.   Quand vous configurez la passerelle RAS, le protocole BGP vous permet de gérer le routage du trafic réseau entre les réseaux d’ordinateurs virtuels et les sites distants de vos locataires.  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Équilibrage de la charge logicielle pour SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Les fournisseurs de services Cloud (CSP) et les entreprises qui déploient SDN peuvent utiliser l’équilibrage de charge logiciel (SLB) pour distribuer uniformément le trafic réseau des clients et des locataires entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité. 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Conteneurs Windows Server](Containers/Container-networking-overview.md)

Les conteneurs Windows Server sont une méthode de virtualisation de système d’exploitation légère qui sépare les applications ou les services d’autres services exécutés sur le même hôte de conteneur. Chaque conteneur possède son propre système d’exploitation, ses processus, son système de fichiers, son registre et ses adresses IP, que vous pouvez connecter à des réseaux virtuels. 

## <a name="system-center"></a>System Center

Déployez et gérez l’infrastructure SDN avec la [gestion d’ordinateurs virtuels (VMM)](https://docs.microsoft.com/system-center/vmm/) et [Operations Manager](https://docs.microsoft.com/system-center/scom/). Avec VMM, vous pouvez provisionner et gérer les ressources nécessaires pour créer et déployer des machines virtuelles et des services sur des clouds privés.  Avec Operations Manager, vous pouvez superviser les services, les appareils et les opérations au sein de votre entreprise afin d’identifier les problèmes et les résoudre immédiatement. 


---