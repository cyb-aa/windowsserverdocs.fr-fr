---
title: Nouveautés de la mise en réseau
description: Cette rubrique fournit des informations générales sur les nouvelles fonctionnalités et technologies de mise en réseau dans Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: get-started-article
ms.assetid: 08fb7563-d319-48a9-b181-ca0ca3032c18
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 43ce6290f6559be7cb078032b79519d1681506d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829190"
---
# <a name="whats-new-in-networking"></a>Nouveautés de la mise en réseau

>S’applique à : Windows Server 2016

Voici les technologies nouvelles ou améliorées de mise en réseau dans Windows Server 2016.  
  UPD cette rubrique contient les sections suivantes.  
  
-   [Nouvelles fonctionnalités de mise en réseau et Technologies](#bkmk_features)  
  
-   [Nouvelles fonctionnalités des Technologies de mise en réseau supplémentaires](#bkmk_existing)  
  
## <a name="bkmk_features"></a>Nouvelles fonctionnalités de mise en réseau et Technologies

Mise en réseau est un aspect fondamental de la plateforme logicielle définie par centre de données (SDDC) et Windows Server 2016 fournit des technologies de mise en réseau SDN (Software Defined) nouvelles et améliorées pour vous aider à déplacer vers une solution SDDC entièrement réalisée pour votre organisation.  
  
Lorsque vous gérez les réseaux en tant que ressources à définition logicielle, vous pouvez décrire les exigences de l’infrastructure d’une application une seule fois, puis choisissez qui exécute l’application - en local ou dans le cloud.  Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle et vous pouvez exécuter en toute transparence des applications, n’importe où, en sachant sécurité, aux performances de qualité de service et de disponibilité.  
  
Les sections suivantes contiennent des informations sur ces nouvelles réseau fonctionnalités et technologies.  
  
### <a name="software-defined-networking-infrastructure"></a>Infrastructure de mise en réseau à définition logicielle

Voici les technologies d’infrastructure SDN nouvelles ou améliorées.  
  
-   **Contrôleur de réseau**. Nouveau dans Windows Server 2016, contrôleur de réseau offre un centralisé programmable d’automation pour gérer, configurer, surveiller et dépanner l’infrastructure de réseau virtuel et physique dans votre centre de données. À l'aide du contrôleur de réseau, vous pouvez automatiser la configuration de l'infrastructure réseau au lieu d'effectuer une configuration manuelle des services et appareils réseau. Pour plus d’informations, consultez [contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md) et [déployer des réseaux à définition logicielle à l’aide de scripts](https://technet.microsoft.com/library/mt427380.aspx).  
  
-   **Commutateur virtuel Hyper-V**. Le commutateur virtuel Hyper-V s’exécute sur les hôtes Hyper-V et vous permet de créer de commutation distribuée et le routage, et une couche de mise en œuvre de stratégie qui est alignée et compatible avec Microsoft Azure. Pour plus d’informations, voir [Commutateur virtuel Hyper\-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
-   **Réseau de virtualisation de fonction (NFV)**. Dans les logiciels d’aujourd'hui centres de données définis, les fonctions de réseau qui sont effectuées par les appliances matérielles (par exemple, les équilibreurs de charge, les pare-feux, routeurs, commutateurs, etc.) sont plus en plus déployées comme des équipements virtuels. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux. Appliances virtuelles sont rapidement émergentes et créer un tout nouveau marché. Ils continuent à susciter l’intérêt et de gagner du terrain dans les deux plateformes de virtualisation et de services cloud. Les technologies NFV suivantes sont disponibles dans Windows Server 2016.  
  
    -   **Pare-feu de centre de données**. Ce pare-feu distribué fournit des listes de contrôle d’accès granulaire (ACL), ce qui vous permet d’appliquer des stratégies de pare-feu au niveau de l’interface de machine virtuelle ou au niveau du sous-réseau.  
  
        Pour plus d’informations, consultez [vue d’ensemble du pare-feu de centre de données](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  
    -   **Passerelle RAS**. Vous pouvez utiliser la passerelle RAS pour acheminer le trafic entre réseaux virtuels et des réseaux physiques, notamment des connexions VPN de site à site à partir de votre centre de données cloud à des sites distants de vos clients. Plus précisément, vous pouvez déployer Internet Key Exchange version 2 (IKEv2) site à site réseaux privés virtuels (VPN), VPN de couche 3 (L3) et les passerelles d’Encapsulation GRE (Generic Routing). En outre, les pools de passerelle et une redondance M + N de passerelles sont maintenant pris en charge ; et le protocole BGP (Border Gateway) avec les fonctionnalités de réflecteur d’itinéraire fournit un routage dynamique entre les réseaux pour tous les scénarios de passerelle (VPN IKEv2 VPN GRE et L3 VPN).  
  
        Pour plus d’informations, consultez [What ' s New in passerelle RAS](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [passerelle RAS pour SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).  
          
    - **Équilibreur de charge logiciel (SLB) et adresses réseau (NAT) de traduction**. Le nord-sud et est-ouest de la couche 4 équilibreur de charge et NAT améliore le débit en prenant en charge retour Direct du serveur, avec lequel le trafic réseau entrant peut contourner l’équilibrage de charge multiplexeur.  
       Pour plus d’informations, consultez [l’équilibrage de charge logiciel &#40;SLB&#41; pour SDN](sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
    Pour plus d’informations, consultez [virtualisation de fonction réseau](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
-   **Standardisé protocoles**. Contrôleur de réseau utilise Representational State Transfer (REST) sur son interface northbound avec des charges utiles de JavaScript Objet Notation (JSON). L’interface southbound du contrôleur de réseau utilise Open vSwitch protocole de gestion de base de données (OVSDB).  
  
-   **Technologies d’encapsulation flexible**. Ces technologies fonctionnent au niveau du plan de données et prend en charge de réseau local virtuel Extensible (VxLAN) et réseau virtualisation générique Encapsulation routage (NVGRE). Pour plus d’informations, consultez [Tunneling GRE dans Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
Pour plus d’informations sur SDN, consultez [Sdn &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
### <a name="cloud-scale-fundamentals"></a>Principes de base de mise à l’échelle cloud
 
Les principes de base de mise à l’échelle cloud suivantes sont désormais disponibles.  
  
-   **Carte d’Interface réseau (NIC) de convergé**. La carte réseau convergence vous permet utilisent une seule carte réseau pour la gestion de stockage prenant en charge l’accès à distance directe mémoire RDMA et le trafic de client. Cela réduit les dépenses d’investissement qui sont associés à chaque serveur dans votre centre de données, car vous avez besoin de moins de cartes réseau pour gérer différents types de trafic sur chaque serveur.  
  
-   **Packet Direct**.  Packet Direct fournit un débit de trafic réseau élevé et une infrastructure de traitement des paquets de faible latence.  
  
-   **Switch Embedded Teaming (SET)**.        JEU est une solution d’association de cartes réseau qui est intégrée dans le commutateur virtuel Hyper-V. JEU permet l’association de cartes réseau physiques jusqu'à huit dans une seule équipe de jeu, ce qui améliore la disponibilité et fournit un basculement. Dans Windows Server 2016, vous pouvez créer des équipes de jeu sont limitées à l’utilisation de bloc de Message serveur (SMB) et RDMA. En outre, vous pouvez utiliser les équipes de jeu pour répartir le trafic réseau pour la virtualisation de réseau Hyper-V. Pour plus d’informations, consultez [Remote Direct Memory Access &#40;RDMA&#41; et Switch Embedded Teaming &#40;définir&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
## <a name="bkmk_existing"></a>Nouvelles fonctionnalités des Technologies de mise en réseau supplémentaires

Cette section contient des informations sur les nouvelles fonctionnalités des technologies de mise en réseau familiers.
  
## <a name="bkmk_dhcp"></a>DHCP  
Le protocole DHCP est une norme IETF (Internet Engineering Task Force) conçue pour réduire le travail d’administration et la complexité de configuration des hôtes sur un réseau TCP/IP, tel qu’un intranet privé. Le recours au service Serveur DHCP permet d’automatiser le processus de configuration de TCP/IP sur les clients DHCP.  
  
Pour plus d’informations, consultez [quelles sont les nouveautés dans DHCP](technologies/dhcp/What-s-New-in-DHCP.md).  
  
## <a name="bkmk_dns"></a>DNS  
Le service DNS permet de nommer les ordinateurs et les services de réseau dans les réseaux TCP/IP. L’attribution de noms DNS permet de rechercher les ordinateurs et les services au moyen de noms conviviaux. Lorsqu’un utilisateur entre un nom DNS dans une application, les services DNS peuvent résoudre ce nom en une autre information qui lui est associée, par exemple une adresse IP.  
  
Voici quelques informations sur le Client DNS et serveur DNS.  
  
### <a name="bkmk_dnsc"></a>Client DNS  
Voici les technologies de client DNS nouvelles ou améliorées.  
  
-   **Liaison de service Client DNS**. Dans Windows 10, le service Client DNS offre une prise en charge améliorée pour les ordinateurs avec plusieurs interfaces réseau.  
  
Pour plus d’informations, consultez [What ' s New in Client DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Client.md)  
  
### <a name="bkmk_dnss"></a>Serveur DNS  
Voici les technologies de serveur DNS nouvelles ou améliorées.  
  
-   **Les stratégies DNS**.  Vous pouvez configurer des stratégies DNS pour spécifier comment un serveur DNS répond aux requêtes DNS. Les réponses DNS peuvent reposer sur l’adresse IP du client (emplacement), l’heure de la journée et plusieurs autres paramètres. Les stratégies DNS permettent la géolocalisation DNS, gestion du trafic, l’équilibrage de charge, DNS « split brain » et autres scénarios.  
  
-   **Prise en charge de nano Server pour le fichier basée DNS**, vous pouvez déployer le serveur DNS dans Windows Server 2016 sur une image Nano Server. Cette option de déploiement est disponible pour vous si vous utilisez DNS basée sur les fichiers. Par le serveur DNS en cours d’exécution sur une image Nano Server, vous pouvez exécuter vos serveurs DNS avec un encombrement réduit, de démarrage rapide et la mise à jour corrective réduite.  
  
    > [!NOTE]   
    > DNS intégré à Active Directory n’est pas pris en charge sur Nano Server.  
  
-   **Réponse taux de limitation (RRL)**.  Vous pouvez activer la limitation du débit réponse sur vos serveurs DNS. Ce faisant, vous évitez la possibilité de systèmes malveillants à l’aide de vos serveurs DNS pour lancer une attaque par déni de service sur un client DNS.  
  
-   **L’authentification basée sur DNS d’entités nommées (DANE)**.   Vous pouvez utiliser des enregistrements TLSA (authentification de Transport Layer Security) pour fournir des informations aux clients DNS qui état quelle autorité de certification (CA) doivent-ils en attendre un certificat à partir de votre nom de domaine. Cela empêche les attaques man-in-the-middle où quelqu'un peut endommager le cache DNS pour pointer vers son propre site Web et fournir une attestation à partir d’une autre autorité de certification.  
  
-   **Prise en charge de l’enregistrement inconnu**.   
     Vous pouvez ajouter des enregistrements qui ne sont pas explicitement prises en charge par le serveur DNS de Windows à l’aide de la fonctionnalité d’enregistrement inconnu.  
  
-   **Indications de racine IPv6**.   
     Vous pouvez utiliser le protocole IPV6 natif prend en charge des indications de racine pour effectuer la résolution de noms internet à l’aide de serveurs racine IPV6.  
  
-   **Prise en charge de Windows PowerShell améliorée**.   
      Nouvelles applets de commande Windows PowerShell sont disponibles pour le serveur DNS.  
  
Pour plus d’informations, consultez [What ' s New in serveur DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Server.md)  
  
## <a name="bkmk_GRE"></a>Tunneling GRE  
Passerelle RAS prend désormais en charge les tunnels GRE Generic Routing Encapsulation () de haute disponibilité pour les connexions de site à site et de redondance M + N des passerelles. GRE est un protocole de tunneling léger qui peut encapsuler une grande variété de protocoles de la couche réseau dans les liaisons point à point virtuelles sur un réseau d’interconnexion IP.  
  
Pour plus d’informations, consultez [Tunneling GRE dans Windows Server 2016](../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="HNV"></a>Virtualisation de réseau Hyper-V  
Introduite dans Windows Server 2012, la virtualisation de réseau Hyper-V (HNV) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée. Moyennant des changements minimaux nécessaires sur l’infrastructure réseau physique, HNV fournit des fournisseurs de services l’agilité nécessaire pour déployer et migrer des charges de travail clientes n’importe où sur les trois clouds : le service fournisseur cloud, le cloud privé ou le cloud public Microsoft Azure.  
  
Pour plus d’informations, consultez [What ' s New in virtualisation de réseau Hyper-V dans Windows Server 2016](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)  
  
## <a name="bkmk_ipam"></a>IPAM  
IPAM fournit des capacités d’administration et d’analyse hautement personnalisables pour l’adresse IP et une infrastructure DNS sur un réseau d’entreprise. L’utilisation d’IPAM, vous pouvez surveiller, d’audit et gérer les serveurs qui exécutent DHCP Dynamic Host Configuration Protocol () et le système DNS (Domain Name).  
  
-   **Amélioré la gestion des adresses IP**.  
     Fonctionnalités d’IPAM sont améliorées pour les scénarios tels que la gestion des sous-réseaux /32 d’IPv4 et IPv6 /128 et recherche gratuits sous-réseaux d’adresses IP et plages dans un bloc d’adresses IP.  
  
-   **Amélioré la gestion des services DNS**.  
     IPAM prend en charge l’enregistrement de ressource DNS redirecteur conditionnel et la gestion des zones DNS pour les serveurs DNS de l’intégré à Active Directory et la sauvegarde de fichiers joints au domaine.  
  
-   **Gestion d’adresse (DDI) DNS, DHCP et IP intégrée**.  
     Plusieurs nouvelles expériences et les opérations de gestion du cycle de vie intégrée sont activées, telles que la visualisation des ressources DNS tous les enregistrements qui se rapportent à une adresse IP d’adresses, un inventaire automatisé des adresses IP en fonction des enregistrements de ressource DNS et la gestion du cycle de vie des adresses IP pour les opérations DNS et DHCP.  
  
-   **Prise en charge de plusieurs forêts Active Directory**.  
     Vous pouvez utiliser IPAM pour gérer les serveurs DNS et DHCP de plusieurs forêts Active Directory lorsqu’il existe une relation d’approbation bidirectionnelle entre la forêt où IPAM est installé et que chacune des forêts distantes.  
  
-   **Prise en charge de Windows PowerShell pour Role Based Access Control**.  
     Vous pouvez utiliser Windows PowerShell pour définir des étendues d’accès sur des objets IPAM.  
  
Pour plus d’informations, consultez [What ' s New in IPAM](technologies/ipam/What-s-New-in-IPAM.md) et [gérer IPAM](technologies/ipam/Manage-IPAM.md).  
  

