---
title: Nouveautés de la mise en réseau
description: Cette rubrique fournit des informations générales sur les nouvelles fonctionnalités et technologies de mise en réseau dans Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85b1a573e8ac724a2a9e22863f890db668423cad
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-networking"></a>Nouveautés de la mise en réseau

>S’applique à: Windows Server2016

Voici les technologies de mise en réseau nouvelles ou améliorées dans Windows Server 2016.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Nouvelles fonctionnalités de mise en réseau et Technologies](#bkmk_features)  
  
-   [Nouvelles fonctionnalités des Technologies de mise en réseau supplémentaires](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Nouvelles fonctionnalités de mise en réseau et Technologies

Mise en réseau est un fondamental de la plateforme de centre de données de défini par logiciel (SDDC) et Windows Server 2016 fournit des technologies de mise en réseau défini par logiciel (SDN) nouvelles et améliorées pour vous aider à adopter une solution SDDC entièrement réalisée pour votre organisation.  
  
Lorsque vous gérez les réseaux en tant que ressources à définition logicielle, vous pouvez décrire les exigences de l’infrastructure d’une application une fois, puis choisissez où l’application s’exécute - sur site ou dans le cloud.  Cette cohérence signifie que vos applications sont désormais plus faciles de montée en puissance parallèle et vous pouvez exécuter en toute transparence des applications, n’importe où, en sachant que la sécurité, les performances de la qualité de service et la disponibilité.  
  
Les sections suivantes contiennent des informations sur ces nouvelles réseau fonctionnalités et technologies.  
  
### <a name="software-defined-networking-infrastructure"></a>Logiciel défini par l’Infrastructure de mise en réseau

Voici les technologies d’infrastructure SDN nouvelles ou améliorées.  
  
-   **Contrôleur réseau**. Nouveau dans Windows Server 2016, le contrôleur de réseau fournit un point programmable et centralisé de l’automatisation pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données. À l’aide du contrôleur de réseau, vous pouvez automatiser la configuration de l’infrastructure réseau au lieu d’effectuer une configuration manuelle des périphériques réseau et services. Pour plus d’informations, voir [contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md) et [déployer des réseaux à définition logicielle à l’aide de scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Commutateur virtuel Hyper-V**. Le commutateur virtuel Hyper-V s’exécute sur les ordinateurs hôtes Hyper-V et vous permet de créer de commutation distribuée et le routage, et une couche de mise en œuvre de stratégie qui est alignée et compatibles avec Microsoft Azure. Pour plus d’informations, voir [commutateur virtuel Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **La virtualisation de fonction (NFV) du réseau**. Dans le logiciel d’aujourd'hui centres de données définies, les fonctions réseau effectuées par les appliances matérielles (équilibreurs de charge, pare-feu, routeurs, commutateurs, etc.) sont plus en plus déployés comme des équipements virtuels. Cette «virtualisation de fonction réseau» est une évolution naturelle de la virtualisation de serveur et de la virtualisation de réseau. Équipements virtuels sont rapidement émergents et création d’un tout nouveau marché. Ils continuent à susciter l’intérêt et de gagner du terrain dans les deux plateformes de virtualisation et de services cloud. Les technologies NFV suivantes sont disponibles dans Windows Server 2016.  
  
    -   **Pare-feu de centre de données**. Ce pare-feu distribué fournit des listes de contrôle granulaire de l’accès (ACL), ce qui vous permet d’appliquer des stratégies de pare-feu au niveau de l’interface de machine virtuelle ou au niveau du sous-réseau.  
  
        Pour plus d’informations, voir [vue d’ensemble du pare-feu de centre de données](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Cette passerelle**. Vous pouvez utiliser cette passerelle de routage du trafic entre les réseaux virtuels et des réseaux physiques, y compris les connexions VPN de site à site à partir de votre centre de données cloud à des sites distants de vos clients. Plus précisément, vous pouvez déployer Internet Key Exchange version 2 (IKEv2) de site réseaux privés virtuels (VPN), VPN de couche 3 (L3) et les passerelles Encapsulation GRE (Generic Routing). En outre, les pools de passerelle et la redondance de M + N des passerelles sont désormais prises en charge; et le protocole BGP (Border Gateway) avec les fonctionnalités de réflecteur d’itinéraire assure le routage dynamique entre les réseaux pour tous les scénarios de passerelle (VPN IKEv2, GRE VPN et VPN L3).  
  
        Pour plus d’informations, voir [Nouveautés dans cette passerelle](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [passerelle pour SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Équilibreur de charge logicielle (SLB) et adresses réseau Translation (NAT)**. Le nord sud et est-ouest couche 4 équilibrage de charge et améliore le débit en prenant en charge directe serveur retourner, à laquelle le trafic réseau de retour peut contourner l’équilibrage de charge multiplexeur NAT.  
       Pour plus d’informations, voir [l’équilibrage de charge logicielle & #40; SLB & #41; pour SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Pour plus d’informations, voir [la virtualisation de fonction réseau](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Normalisé protocoles**. Contrôleur de réseau utilise Representational State Transfer (REST) sur son interface northbound avec les charges utiles de JavaScript Objet Notation (JSON). L’interface southbound du contrôleur de réseau utilise vSwitch ouvrir protocole de gestion de base de données (OVSDB).  
  
-   **Technologies d’encapsulation flexible**. Ces technologies fonctionnent au niveau du plan de données et prennent en charge de réseau local virtuel Extensible (VxLAN) et réseau virtualisation générique Encapsulation routage (NVGRE). Pour plus d’informations, voir [Tunneling GRE dans Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Pour plus d’informations sur SDN, voir [logiciel défini de mise en réseau & #40; SDN & #41; ](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Notions fondamentales de montée en puissance parallèle de cloud
 
Les principes fondamentaux de montée en puissance parallèle de cloud suivantes sont désormais disponibles.  
  
-   **Carte d’Interface réseau (NIC) de convergé**. La carte réseau convergence permet d’utiliser une seule carte réseau pour la gestion, stockage accès à distance Direct mémoire RDMA activé et le trafic de client. Cela réduit les dépenses d’investissement qui sont associés à chaque serveur dans votre centre de données, car vous avez besoin de moins de cartes réseau pour gérer les différents types de trafic par serveur.  
  
-   **Paquet Direct**.  Paquet Direct offre un débit supérieur du trafic réseau élevé et l’infrastructure de traitement des paquets à faible latence.  
  
-   **Commutateur incorporé (jeu) association de cartes réseau**.        JEU est une solution d’association de cartes réseau qui est intégrée dans le commutateur virtuel Hyper-V. ENSEMBLE permet l’association de cartes réseau physiques jusqu'à huit dans une seule association ensemble, ce qui améliore la disponibilité et fournit un basculement. Dans Windows Server 2016, vous pouvez créer des équipes de jeu sont limités à l’utilisation du bloc de Message serveur (SMB) et RDMA. En outre, vous pouvez utiliser des équipes de jeu pour distribuer le trafic réseau pour la virtualisation de réseau Hyper-V. Pour plus d’informations, voir [Remote Direct Memory Access & #40; RDMA & #41; et Switch Embedded Teaming & #40; SET & #41; ](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Nouvelles fonctionnalités des Technologies de mise en réseau supplémentaires

Cette section contient des informations sur les nouvelles fonctionnalités des technologies de mise en réseau familiers.
  
## <a name="bkmk_dhcp"></a>DHCP  
DHCP est une norme Internet Engineering Task Force (IETF) qui est conçue pour réduire la charge administrative et la complexité de la configuration des hôtes sur un réseau basé sur IP, tel qu’un intranet privé. En utilisant le service serveur DHCP, le processus de configuration TCP/IP sur les clients DHCP est automatique.  
  
Pour plus d’informations, voir [Nouveautés dans DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
DNS est un système qui est utilisé dans les réseaux TCP/IP de nommer les ordinateurs et les services réseau. Attribution de noms DNS localise les ordinateurs et les services par le biais de noms conviviaux. Lorsqu’un utilisateur entre un nom DNS dans une application, les services DNS peuvent résoudre le nom en une autre information qui est associée au nom, par exemple, une adresse IP.  
  
Voici les informations sur le Client DNS et serveur DNS.  
  
### <a name="bkmk_dnsc"></a>Client DNS  
Voici les technologies de client DNS nouvelles ou améliorées.  
  
-   **Liaison du service Client DNS**. Dans Windows 10, le service Client DNS offre une prise en charge améliorée pour les ordinateurs avec plusieurs interfaces réseau.  
  
Pour plus d’informations, voir [Nouveautés dans le Client DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Serveur DNS  
Voici les technologies de serveur DNS nouvelles ou améliorées.  
  
-   **Stratégies DNS**.  Vous pouvez configurer des stratégies DNS pour spécifier comment un serveur DNS répond aux requêtes DNS. Les réponses DNS peuvent être basés sur l’adresse IP du client (emplacement), heure de la journée et plusieurs autres paramètres. Stratégies DNS activer DNS prenant en charge emplacement, la gestion du trafic, l’équilibrage de charge, DNS «split brain» et autres scénarios.  
  
-   **Prise en charge de nano Server de fichier en fonction de DNS**, vous pouvez déployer le serveur DNS dans Windows Server 2016 sur une image Nano Server. Cette option de déploiement est disponible si vous utilisez DNS basée sur les fichiers. Par le serveur DNS en cours d’exécution sur une image Nano Server, vous pouvez exécuter vos serveurs DNS avec un encombrement réduit de démarrage rapide et de mise à jour corrective réduite.  
  
    > [!NOTE]   
    > DNS intégré à Active Directory n’est pas pris en charge sur Nano Server.  
  
-   **Réponse du taux de limitation (RRL)**.  Vous pouvez activer la limitation de vitesse de réponse sur vos serveurs DNS. Ce faisant, vous évitez la possibilité de systèmes malveillants à l’aide de vos serveurs DNS pour lancer une attaque par déni de service sur un client DNS.  
  
-   **L’authentification basée sur DNS d’entités nommées (DANE)**.   Vous pouvez utiliser des enregistrements TLSA (Transport Layer Security authentification) pour fournir des informations sur les clients DNS que l’autorité de certification (CA) ils doivent s’attendre un certificat à partir de votre nom de domaine. Cela empêche les attaques de man-in-the-middle où quelqu'un peut endommager le cache DNS pour pointer vers son site Web acheté et fournir une attestation à partir d’une autre autorité de certification.  
  
-   **Prise en charge de l’enregistrement inconnu**.   
     Vous pouvez ajouter des enregistrements qui ne sont pas explicitement prises en charge par le serveur DNS Windows à l’aide de la fonctionnalité d’enregistrement inconnu.  
  
-   **Les indications de racine IPv6**.   
     Vous pouvez utiliser le protocole IPV6 natif prend en charge des indications de racine pour effectuer la résolution de noms internet à l’aide de serveurs racine IPV6.  
  
-   **Amélioration de la prise en charge de Windows PowerShell**.   
      Nouvelles applets de commande Windows PowerShell sont disponibles pour le serveur DNS.  
  
Pour plus d’informations, voir [Nouveautés dans le serveur DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>Tunneling GRE  
Cette passerelle prend désormais en charge les tunnels GRE Generic Routing Encapsulation () de haute disponibilité pour les connexions de site à site et la redondance de passerelles M + N. GRE est un protocole de tunneling léger qui peut encapsuler une grande variété de protocoles de la couche réseau à l’intérieur des liaisons point à point virtuelles sur un réseau d’interconnexion IP.  
  
Pour plus d’informations, voir [Tunneling GRE dans Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualisation de réseau Hyper-V  
Introduite dans Windows Server 2012, la virtualisation de réseau Hyper-V (HNV) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée. Avec les modifications minimales nécessaires sur l’infrastructure réseau physique, HNV donne les fournisseurs de services la souplesse pour déployer et migrer les charges de travail clientes n’importe où dans les trois clouds: le cloud de fournisseur de service, le cloud privé ou dans le cloud public Microsoft Azure.  
  
Pour plus d’informations, voir [nouvelles fonctionnalités de virtualisation de réseau Hyper-V dans Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM fournit des fonctionnalités d’administration et d’analyse hautement personnalisables pour l’adresse IP et l’infrastructure DNS sur un réseau d’entreprise. L’utilisation d’IPAM, vous pouvez analyser, auditer et gérer les serveurs qui exécutent DHCP Dynamic Host Configuration Protocol () et le système DNS (Domain Name).  
  
-   **Gestion des adresses IP améliorée**.  
     Fonctionnalités d’IPAM sont améliorées pour les scénarios tels que la gestion des sous-réseaux /32 IPv4 et IPv6 /128 et recherche libres sous-réseaux d’adresses IP et plages dans un bloc d’adresses IP.  
  
-   **Gestion du service DNS améliorée**.  
     IPAM prend en charge l’enregistrement de ressource DNS redirecteur conditionnel et la gestion des zones DNS pour les serveurs DNS de l’intégré à Active Directory et reposant sur le fichier joint au domaine.  
  
-   **Gestion de l’adresse (DDI) DNS, DHCP et IP intégrée**.  
     Plusieurs nouvelles expériences automatiser et gestion du cycle de vie intégrée opérations sont activées, telles que la visualisation de tous les enregistrements de ressource DNS qui se rapportent à une adresse IP, inventaire d’adresses IP basée sur les enregistrements de ressource DNS et la gestion du cycle de vie des adresses IP pour les opérations de DNS et DHCP.  
  
-   **Prise en charge de plusieurs forêts Active Directory**.  
     Vous pouvez utiliser IPAM pour gérer les serveurs DNS et DHCP de plusieurs forêts Active Directory lorsqu’il existe une relation d’approbation bidirectionnelle entre la forêt où IPAM est installé et chacune des forêts à distance.  
  
-   **Prise en charge de Windows PowerShell pour Role Based Access Control**.  
     Vous pouvez utiliser Windows PowerShell pour définir des étendues d’accès sur des objets IPAM.  
  
Pour plus d’informations, voir [Nouveautés dans IPAM](technologies/ipam/What-s-New-in-IPAM.md) et [gérer IPAM](technologies/ipam/Manage-IPAM.md).  
  

