---
title: Nouveautés de la mise en réseau
description: Cette rubrique fournit des informations générales sur les nouvelles fonctionnalités et technologies pour la mise en réseau dans Windows Server 2016
ms.prod: windows-server
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
author: dcuomo
ms.author: dacuo
ms.openlocfilehash: 0a3d7bb4f8521493743024408ae0f1f35a4b251c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855792"
---
# <a name="whats-new-in-networking"></a>Nouveautés de la mise en réseau

>S’applique à : Windows Server 2016

Voici les technologies de mise en réseau nouvelles ou améliorées de Windows Server 2016.  
  UPD cette rubrique contient les sections suivantes.  
  
-   [Nouvelles technologies et fonctionnalités de mise en réseau](#bkmk_features)  
  
-   [Nouvelles fonctionnalités pour les technologies de mise en réseau supplémentaires](#bkmk_existing)  
  
## <a name="new-networking-features-and-technologies"></a><a name="bkmk_features"></a>Nouvelles technologies et fonctionnalités de mise en réseau

La mise en réseau est une partie fondamentale de la plateforme SDDC (Software Defined Datacenter), et Windows Server 2016 fournit des technologies SDN (Software Defined Networking) nouvelles et améliorées pour vous aider à migrer vers une solution SDDC entièrement réalisée pour votre organisation.  
  
Lorsque vous gérez des réseaux en tant que ressource définie par un logiciel, vous pouvez décrire une fois les exigences de l’infrastructure d’une application, puis choisir l’emplacement d’exécution de l’application (localement ou dans le Cloud).  Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle et que vous pouvez exécuter en toute transparence des applications, où que vous soyez, avec une confiance égale pour la sécurité, les performances, la qualité de service et la disponibilité.  
  
Les sections suivantes contiennent des informations sur ces nouvelles technologies et fonctionnalités de mise en réseau.  
  
### <a name="software-defined-networking-infrastructure"></a>Infrastructure de mise en réseau définie par logiciel

Voici les technologies d’infrastructure SDN nouvelles ou améliorées.  
  
-   **Contrôleur de réseau**. Nouveauté de Windows Server 2016, le contrôleur de réseau fournit un point d’automatisation centralisé et programmable pour gérer, configurer, surveiller et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de donnes. À l'aide du contrôleur de réseau, vous pouvez automatiser la configuration de l'infrastructure réseau au lieu d'effectuer une configuration manuelle des services et appareils réseau. Pour plus d’informations, consultez [contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md) et déployer des [réseaux définis par logiciel à l’aide de scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Commutateur virtuel Hyper-V**. Le commutateur virtuel Hyper-V s’exécute sur les ordinateurs hôtes Hyper-V et vous permet de créer une commutation et un routage distribués, ainsi qu’une couche d’application de stratégie qui est alignée et compatible avec Microsoft Azure. Pour plus d’informations, voir [Commutateur virtuel Hyper\-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Virtualisation de fonction réseau (NFV)** . Dans les centres de développement de logiciels actuels, les fonctions réseau qui sont effectuées par les appliances matérielles (par exemple, les équilibreurs de charge, les pare-feu, les routeurs, les commutateurs, etc.) sont de plus en plus déployées en tant qu’Appliances virtuelles. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux. Les appliances virtuelles sont rapidement émergentes et créent un nouveau marché. Ils continuent à générer un intérêt et à gagner de l’énergie dans les plateformes de virtualisation et les services Cloud. Les technologies NFV suivantes sont disponibles dans Windows Server 2016.  
  
    -   **Pare-feu de centre de centres**. Ce pare-feu distribué fournit des listes de contrôle d’accès (ACL) granulaires, ce qui vous permet d’appliquer des stratégies de pare-feu au niveau de l’interface de la machine virtuelle ou au niveau du sous-réseau.  
  
        Pour plus d’informations, consultez [vue d’ensemble du pare-feu de centre](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)de données.  
  
    -   **Passerelle RAS**. Vous pouvez utiliser la passerelle RAS pour acheminer le trafic entre les réseaux virtuels et les réseaux physiques, y compris les connexions VPN de site à site entre votre centre de distribution Cloud et les sites distants de vos locataires. En particulier, vous pouvez déployer des passerelles VPN de site à site (VPN) de site à site (VPN) de protocole IKE (Internet Key Exchange) version 2 (IKEv2), de couche 3 (L3) et d’encapsulation générique de routage (GRE). En outre, les pools de passerelle et la redondance M + N des passerelles sont désormais pris en charge ; et Border Gateway Protocol (BGP) avec des fonctionnalités de réflecteur de routage fournit un routage dynamique entre les réseaux pour tous les scénarios de passerelle (VPN IKEv2, VPN GRE et VPN L3).  
  
        Pour plus d’informations, consultez [Nouveautés de la passerelle RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [de la passerelle RAS pour SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Les load balancer logicielles (SLB) et la traduction d’adresses réseau (NAT)** . L’équilibreur de charge Nord-Sud et est de la couche 4 de l’ouest des États-parleurs et NAT améliorent le débit en prenant en charge le retour au serveur direct, avec lequel le trafic réseau de retour peut contourner le multiplexeur d’équilibrage de charge.  
       Pour plus d’informations, consultez [équilibrage &#40;de charge&#41; logiciel SLB pour SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Pour plus d’informations, consultez [Network Function Virtualization](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Protocoles standardisés**. Le contrôleur de réseau utilise REST sur son interface Northbound avec des charges utiles JavaScript Object Notation (JSON). L’interface Southbound du contrôleur de réseau utilise le protocole OVSDB (Open vSwitch Database Management Protocol).  
  
-   **Technologies d’encapsulation flexibles**. Ces technologies fonctionnent au niveau du plan de données et prennent en charge à la fois Virtual extensible LAN (VxLAN) et la virtualisation de réseau l’encapsulation générique de routage (NVGRE). Pour plus d’informations, voir [tunneling GRE dans Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Pour plus d’informations sur SDN, consultez [Software Defined &#40;Networking SDN&#41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Notions de base de la mise à l’échelle du Cloud
 
Les principes de base de l’échelle du Cloud suivants sont désormais disponibles.  
  
-   **Carte d’interface réseau (NIC) convergée**. La carte réseau convergée vous permet d’utiliser une seule carte réseau pour la gestion, le stockage avec accès direct à la mémoire à distance (RDMA) et le trafic de locataire. Cela réduit les dépenses en capital associées à chaque serveur dans votre centre de centres, car vous avez besoin de moins de cartes réseau pour gérer différents types de trafic par serveur.  
  
-   **Paquet direct**.  Packet direct offre un débit de trafic réseau élevé et une infrastructure de traitement de paquets à faible latence.  
  
-   **Switch Embedded Teaming (Set)** .        SET est une solution d’association de cartes réseau intégrée au commutateur virtuel Hyper-V. L’ensemble permet d’associer jusqu’à huit cartes réseau physiques à une seule équipe, ce qui améliore la disponibilité et assure le basculement. Dans Windows Server 2016, vous pouvez créer des équipes qui sont limitées à l’utilisation du protocole SMB (Server Message Block) et RDMA. En outre, vous pouvez utiliser définir des équipes pour distribuer le trafic réseau pour la virtualisation de réseau Hyper-V. Pour plus d’informations, [ &#40;consultez accès direct à la&#41; mémoire à distance RDMA et &#40;Switch&#41;Embedded Teaming Set](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="new-features-for-additional-networking-technologies"></a><a name="bkmk_existing"></a>Nouvelles fonctionnalités pour les technologies de mise en réseau supplémentaires

Cette section contient des informations sur les nouvelles fonctionnalités des technologies de mise en réseau familières.
  
## <a name="dhcp"></a><a name="bkmk_dhcp"></a>DHCP  
Le protocole DHCP est une norme IETF (Internet Engineering Task Force) conçue pour réduire le travail d’administration et la complexité de configuration des hôtes sur un réseau TCP/IP, tel qu’un intranet privé. Le recours au service Serveur DHCP permet d’automatiser le processus de configuration de TCP/IP sur les clients DHCP.  
  
Pour plus d’informations, consultez [Nouveautés dans DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="dns"></a><a name="bkmk_dns"></a>DN  
Le service DNS permet de nommer les ordinateurs et les services de réseau dans les réseaux TCP/IP. L’attribution de noms DNS permet de rechercher les ordinateurs et les services au moyen de noms conviviaux. Lorsqu’un utilisateur entre un nom DNS dans une application, les services DNS peuvent résoudre ce nom en une autre information qui lui est associée, par exemple une adresse IP.  
  
Vous trouverez ci-dessous des informations sur le client DNS et le serveur DNS.  
  
### <a name="dns-client"></a><a name="bkmk_dnsc"></a>Client DNS  
Voici les nouvelles technologies clientes DNS ou améliorées.  
  
-   **Liaison de service du client DNS**. Dans Windows 10, le service client DNS offre une prise en charge améliorée pour les ordinateurs dotés de plusieurs interfaces réseau.  
  
Pour plus d’informations, voir [Nouveautés du client DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="dns-server"></a><a name="bkmk_dnss"></a>Serveur DNS  
Voici les technologies de serveur DNS nouvelles ou améliorées.  
  
-   **Stratégies DNS**.  Vous pouvez configurer des stratégies DNS pour spécifier la façon dont un serveur DNS répond aux requêtes DNS. Les réponses DNS peuvent être basées sur l’adresse IP du client (emplacement), l’heure de la journée et plusieurs autres paramètres. Les stratégies DNS activent les DNS sensibles à l’emplacement, la gestion du trafic, l’équilibrage de charge, le DNS de fractionnement et d’autres scénarios.  
  
-   **Prise en charge de nano Server pour les DNS basés sur des fichiers**, vous pouvez déployer le serveur DNS dans Windows Server 2016 sur une image nano Server. Cette option de déploiement est disponible si vous utilisez un serveur DNS basé sur des fichiers. En exécutant le serveur DNS sur une image nano Server, vous pouvez exécuter vos serveurs DNS avec un encombrement réduit, un démarrage rapide et une mise à jour corrective réduite.  
  
    > [!NOTE]   
    > Active Directory DNS intégré n’est pas pris en charge sur nano Server.  
  
-   **Limitation du taux de réponse (RRL)** .  Vous pouvez activer la limitation du taux de réponse sur vos serveurs DNS. En procédant ainsi, vous évitez la possibilité que des systèmes malveillants utilisent vos serveurs DNS pour lancer une attaque par déni de service sur un client DNS.  
  
-   **Authentification DNS des entités nommées**.   Vous pouvez utiliser des enregistrements TLSA (Transport Layer Security Authentication) pour fournir des informations aux clients DNS qui indiquent l’autorité de certification à partir de laquelle ils doivent s’attendre à recevoir un certificat pour votre nom de domaine. Cela empêche les attaques de l’intercepteur, où un utilisateur peut corrompre le cache DNS pour pointer vers son propre site Web et fournir un certificat qu’il a émis à partir d’une autre autorité de certification.  
  
-   **Prise en charge des enregistrements inconnus**.   
     Vous pouvez ajouter des enregistrements qui ne sont pas explicitement pris en charge par le serveur DNS Windows à l’aide de la fonctionnalité d’enregistrement inconnu.  
  
-   **Indications de racine IPv6**.   
     Vous pouvez utiliser la prise en charge des indications de racine IPV6 natives pour effectuer une résolution de noms Internet à l’aide des serveurs racine IPV6.  
  
-   **Prise en charge améliorée de Windows PowerShell**.   
      De nouvelles applets de commande Windows PowerShell sont disponibles pour le serveur DNS.  
  
Pour plus d’informations, voir [Nouveautés du serveur DNS dans Windows server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="gre-tunneling"></a><a name="bkmk_GRE"></a>Tunneling GRE  
La passerelle RAS prend désormais en charge les tunnels GRE (Generic Routing Encapsulation) à haute disponibilité pour les connexions de site à site et la redondance M + N des passerelles. GRE est un protocole de tunneling léger qui peut encapsuler une grande variété de protocoles de la couche réseau dans les liaisons point à point virtuelles sur un réseau d’interconnexion IP.  
  
Pour plus d’informations, voir [tunneling GRE dans Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="hyper-v-network-virtualization"></a><a name="HNV"></a>Virtualisation de réseau Hyper-V  
Introduite dans Windows Server 2012, la virtualisation de réseau Hyper-V (HNV) permet la virtualisation de réseaux clients sur une infrastructure de réseau physique partagée. Avec des modifications minimales nécessaires sur l’infrastructure réseau physique, HNV offre aux fournisseurs de services l’agilité de déployer et de migrer les charges de travail des locataires n’importe où sur les trois Clouds : le Cloud du fournisseur de services, le cloud privé ou le Microsoft Azure cloud public.  
  
Pour plus d’informations, consultez [Nouveautés de la virtualisation de réseau Hyper-V dans Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="ipam"></a><a name="bkmk_ipam"></a>IP  
IPAM offre des fonctionnalités d’administration et d’analyse hautement personnalisables pour l’adresse IP et l’infrastructure DNS sur un réseau d’entreprise. IPAM vous permet de surveiller, d’auditer et de gérer les serveurs qui exécutent le protocole DHCP (Dynamic Host Configuration Protocol) et le système DNS (Domain Name System).  
  
-   **Gestion améliorée des adresses IP**.  
     Les fonctionnalités IPAM sont améliorées pour des scénarios tels que la gestion des sous-réseaux IPv4/32 et IPv6/128, ainsi que la recherche de sous-réseaux et de plages d’adresses IP libres dans un bloc d’adresses IP.  
  
-   **Gestion améliorée du service DNS**.  
     IPAM prend en charge les enregistrements de ressources DNS, les redirecteurs conditionnels et la gestion des zones DNS pour les serveurs DNS intégrés au domaine Active Directory et sauvegardés dans un fichier.  
  
-   **Gestion intégrée des serveurs DNS, DHCP et d’adresses IP (DDI)** .  
     Plusieurs nouvelles expériences et opérations de gestion du cycle de vie intégrées sont activées, telles que la visualisation de tous les enregistrements de ressources DNS qui se rapportent à une adresse IP, l’inventaire automatisé des adresses IP basées sur les enregistrements de ressources DNS et la gestion du cycle de vie des adresses IP. pour les opérations DNS et DHCP.  
  
-   **Prise en charge de plusieurs forêts Active Directory**.  
     Vous pouvez utiliser IPAM pour gérer les serveurs DNS et DHCP de plusieurs forêts Active Directory lorsqu’il existe une relation d’approbation bidirectionnelle entre la forêt où IPAM est installé et chacune des forêts distantes.  
  
-   **Prise en charge de Windows PowerShell pour les Access Control basées sur les rôles**.  
     Vous pouvez utiliser Windows PowerShell pour définir des étendues d’accès sur des objets IPAM.  
  
Pour plus d’informations, voir [Nouveautés d’IPAM](technologies/ipam/What-s-New-in-IPAM.md) et [gestion des adresses IP](technologies/ipam/Manage-IPAM.md).  
  

