---
title: Technologies SDN
description: Les rubriques de cette section fournissent la vue d’ensemble et des informations techniques sur les technologies de mise en réseau de logiciels définies qui sont inclus dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.service: virtual-network
ms.technology: networking-sdn
ms.topic: article
ms.assetid: b491089c-5bcb-49d4-95b1-915b7ce69f88
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f842ac0d1a09106c1898374cf8dd1c7823d7dae
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="sdn-technologies"></a>Technologies SDN

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Les rubriques de cette section fournissent la vue d’ensemble et des informations techniques sur les technologies de mise en réseau de logiciels définies qui sont inclus dans Windows Server2016.  
  
> [!NOTE]  
> Pour plus de documentation logiciel défini de mise en réseau, vous pouvez utiliser les sections suivantes de la bibliothèque.  
>   
> - [Planifier SDN](../plan/Plan-Software-Defined-Networking.md)
> - [Déployer SDN](../deploy/Deploy-Software-Defined-Networking.md)
> - [Gérer SDN](../manage/manage-sdn.md)
> - [Sécurité pour SDN](../security/sdn-security-top.md)
> - [Résoudre les problèmes SDN](../troubleshoot/Troubleshoot-Software-Defined-Networking.md)

Il existe de nombreuses technologies qui fonctionnent ensemble pour créer des solutions logicielles défini de mise en réseau (SDN) de Microsoft, notamment les suivantes:  
  
-   **[Border Gateway Protocol et #40; protocole BGP et #41;](../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)**  
  
    Lorsque configurés sur une passerelle Windows Server2016 Remote Access Service (RAS), protocole BGP (Border Gateway) vous offre la possibilité de gérer le routage du trafic réseau entre les réseaux d’ordinateurs virtuels de vos clients et leurs sites distants. Protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il est un protocole de routage dynamique et découvre automatiquement les itinéraires entre les sites qui sont connectés à l’aide de connexions VPN site à site.  
  
-   **[Vue d’ensemble du pare-feu de centre de données](../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)**  
  
    Pare-feu de centre de données est un nouveau service inclus avec Windows Server2016. Il est une couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et destination) avec état et multi-locataire. Lorsque déployé et proposées en tant que service par le fournisseur de services, les administrateurs clients peuvent installer et configurer des stratégies de pare-feu pour protéger leurs réseaux virtuels du trafic indésirable provenant d’Internet et les réseaux intranet.  
  
  
-   **[Virtualisation de réseau Hyper-V](../../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)**  
  
    Virtualisation de réseau Hyper-V (HNV) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée.  
  
- **[Interne du Service DNS et #40; noms de domaines internationaux et #41; pour SDN](../../sdn/technologies/Idns-for-Sdn.md)**

    Ordinateurs virtuels hébergés \(VMs\) et applications requièrent DNS de communiquer au sein de leurs propres réseaux et avec les ressources externes sur Internet. Avec les noms de domaines internationaux, vous pouvez fournir des clients avec les services de résolution de noms DNS pour leur espace de noms isolé, local et pour les ressources Internet.

-   **[Contrôleur de réseau](../../sdn/technologies/network-controller/Network-Controller.md)**  
  
    Le contrôleur de réseau offre une centralisée programmable de l’automatisation pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.  
  
-   **[Virtualisation de fonction réseau](../../sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)**  
  
    Fonctions réseau effectuées par les appliances matérielles (équilibreurs de charge, pare-feu, routeurs, commutateurs, etc.) sont en cours virtualisées plus en plus comme des équipements virtuels.  
  
    Microsoft a virtualisés réseaux, les commutateurs, les passerelles, NAT, équilibreurs de charge et pare-feu.  

-   **[Passerelle pour SDN](../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)**
  
    Cette passerelle est un partagé, basée sur les logiciels, protocole BGP (Border Gateway) compatible avec routeur dans Windows Server2016 est conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels clients à l’aide de la virtualisation de réseau Hyper-V.  
      
- **[Accès Direct à la mémoire à distance et #40; rDMA et #41; et Switch Embedded Teaming et #40; sET et #41;](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)**  
  
    Vous pouvez utiliser une carte réseau convergence pour combiner RDMA et Ethernet le trafic à l’aide d’une seule carte réseau. La carte réseau convergence permet d’utiliser une seule carte réseau pour la gestion, stockage accès à distance Direct mémoire RDMA activé et le trafic de client. Cela réduit les dépenses d’investissement qui sont associés à chaque serveur dans votre centre de données, car vous avez besoin de moins de cartes réseau pour gérer les différents types de trafic par serveur.  
  
    JEU est une solution d’association de cartes réseau qui est intégrée dans le commutateur virtuel Hyper-V. ENSEMBLE permet l’association de cartes réseau physiques jusqu'à huit dans une seule association ensemble, ce qui améliore la disponibilité et fournit un basculement. Dans Windows Server2016, vous pouvez créer des équipes de jeu sont limités à l’utilisation du bloc de Message serveur (SMB) et RDMA.
  

-   **[L’équilibrage de charge logicielle & #40; SLB & #41; pour SDN](../../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)**  

    Fournisseurs de services cloud (CSP) et les entreprises qui déploient des logiciels défini de mise en réseau (SDN) dans Windows Server2016 peuvent utiliser l’équilibrage de charge logiciels (SLB) pour répartir les clients et le trafic réseau de client client entre les ressources du réseau virtuel. L’équilibrage du serveur Windows permet à plusieurs serveurs pour héberger la même charge de travail, fournissant une évolutivité et haute disponibilité.
  
-   **[SystemCenter](../../sdn/Sc-Tech-for-Sdn.md)** vous pouvez utiliser SystemCenter2016 Virtual Machine Manager (VMM) et Operations Manager pour déployer et gérer l’infrastructure SDN, notamment les contrôleurs de réseau, les équilibrages de charge logicielle et les passerelles. Vous pouvez également utiliser VMM de manière centralisée définir et contrôler les stratégies de réseau virtuel, lier les stratégies à vos applications ou les charges de travail.
  
- **[Conteneurs Windows](../technologies/Containers/Container-networking-overview.md)**
    
    Conteneurs Windows Server constituent une méthode de virtualisation de système d’exploitation léger utilisée pour séparer les services ou applications à partir d’autres services qui s’exécutent sur le même hôte de conteneur. Pour ce faire, chaque conteneur possède sa propre vue du système d’exploitation, processus, système de fichiers, Registre et des adresses IP. Avec Windows Server2016, vous pouvez désormais connecter des conteneurs Windows Server vers des réseaux virtuels. Fonction de conteneurs Windows de la même façon pour les ordinateurs virtuels dans le plan de mise en réseau. Chaque conteneur possède une carte réseau virtuelle qui est connectée à un commutateur virtuel sur lequel le trafic entrant et sortant est transféré. Pour appliquer l’isolation entre les conteneurs sur le même ordinateur hôte, un compartiment réseau est créé pour chaque conteneur Hyper-V dans lequel la carte réseau pour le conteneur est installée et de Windows Server. Conteneurs Windows Server utilisent une carte réseau virtuelle hôte à associer au commutateur virtuel. Conteneurs Hyper-V permet d’associer au commutateur virtuel une carte réseau VM synthétique (ne pas exposée à l’ordinateur virtuel utilitaire).

