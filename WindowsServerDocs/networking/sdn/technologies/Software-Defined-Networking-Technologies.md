---
title: Technologies SDN
description: Les rubriques de cette section fournissent la vue d’ensemble et des informations techniques sur les technologies Sdn qui sont inclus dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.date: 02/14/2019
ms.openlocfilehash: 0fc076eaeacc98c5554f44ae6177a3a8b8286647
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034649"
---
# <a name="sdn-technologies"></a>Technologies SDN

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Les rubriques de cette section fournissent la vue d’ensemble et des informations techniques sur les technologies Sdn qui sont inclus dans Windows Server 2016.  

## <a name="network-controllernetwork-controllernetwork-controllermd"></a>[Contrôleur de réseau](network-controller/Network-Controller.md)

Le contrôleur de réseau fournit un point centralisé et programmable d’automation pour gérer, configurer, surveiller et résoudre les problèmes à la fois virtuels et infrastructure réseau physique dans votre centre de données. Avec le contrôleur de réseau, vous pouvez automatiser la configuration de l’infrastructure réseau au lieu d’effectuer une configuration manuelle de périphériques réseau et de services. 

Le contrôleur de réseau est un serveur hautement disponible et évolutif et fournit deux interfaces de programmation d’applications (API) :

1. **API southbound** – permet au contrôleur de réseau communiquer avec le réseau.
2. **L’API northbound** – vous permet de communiquer avec le contrôleur de réseau.

Vous pouvez utiliser Windows PowerShell, l’API REST Representational State Transfer () ou une application de gestion pour gérer l’infrastructure réseau physique et virtuelle suivante :

- Ordinateurs virtuels Hyper-V et commutateurs virtuels 
- Commutateurs de réseau physique 
- Routeurs de réseau physique 
- Logiciel pare-feu 
- Passerelles VPN, y compris les passerelles mutualisées Service d’accès à distance (RAS) 
- Programmes d'équilibrage de la charge 
  
## <a name="hyper-v-network-virtualizationhyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualisation de réseau Hyper-V](hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

Virtualisation de réseau Hyper-V (HNV) vous aide à abstraire vos applications et les charges de travail à partir du réseau physique à l’aide de réseaux virtuels. Les réseaux virtuels fournissent l’isolation mutualisée nécessaire moyennant une exécution sur une structure réseau physique partagée, ce qui augmente l’utilisation des ressources. Pour vous assurer que vous pouvez reporter vos investissements existants, vous pouvez configurer des réseaux virtuels sur le matériel réseau existant. En outre, les réseaux virtuels sont compatibles avec les réseaux locaux virtuels (VLAN).
  
## <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutateur virtuel Hyper-V](../../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md) 

Le commutateur virtuel Hyper-V est un logiciel Ethernet réseau commutateur de couche 2 qui est disponible dans le Gestionnaire Hyper-V après avoir installé le rôle serveur Hyper-V. Le commutateur offre des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique à la fois. Commutateur virtuel Hyper-V fournit également, l’application des stratégies de sécurité, d’isolation et niveaux de service.
  
