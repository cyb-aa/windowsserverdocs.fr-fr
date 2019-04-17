---
title: Logiciel défini de mise en réseau (SDN)
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les technologies de mise en réseau défini par logiciel (SDN) qui sont fournis dans Windows Server, SystemCenter et MicrosoftAzure.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33351ccfa1466f667ef9351768c89b373734075d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="software-defined-networking-sdn"></a>Logiciel défini de mise en réseau (SDN)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les technologies de mise en réseau défini par logiciel (SDN) qui sont fournis dans WindowsServerDatacenter edition, SystemCenter2016 et MicrosoftAzure.  
  
> [!NOTE]
> Outre cette rubrique, le contenu SDN suivant est disponible.
> 
> - [Quelles sont les nouveautés dans SDN pour Windows Server](sdn-whats-new.md)
> - [Introduction aux SDN dans WindowsServerDatacenter](sdn-intro.md)
> - [Technologies SDN](technologies/Software-Defined-Networking-Technologies.md)  
> - [Planifier SDN](plan/Plan-Software-Defined-Networking.md) 
> - [Déployer SDN](deploy/Deploy-Software-Defined-Networking.md)  
> - [Gérer SDN](manage/manage-sdn.md)
> - [Sécurité pour SDN](security/sdn-security-top.md)
> - [Résoudre les problèmes SDN](troubleshoot/Troubleshoot-Software-Defined-Networking.md)
> - [Technologies SystemCenter pour SDN](Sc-Tech-for-Sdn.md)
> - [MicrosoftAzure et SDN](Azure_and_Sdn.md)
> - [Contactez le centre de données et l’équipe réseau de Cloud](contact-sdn-team.md)
  
## <a name="bkmk_sdn"></a>Vue d’ensemble de la mise en réseau Sdn Server

Logiciel défini de mise en réseau \(SDN\) fournit une méthode pour configurer et gérer les périphériques réseau physiques et virtuels tels que les routeurs, les commutateurs et les passerelles dans votre centre de données de manière centralisée. Éléments de réseau virtuel tels que le commutateur virtuel Hyper-V, la virtualisation de réseau Hyper-V et passerelle sont conçus pour faire partie intégrante de votre infrastructure SDN.

>[!NOTE]
>Pour les ordinateurs hôtes Hyper-V et \(VMs\) des ordinateurs virtuels qui exécutent des serveurs d’infrastructure SDN, tels que le contrôleur de réseau et l’équilibrage de charge logicielle des nœuds, vous devez installer Windows Server 2016 Datacenter edition. Pour les ordinateurs hôtes Hyper-V qui contiennent uniquement locataire la charge de travail ordinateurs virtuels qui sont connectés aux réseaux contrôlés par SDN\, vous pouvez exécuter Windows Server 2016, Standard edition.

Vous pouvez toujours utiliser vos commutateurs physiques existants, routeurs et autres périphériques matériels, vous pouvez obtenir une intégration plus étroite entre le réseau virtuel et le réseau physique si ces périphériques sont conçus pour la compatibilité avec Sdn.  
  
SDN est possible, car les plans de réseau - les plans de gestion, le contrôle et données - ne sont plus liés aux périphériques réseau eux-mêmes, mais sont abstraits pour une utilisation par d’autres entités, tels que les logiciels de gestion de centre de données tels que SystemCenter2016.  
  
SDN vous permet de gérer votre réseau de centre de données pour fournir une méthode automatisée et centralisée pour répondre aux besoins de vos applications et les charges de travail de façon dynamique. Sdn offre les fonctionnalités suivantes.  
  
-   Possibilité d’abstraire vos applications et les charges de travail à partir du réseau physique sous-jacent, grâce à la virtualisation du réseau. Comme avec la virtualisation de serveur à l’aide d’Hyper-V, les abstractions sont cohérentes et fonctionnent avec vos applications et les charges de travail de manière sans interruption de service. Par exemple, Sdn fournit des abstractions virtuelles pour les éléments de votre réseau physique, tels que les adresses IP, les commutateurs et les équilibreurs de charge.  
  
-   Possibilité de définir de manière centralisée les stratégies de contrôle qui régissent les réseaux physiques et virtuels, y compris les flux de trafic entre ces deux types de réseau.  
  
-   La possibilité d’implémenter des stratégies de réseau de façon cohérente à grande échelle, même lorsque vous déployez de nouvelles charges de travail ou déplacez les charges de travail sur des réseaux virtuels ou physiques.  
  
## <a name="bkmk_ws"></a>Technologies Windows Server pour les logiciels défini par la mise en réseau  
Windows Server inclut les technologies de mise en réseau suivantes logiciels définis.  

### <a name="bkmk_nc"></a>Contrôleur de réseau

