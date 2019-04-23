---
title: Mise en réseau
description: Cette rubrique fournit une vue d’ensemble des technologies de mise en réseau SDN (Software Defined Networking) et de plateforme réseau qui sont disponibles dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: 05/08/2018
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 7caa99f1b6b9e25e5a6f2c4333b033fb3088195d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862590"
---
# <a name="networking"></a>Mise en réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de Windows Server ? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> Mise en réseau est un aspect fondamental du centre de données défini par logiciel \(SDDC\) plateforme et Windows Server 2016 fournit de nouvelles et améliorées Sdn \(SDN\) technologies pour vous aider à déplacer vers une solution SDDC entièrement réalisée pour votre organisation.

Lorsque vous gérez les réseaux sous la forme de ressources à définition logicielle, vous pouvez décrire une seule fois les exigences d’une application en matière d’infrastructure, puis déterminer à quel emplacement cette application sera exécutée (de manière locale ou dans le cloud). 

Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle ; vous pouvez exécuter des applications en toute transparence, n’importe où, en sachant que la sécurité, les performances, la qualité de service et la disponibilité seront toujours aussi fiables.

>[!Note]
> Pour télécharger Windows Server, accédez à [Windows Server - Évaluations](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

Windows Server 2016 ajoute les nouvelles technologies de mise en réseau suivantes :

- Mise en réseau logicielle : Le Contrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données. Contrôleur de réseau vous permet d’utiliser la virtualisation de fonction réseau pour déployer facilement des machines virtuelles \(machines virtuelles\) pour l’équilibrage de charge logiciel \(SLB\) pour optimiser le trafic réseau est chargé pour vos clients et RAS Passerelles pour donner aux locataires avec les options de connectivité dont ils ont besoin d’Internet, locales et cloud entre les ressources. Ce Contrôleur vous permet également de gérer le pare-feu du centre de données sur les machines virtuelles et les hôtes Hyper-V.

- Plateforme de réseau : À l’aide des nouvelles fonctionnalités des technologies de plate-forme réseau existantes, vous pouvez utiliser une stratégie DNS pour personnaliser vos réponses du serveur DNS aux requêtes, utiliser une carte de réseau convergé que handles combinées Remote Direct Memory Access \(RDMA\) et le trafic Ethernet , utilisez Switch Embedded Teaming \(définir\) pour créer des commutateurs virtuels Hyper-V connectés à des cartes réseau RDMA et utiliser la gestion des adresses IP \(IPAM\) pour gérer les zones DNS et serveurs ainsi que les adresses IP et DHCP.

Pour en savoir plus, voir [Scénarios de réseaux pris en charge par Windows Server](windows-server-supported-networking-scenarios.md).

Les sections suivantes fournissent des informations sur les technologies SDN et les technologies de plateforme réseau.

## <a name="software-defined-networking-technologies"></a>Technologies de mise en réseau SDN (Software-Defined Networking)

### <a name="software-defined-networking-40sdn41sdnsoftware-defined-networkingmd"></a>[Software Defined Networking &#40;SDN&#41;](sdn/software-defined-networking.md)

Vous pouvez utiliser cette rubrique pour découvrir les technologies de mise en réseau SDN (Software-Defined Networking) offertes par Windows Server, System Center et Microsoft Azure.

>[!NOTE]
>Pour les hôtes Hyper-V et les machines virtuelles \(machines virtuelles\) que vous exécutez des serveurs d’infrastructure SDN, telles que les nœuds de contrôleur de réseau et l’équilibrage de charge, vous devez installer Windows Server 2016 Datacenter edition. Pour Hyper-V hôtes qui contiennent uniquement des locataires la charge de travail des machines virtuelles qui sont connectés à SDN\-contrôlé de réseaux, vous pouvez exécuter Windows Server 2016 Standard edition.

### <a name="deploy-a-software-defined-network-infrastructure-using-scriptssdndeploydeploy-a-software-defined-network-infrastructure-using-scriptsmd"></a>[Déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Ce guide fournit des instructions permettant de déployer le Contrôleur réseau via des passerelles et des réseaux virtuels dans un environnement de laboratoire de test.

### <a name="network-controllersdntechnologiesnetwork-controllernetwork-controllermd"></a>[Contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md)

Le Contrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.

### <a name="software-load-balancing-40slb41-for-sdnsdntechnologiesnetwork-function-virtualizationsoftware-load-balancing-for-sdnmd"></a>[L’équilibrage de charge logiciel &#40;SLB&#41; pour SDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Fournisseurs de services cloud \(CSP\) et les entreprises qui déploient la mise en réseau SDN (Software Defined) dans Windows Server 2016 peuvent utiliser l’équilibrage de charge logiciel \(SLB\) pour répartir uniformément le client et client trafic réseau client entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.

### <a name="ras-gateway-for-sdnsdntechnologiesnetwork-function-virtualizationras-gateway-for-sdnmd"></a>[Passerelle RAS pour SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

Passerelle RAS, c'est-à-dire un multilocataire basé sur le logiciel, Border Gateway Protocol \(BGP\) routeur capable de Windows Server 2016, est conçu pour les fournisseurs de services Cloud \(CSP\) et les entreprises qui hébergent plusieurs réseaux virtuels des locataires à l’aide de la virtualisation de réseau Hyper-V. 

### <a name="network-function-virtualizationsdntechnologiesnetwork-function-virtualizationnetwork-function-virtualizationmd"></a>[Virtualisation de réseau (fonction)](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

Dans les centres de données software-defined réseau des fonctions qui sont effectuées par les appliances matérielles \(telles que les équilibreurs de charge, les pare-feux, routeurs, commutateurs et ainsi de suite\) sont plus en plus virtualisées, comme des équipements virtuels. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux.

### <a name="datacenter-firewall-overviewsdntechnologiesnetwork-function-virtualizationdatacenter-firewall-overviewmd"></a>[Vue d’ensemble du pare-feu de centre de données](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Le pare-feu de centre de données est un pare-feu de couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et de destination) avec état et multi-locataire.

## <a name="bkmk_networking"></a>Technologies de mise en réseau

Le tableau suivant fournit des liens vers certaines technologies de mise en réseau de Windows Server 2016.

### <a name="whats-new-in-networkingnetworkingwhat-s-new-in-networkingmd"></a>[Nouveautés de la mise en réseau](../networking/What-s-New-in-Networking.md)

Les sections suivantes vous permettent de découvrir les nouvelles technologies de mise en réseau et les nouvelles fonctionnalités des technologies existantes dans Windows Server 2016.

### <a name="branchcachebranchcachebranchcachemd"></a>[BranchCache](branchcache/BranchCache.md)

BranchCache est un réseau étendu \(WAN\) technologie d’optimisation de la bande passante. Pour optimiser la bande passante d’un réseau étendu lorsque des utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache extrait le contenu des serveurs de contenu de votre siège social ou du cloud hébergé et le met en cache sur les systèmes des filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.

### <a name="core-network-guide-for-windows-server-2016core-network-guidecore-network-guide-windows-servermd"></a>[Guide du réseau pour Windows Server 2016](core-network-guide/core-network-guide-windows-server.md)

Découvrez comment déployer un réseau Windows Server avec le Guide du réseau de base, ainsi que comment ajouter des fonctionnalités à votre déploiement de réseau avec les Guides d’accompagnement du réseau de base.

### <a name="directaccessremoteremote-accessdirectaccessdirectaccessmd"></a>[DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess assure la connectivité entre les utilisateurs distants et les ressources réseau de l'organisation. 

La documentation de DirectAccess se trouve désormais dans la section [Accès à distance et gestion de serveur](https://docs.microsoft.com/windows-server/remote/) de la table des matières de Windows Server 2016, sous [Accès à distance](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Pour plus d’informations, voir [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### <a name="domain-name-system-40dns41dnsdns-topmd"></a>[Domain Name System &#40;DNS&#41;](dns/dns-top.md)

Le système \(DNS\) fait partie d’une série de protocoles répondant aux normes du secteur, qui inclut le protocole TCP/IP standard. Lorsqu’ils sont associés, le client DNS et le serveur DNS fournissent des services de résolution des noms pour le mappage des noms d’ordinateurs et des adresses IP aux utilisateurs et aux ordinateurs.

### <a name="dynamic-host-configuration-protocol-40dhcp41technologiesdhcpdhcp-topmd"></a>[Dynamic Host Configuration Protocol &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

La fonction Dynamic Host Configuration Protocol \(DHCP\) est un protocole client/serveur qui fournit automatiquement une adresse Internet Protocol \(IP\) et d’autres informations de configuration pertinentes à un hôte IP (par exemple, masque de sous-réseau et passerelle par défaut).

### <a name="hyper-v-network-virtualizationsdntechnologieshyper-v-network-virtualizationhyper-v-network-virtualizationmd"></a>[Virtualisation de réseau Hyper-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

La fonction de virtualisation de réseau Hyper-V \(HNV\) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée.

### <a name="hyper-v-virtual-switchvirtualizationhyper-v-virtual-switchhyper-v-virtual-switchmd"></a>[Commutateur virtuel Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Le commutateur virtuel Hyper-V désigne un commutateur réseau Ethernet de couche 2 logiciel disponible dans le Gestionnaire Hyper-V lorsque vous installez le rôle serveur Hyper-V. Le commutateur offre des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique à la fois. Qui plus est, le commutateur virtuel Hyper-V assure l’application de la stratégie de sécurité et d’isolement, ainsi que des niveaux de service. 

La documentation du commutateur virtuel Hyper\-V se trouve désormais dans la section **Virtualisation** de la table des matières de Windows Server 2016. Pour plus d’informations, voir [Commutateur virtuel Hyper\-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### <a name="ip-address-management-40ipam41technologiesipamipam-topmd"></a>[Gestion des adresses IP &#40;IPAM&#41;](technologies/ipam/ipam-top.md)

La fonction de gestion des adresses IP \(IPAM\) est une suite d’outils intégrés permettant d’activer la planification, le déploiement, la gestion et la surveillance de bout en bout de votre infrastructure d’adresses IP, avec une expérience utilisateur enrichie. IPAM découvre automatiquement les serveurs d’infrastructure d’adresses IP et Domain Name System \(DNS\) serveurs sur votre réseau et vous permet de les gérer depuis une interface centrale.

### <a name="network-load-balancingtechnologiesnetwork-load-balancingmd"></a>[Équilibrage de charge réseau](technologies/Network-Load-Balancing.md)

L’équilibrage de la charge réseau ou Network Load Balancing \(NLB\) répartit le trafic sur plusieurs serveurs à l’aide du protocole réseau TCP/IP. Dans le cas de déploiements qui ne sont pas de type SDN, l’équilibrage de la charge réseau s’assure que les applications sans état, par exemple les serveurs web qui exécutent Internet Information Services \(IIS\), sont évolutives, via l’ajout de serveurs supplémentaires au fur et à mesure de l’augmentation de la charge.

### <a name="high-performance-networkingtechnologieshpnhpn-topmd"></a>[Mise en réseau hautes performances](technologies/hpn/hpn-top.md)

Les technologies de déchargement et d’optimisation du réseau dans Windows Server 2016 incluent des fonctions et des technologies purement logicielles, des fonctions et des technologies intégrées à la fois logicielles et matérielles, ainsi que des fonctions et des technologies purement matérielles.

La documentation suivante sur les technologies de déchargement et d’optimisation est également disponible.

- [Guide de Configuration de réseau convergé Interface (carte)](technologies/conv-nic/cnic-top.md)
- [Data Center Bridging (DCB)](technologies/dcb/dcb-top.md)
- [Virtuel l’échelle côté réception (vRSS)](technologies/vrss/vrss-top.md)


### <a name="network-policy-servertechnologiesnpsnps-topmd"></a>[Serveur de stratégie réseau](technologies/nps/nps-top.md)

Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.

### <a name="network-shell-netshtechnologiesnetshnetshmd"></a>[Network Shell (Netsh)](technologies/netsh/netsh.md)

Vous pouvez utiliser l’interpréteur de commandes réseau \(netsh\) mise en réseau de l’utilitaire pour gérer des technologies de mise en réseau dans Windows Server 2016 et Windows 10.

### <a name="network-subsystem-performance-tuningtechnologiesnetwork-subsystemnet-sub-performance-topmd"></a>[Performances du sous-système réseau paramétrage](technologies/network-subsystem/net-sub-performance-top.md)

Cette rubrique fournit des informations sur le choix de la carte réseau appropriée pour votre charge de travail de serveur, classement des interfaces réseau, les compteurs de performances associées au réseau et optimisation des performances des cartes réseau et les technologies de mise en réseau associées, telles que Trafic entrant \(RSS\), fusion de côté réception \(RSC\)et d’autres.

### <a name="nic-teamingtechnologiesnic-teamingnic-teamingmd"></a>[Association de cartes réseau](technologies/nic-teaming/NIC-Teaming.md)

La fonction d’association de cartes réseau vous permet de regrouper plusieurs cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelle à définition logicielle. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.

### <a name="quality-of-service-qos-policytechnologiesqosqos-policy-topmd"></a>[Qualité de la stratégie de Service (QoS)](technologies/qos/qos-policy-top.md)

Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure Active Directory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.

### <a name="remote-accessremoteremote-accessremote-accessmd"></a>[Accès à distance](../remote/remote-access/remote-access.md)

Vous pouvez utiliser des technologies de l’accès à distance, tels que DirectAccess et les réseaux privés virtuels \(VPN\) pour fournir des travailleurs distants avec une connectivité aux ressources réseau internes. En outre, vous pouvez utiliser l’accès à distance pour réseau local \(LAN\) et de routage pour le Proxy d’Application Web. Celui-ci fournit la fonctionnalité de proxy inverse pour les applications web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil depuis l’extérieur du réseau d’entreprise.

La documentation de l’accès à distance se trouve désormais dans la section [Accès à distance et gestion de serveur](https://docs.microsoft.com/windows-server/remote/) de la table des matières de Windows Server 2016. Pour plus d’informations, voir [Accès à distance](../remote/remote-access/remote-access.md).

Pour plus d’informations sur le proxy d’application web, qui est un service de rôle du rôle serveur Accès à distance, voir [Proxy d’application web dans Windows Server 2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### <a name="virtual-private-networking-vpnremoteremote-accessvpnvpn-topmd"></a>[Réseau privé virtuel (VPN)](../remote/remote-access/vpn/vpn-top.md)

Dans Windows Server 2016, **DirectAccess et VPN** un service du rôle serveur **Accès à distance**.

Lorsque vous installez accès à distance comme un serveur VPN, vous pouvez utiliser de réseaux privés virtuels \(VPN\) pour fournir à vos employés à distance avec des connexions à un réseau de votre organisation sur Internet - tout en conservant les informations confidentialité des données avec des connexions chiffrées.

Avec un VPN Accès à distance Windows Server 2016 \(et des ordinateurs clients Windows 10\), vous avez la possibilité de déployer un VPN Toujours actif (AlwaysOn). Un VPN Toujours actif (AlwaysOn) vous donne la possibilité de gérer les clients VPN distants qui sont connectés en permanence, tout en se montrant très pratique pour les collaborateurs distants qui n’ont plus besoin de se connecter ou déconnecter manuellement du VPN du réseau de votre organisation.

Pour plus d’informations, voir [Guide de déploiement de l’accès à distance Toujours actif sur VPN pour Windows Server 2016 et Windows 10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentation VPN se trouve désormais dans la section [Accès à distance et gestion de serveur](https://docs.microsoft.com/windows-server/remote/) de la table des matières de Windows Server 2016, sous [Accès à distance](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Pour plus d’informations sur le VPN, voir [Virtual Private Networking (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### <a name="windows-container-networkinghttpsdocsmicrosoftcomvirtualizationwindowscontainersmanage-containerscontainer-networking"></a>[Mise en réseau de conteneur Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

La mise en réseau de conteneurs Windows vous permet de créer et gérer des réseaux de manière à connecter les points de terminaison de conteneur, aussi bien aux hôtes Windows 10 et Windows Server à l’aide d’outils conformes aux normes du secteur et de flux de travail. Les réseaux de conteneurs Windows prennent en charge plusieurs topologies, notamment privée, à plat-couche 2 et routé-couche 3.

Également pris en charge sont des superpositions que vous pouvez créer localement sur l’ordinateur hôte à l’aide de Docker, Kubernetes ou Windows PowerShell via plug-ins qui communiquent avec le Service de mise en réseau Windows hôte \(HNS\). Vous pouvez créer et gérer plusieurs\-réseaux de cluster nœud par le biais des systèmes d’orchestration de niveau supérieur en communiquant via un agent local HNS de chaque nœud.

### <a name="windows-internet-name-service-winstechnologieswinswins-topmd"></a>[Windows Internet Name Service (WINS)](technologies/wins/wins-top.md)

Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adresses IP. Il est recommandé d'utiliser DNS plutôt que WINS.

## <a name="additional-resources"></a>Ressources complémentaires

Vous trouverez des ressources de mise en réseau pour les systèmes antérieurs à Windows Server 2016 aux emplacements suivants.

- [Vue d’ensemble de la mise en réseau](https://technet.microsoft.com/library/hh831357.aspx) Windows Server 2012 et Windows Server 2012 R2
- [Mise en réseau](https://technet.microsoft.com/library/cc753940) Windows Server 2008 et Windows Server 2008 R2
- Windows Server 2003 [Windows Server 2003/2003 R2 - contenu retiré ](https://www.microsoft.com/download/details.aspx?id=53314)