Vous pouvez également déployer le commutateur virtuel Hyper-V avec SET Switch Embedded Teaming () et des accès de mémoire Direct à distance (RDMA). Pour plus d’informations, consultez la section [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](#remote-direct-memory-access-rdma-and-switch-embedded-teaming-set) dans cette rubrique.

## <a name="internal-dns-service-idns-for-sdnidns-for-sdnmd"></a>[Service DNS interne (IDN) pour SDN](Idns-for-Sdn.md)

Hébergé machines virtuelles (VM) et les applications nécessitent DNS communiquer au sein de leurs réseaux et avec des ressources externes sur Internet. Avec les noms de domaines internationaux, vous pouvez fournir des clients avec les services de résolution de noms DNS pour l’espace de noms local isolé et ressources Internet. 
  
## <a name="network-function-virtualizationnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualisation de réseau (fonction)](network-function-virtualization/Network-Function-Virtualization.md)

Les appliances matérielles, telles que les équilibreurs de charge, les pare-feux, routeurs et commutateurs sont plus en plus des appliances virtuelles. Microsoft a virtualisées réseaux, les commutateurs, passerelles, NAT, équilibreurs de charge et les pare-feux. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux. Appliances virtuelles sont rapidement émergentes et créer un tout nouveau marché. Ils continuent à susciter l’intérêt et de gagner du terrain dans les deux plateformes de virtualisation et de services cloud. 
  
Les technologies de virtualisation de fonction réseau suivantes sont disponibles.  
  
-   **Équilibreur de charge logiciel (SLB) et adresses réseau (NAT) de traduction**. Améliorer le débit en prenant en charge le retour Direct du serveur dans lequel le trafic réseau de retour peut contourner l’équilibrage de charge multiplexeur. Pour plus d’informations, consultez [/(SLB/) de l’équilibrage de charge logicielle pour SDN](network-function-virtualization/software-load-balancing-for-sdn.md).
  
-   **Pare-feu de centre de données**. Fournir des listes de contrôle d’accès granulaire (ACL), ce qui vous permet d’appliquer des stratégies de pare-feu au niveau de l’interface de machine virtuelle ou du sous-réseau. Pour plus d’informations, consultez [vue d’ensemble du pare-feu de centre de données](network-function-virtualization/Datacenter-Firewall-Overview.md).
  
-   **Passerelle RAS pour SDN**. Acheminer le trafic réseau entre le réseau physique et les ressources réseau de machine virtuelle, quel que soit l’emplacement. Vous pouvez acheminer le trafic réseau au même emplacement physique ou de plusieurs emplacements. Pour plus d’informations, consultez [passerelle RAS pour SDN](network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>RDMA (Remote Direct Memory Access) et SET (Switch Embedded Teaming)  
Dans Windows Server 2016, vous pouvez activer RDMA sur les cartes réseau qui sont liés à un commutateur virtuel Hyper-V avec ou sans commutateur SET (Embedded Teaming). Cela vous permet d’utiliser moins de cartes réseau lorsque vous souhaitez utiliser RDMA et SET en même temps.  
  
JEU est une autre solution association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile de mise en réseau SDN (Software Defined) dans Windows Server 2016. JEU de certaines des fonctionnalités d’association de cartes réseau s’intègre dans le commutateur virtuel Hyper-V.  
  
ENSEMBLE vous permet de regrouper entre un et huit Ethernet de cartes réseau physiques dans un ou plusieurs adaptateurs de réseau virtuel basé sur le logiciel. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.  
Toutes les cartes réseau de l’ensemble doivent être installés dans le même hôte physique Hyper-V à placer dans une équipe.  
  
En outre, vous pouvez utiliser les commandes Windows PowerShell pour activer Data Center Bridging (DCB), créez un commutateur virtuel Hyper-V avec une carte réseau virtuelle de RDMA (vNIC) et créer un commutateur virtuel Hyper-V avec des cartes réseau virtuelles RDMA et SET. Pour plus d’informations, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

## <a name="border-gateway-protocol-bgpremoteremote-accessbgpborder-gateway-protocol-bgpmd"></a>[Protocole BGP (Border Gateway Protocol)](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
  
Protocole BGP (Border Gateway) est un protocole de routage dynamique qui découvre automatiquement les itinéraires entre les sites qui utilisent des connexions VPN de site à site. Le protocole BGP réduit par conséquent, une configuration manuelle des routeurs.   Lorsque vous configurez la passerelle RAS, BGP vous permet de gérer le routage du trafic réseau entre les réseaux de machines virtuelles et les sites distants de vos clients.  
  
## <a name="software-load-balancing-slb-for-sdnnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[Équilibrage de la charge logicielle pour SDN](network-function-virtualization/software-load-balancing-for-sdn.md)
Fournisseurs de services cloud (CSP) et les entreprises qui déploient SDN peuvent utiliser l’équilibrage de charge logiciel (SLB) pour répartir uniformément le client et le trafic réseau de client client entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité. 

## <a name="windows-server-containerscontainerscontainer-networking-overviewmd"></a>[Conteneurs Windows Server](Containers/Container-networking-overview.md)

Conteneurs Windows Server constituent une méthode de la virtualisation de système d’exploitation léger séparation des applications ou services provenant d’autres services en cours d’exécution sur le même hôte de conteneur. Chaque conteneur possède son propre système d’exploitation, processus, système de fichiers, Registre et les adresses IP, vous pouvez vous connecter à des réseaux virtuels. 

## <a name="system-center"></a>System Center

Déployer et gérer l’infrastructure SDN avec [gestion de Machine virtuelle (VMM)](https://docs.microsoft.com/system-center/vmm/) et [Operations Manager](https://docs.microsoft.com/system-center/scom/). Avec VMM, vous approvisionnez et gérez les ressources nécessaires pour créer et déployer des machines virtuelles et services dans des clouds privés.  Avec Operations Manager, vous surveillez les services, les appareils et les opérations au sein de votre entreprise pour identifier les problèmes pour une action immédiate. 


---