Nouveau dans Windows Server2016, le contrôleur de réseau offre une centralisée programmable d’automation pour gérer, configurer, surveiller et résoudre les problèmes virtuelle et d’infrastructure réseau physique dans votre centre de données. À l’aide du contrôleur de réseau, vous pouvez automatiser la configuration de l’infrastructure réseau au lieu d’effectuer une configuration manuelle des périphériques réseau et services.  
  
Contrôleur de réseau est un rôle serveur hautement disponible et évolutif et fournit une application programming interface (API) - l’API Southbound - qui permet à un contrôleur de réseau communiquer avec le réseau et une deuxième API - l’API Northbound - qui vous permet de communiquer avec le contrôleur de réseau.  
  
À l’aide de Windows PowerShell, l’API REST Representational State Transfer () ou une application de gestion, vous pouvez utiliser le contrôleur de réseau pour gérer l’infrastructure réseau physique et virtuelle suivante.  
  
-   Ordinateurs virtuels Hyper-V et commutateurs virtuels  
  
-   Commutateurs de réseau physique  
  
-   Routeurs de réseau physique  
  
-   Logiciel pare-feu  
  
-   Passerelles VPN, y compris les passerelles mutualisées de Service d’accès à distance (RAS)  
  
-   Équilibreurs de charge  
  
Pour plus d’informations, voir [contrôleur de réseau](../sdn/technologies/network-controller/Network-Controller.md).  
  
### <a name="bkmk_hv"></a>Virtualisation de réseau Hyper-V

La virtualisation de réseau Hyper-V vous aide à abstraire vos applications et les charges de travail à partir du réseau physique à l’aide de réseaux virtuels. Les réseaux virtuels fournissent l’isolation mutualisée nécessaire pendant l’exécution d’une infrastructure réseau physique partagée, ce qui augmente l’utilisation des ressources. Pour vous assurer que vous pouvez reporter vos investissements existants, vous pouvez configurer des réseaux virtuels sur le matériel réseau existant. En outre, les réseaux virtuels sont compatibles avec les réseaux locaux virtuels (VLAN).  
  
Pour plus d’informations, voir [la virtualisation de réseau Hyper-V](../sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md).  
  
### <a name="bkmk_switch"></a>Commutateur virtuel Hyper-V

Le commutateur virtuel Hyper-V est un logiciel Ethernet réseau commutateur de couche 2 qui est disponible dans le Gestionnaire Hyper-V après avoir installé le rôle serveur Hyper-V. Le commutateur inclut des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles à des réseaux virtuels et le réseau physique. En outre, le commutateur virtuel Hyper-V fournit des stratégies de sécurité, d’isolation et de niveaux de service.  
  
Dans le commutateur virtuel Hyper-V dans Windows Server2016, vous pouvez également déployer le commutateur SET (Embedded Teaming) et l’accès à la mémoire Direct à distance (RDMA). Pour plus d’informations, consultez la section [accès à la mémoire Direct à distance (RDMA) et le commutateur SET (Embedded Teaming)](#bkmk_rdma) dans cette rubrique.  
  
Pour plus d’informations sur le commutateur virtuel Hyper-V, voir [commutateur virtuel Hyper-V](../../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="bkmk_idns"></a>Interne du Service DNS et #40; noms de domaines internationaux et #41;

Ordinateurs virtuels hébergés \(VMs\) et applications requièrent DNS de communiquer au sein de leurs propres réseaux et avec les ressources externes sur Internet. Avec les noms de domaines internationaux, vous pouvez fournir des clients avec les services de résolution de noms DNS pour leur espace de noms isolé, local et pour les ressources Internet.

Pour plus d’informations, voir [Service DNS interne et #40; noms de domaines internationaux et #41; pour SDN](technologies/Idns-for-Sdn.md).
  
### <a name="bkmk_nfv"></a>Virtualisation de fonction réseau

Dans le logiciel d’aujourd'hui centres de données définies, les fonctions réseau effectuées par les appliances matérielles (équilibreurs de charge, pare-feu, routeurs, commutateurs, etc.) sont plus en plus en cours virtualisés comme des équipements virtuels. Cette «virtualisation de fonction réseau» est une évolution naturelle de la virtualisation de serveur et de la virtualisation de réseau. Équipements virtuels sont rapidement émergents et création d’un tout nouveau marché. Ils continuent à susciter l’intérêt et de gagner du terrain dans les deux plateformes de virtualisation et de services cloud.  
  
Les technologies de virtualisation de fonction réseau suivants sont disponibles.  
  
-   **Équilibreur de charge logicielle (SLB) et adresses réseau Translation (NAT)**. Le nord sud et est-ouest couche 4 équilibrage de charge et améliore le débit en prenant en charge directe serveur retourner, à laquelle le trafic réseau de retour peut contourner l’équilibrage de charge multiplexeur NAT. Pour plus d’informations, voir [l’équilibrage de charge logiciels (SLB) pour SDN](technologies/network-function-virtualization/software-load-balancing-for-sdn.md).  
  
-   **Pare-feu de centre de données**. Ce pare-feu distribué fournit des listes de contrôle granulaire de l’accès (ACL), ce qui vous permet d’appliquer des stratégies de pare-feu au niveau de l’interface de machine virtuelle ou au niveau du sous-réseau.  
  
    Pour plus d’informations, voir [vue d’ensemble du pare-feu de centre de données](../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
-   **Cette passerelle**. Vous pouvez utiliser des passerelles pour le pontage du trafic entre les réseaux virtuels et des réseaux non virtualisé; plus précisément, vous pouvez déployer des passerelles VPN de site à site, les passerelles de transfert et Encapsulation GRE (Generic Routing). En outre, M + N la redondance de passerelles est pris en charge. Pour plus d’informations, voir [passerelle pour SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).
  
Pour plus d’informations, voir [la virtualisation de fonction réseau](technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_rdma"></a>Accès Direct à la mémoire à distance (RDMA) et le commutateur incorporé association (ensemble)  
Dans Windows Server2016, vous pouvez activer RDMA sur les cartes réseau qui sont liés à un commutateur virtuel Hyper-V avec ou sans commutateur SET (Embedded Teaming). Cela vous permet d’utiliser moins de cartes réseau lorsque vous souhaitez utiliser RDMA et SET en même temps.  
  
JEU est une solution alternative association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile logicielle définie de mise en réseau (SDN) dans Windows Server2016. JEU de certaines des fonctionnalités d’association de cartes réseau s’intègre dans le commutateur virtuel Hyper-V.  
  
JEU vous permet de regrouper entre une et huit cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuel basé sur les logiciels. Ces cartes réseau virtuelles fournissent des performances élevées et la tolérance de panne en cas de défaillance de la carte réseau.  
Toutes les cartes réseau de jeu doivent être installés dans le même hôte physique Hyper-V doivent être placés dans une équipe.  
  
En outre, vous pouvez utiliser les commandes Windows PowerShell pour activer Data Center Bridging (DCB), créez un commutateur virtuel Hyper-V avec une carte réseau virtuelle de RDMA (vNIC) et créer un commutateur virtuel Hyper-V avec des cartes VNIC RDMA et SET.  
  
Pour plus d’informations, voir [accès à la mémoire Direct à distance (RDMA) et le commutateur SET (Embedded Teaming)](https://technet.microsoft.com/library/mt403349.aspx).  
  
### <a name="bkmk_rras"></a>Passerelle pour SDN  
Cette passerelle est un partagé, basée sur les logiciels, protocole BGP (Border Gateway) compatible avec routeur dans Windows Server2016 est conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels clients à l’aide de la virtualisation de réseau Hyper-V.  
  
Cette passerelle fournit des pools de passerelle, la redondance M + N, plusieurs types de connexions VPN site à site et BGP réflecteur d’itinéraire pour vous fournir des choix de conception flexible pour votre infrastructure de passerelle.  
  
Pour plus d’informations, voir [passerelle pour SDN](technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
  
### <a name="bkmk_slb"></a>(SLB) d’équilibrage de charge logicielle  
Fournisseurs de services cloud (CSP) et les entreprises qui déploient des logiciels défini de mise en réseau (SDN) dans Windows Server2016 peuvent utiliser l’équilibrage de charge logiciels (SLB) pour répartir les clients et le trafic réseau de client client entre les ressources du réseau virtuel. L’équilibrage du serveur Windows permet à plusieurs serveurs pour héberger la même charge de travail, fournissant une évolutivité et haute disponibilité.  
  
Pour plus d’informations, voir [l’équilibrage de charge logicielle et #40; sLB et #41; pour SDN](../sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md).

### <a name="windows-server-containers"></a>Conteneurs Windows Server

Conteneurs Windows Server constituent une méthode de virtualisation de système d’exploitation léger utilisée pour séparer les services ou applications à partir d’autres services qui s’exécutent sur le même hôte de conteneur. Pour ce faire, chaque conteneur possède sa propre vue du système d’exploitation, processus, système de fichiers, Registre et des adresses IP. Avec Windows Server2016, vous pouvez désormais connecter des conteneurs Windows Server vers des réseaux virtuels. Pour plus d’informations, voir [conteneurs Windows Server](technologies/containers/Container-networking-overview.md).

### <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>Contactez l’équipe de produit de centre de données et de mise en réseau de Cloud

Si vous êtes intéressé par traitant des technologies SDN avec Microsoft ou d’autres clients SDN, il existe diverses méthodes permettant de contact.

Pour plus d’informations, voir [contacter l’équipe de mise en réseau de Cloud et le centre de données](contact-sdn-team.md).
