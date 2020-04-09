---
title: Mise en réseau
description: Cette rubrique fournit une vue d’ensemble des technologies de mise en réseau SDN (Software Defined Networking) et de plateforme réseau qui sont disponibles dans Windows Server 2016.
ms.prod: windows-server
layout: LandingPage
ms.technology: networking
ms.topic: landing-page
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: anpaul
author: AnirbanPaul
ms.localizationpriority: medium
ms.openlocfilehash: de1acf0a0ed233fdd7675da3bca63f7f16824a9a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855802"
---
# <a name="networking"></a>Mise en réseau

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de Windows Server ? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<hr />

La mise en réseau est une partie fondamentale du centre de donnes défini par le logiciel \(la plateforme du\) SDDC, et Windows Server 2016 fournit une mise en réseau définie et améliorée \(SDN\) technologies pour vous aider à migrer vers une solution SDDC entièrement réalisée pour votre organisation.

Lorsque vous gérez des réseaux en tant que ressource définie par un logiciel, vous pouvez décrire une fois les exigences de l’infrastructure d’une application, puis choisir l’emplacement d’exécution de l’application (localement ou dans le Cloud). 

Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle ; vous pouvez exécuter des applications en toute transparence, n’importe où, en sachant que la sécurité, les performances, la qualité de service et la disponibilité seront toujours aussi fiables.

<hr />

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-whats-new.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h2><a href="../networking/What-s-New-in-Networking.md">Nouveautés&#39;en matière de mise en réseau</a></h2>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

<h2>Mise en réseau SDN (Software Defined Networking)</h2>

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">Mise en réseau SDN (Software Defined Networking)</a></h3>
                        <hr />
                        <p>Vous pouvez utiliser cette rubrique pour découvrir les technologies de mise en réseau SDN (Software-Defined Networking) offertes par Windows Server, System Center et Microsoft Azure.</p>
                        <p><b>Remarque :</b> Pour les ordinateurs hôtes Hyper-V et les machines virtuelles qui exécutent des serveurs d’infrastructure SDN, tels que des nœuds de contrôleur de réseau et d’équilibrage de la charge logicielle, vous devez installer Windows Server Datacenter Edition. Pour les ordinateurs hôtes Hyper-V qui contiennent uniquement des machines virtuelles de charge de travail client connectées à des réseaux contrôlés par SDN, vous pouvez exécuter Windows Server Standard Edition.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                    <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">Déployer une infrastructure réseau définie par logiciel à l’aide de scripts</a></h3>
                    <hr />
                    <p>Ce guide fournit des instructions permettant de déployer le Contrôleur réseau via des passerelles et des réseaux virtuels dans un environnement de laboratoire de test.</p>
                </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">Contrôleur de réseau</a></h3>
                        <hr />
                        <p>Le Contrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">Équilibrage &#40;de charge logiciel&#41; SLB pour Sdn</a></h3>
                        <hr />
                        <p>Les fournisseurs de services Cloud (CSP) et les entreprises qui déploient des réseaux à définition logicielle (SDN) dans Windows Server 2016 peuvent utiliser l’équilibrage de charge logiciel (SLB) pour distribuer uniformément le trafic réseau des clients et des locataires entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">Passerelle du serveur d’accès à distance pour SDN</a></h3>
                        <hr />
                        <p>La passerelle RAS, qui est un routeur basé sur le protocole BGP (Software-based Border Gateway Protocol) de Windows Server 2016, est conçue pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels locataires à l’aide d’un réseau Hyper-V. Virtualisation.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">Virtualisation de fonction réseau</a></h3>
                        <hr />
                        <p>Dans les centres de développement logiciels, les fonctions réseau qui sont effectuées par les appliances matérielles (par exemple, les équilibreurs de charge, les pare-feu, les routeurs, les commutateurs, etc.) sont de plus en plus virtualisées en tant qu’Appliances virtuelles. Cela &quot;la virtualisation de fonction réseau&quot; est une progression naturelle de la virtualisation de serveur et de la virtualisation de réseau.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">Présentation du pare-feu de centre de données</a></h3>
                        <hr />
                        <p>Le pare-feu de centre de données est un pare-feu de couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et de destination) avec état et multi-locataire.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<hr />

## <a name="networking-technologies"></a><a name="bkmk_networking"></a>Technologies de mise en réseau

<ul class="cardsF panelContent">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="branchcache/BranchCache.md">BranchCache</a></h3>
                        <hr />
                        <p>BranchCache est une technologie d’optimisation de la bande passante du réseau étendu. Pour optimiser la bande passante d’un réseau étendu lorsque des utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache extrait le contenu des serveurs de contenu de votre siège social ou du cloud hébergé et le met en cache sur les systèmes des filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">Guide du réseau de base</a></h3>
                        <hr />
                        <p>Découvrez comment déployer un réseau Windows Server avec le Guide du réseau de base, ainsi que comment ajouter des fonctionnalités à votre déploiement de réseau avec les Guides d’accompagnement du réseau de base.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/directaccess/DirectAccess.md">DirectAccess</a></h3>
                        <hr />
                        <p>DirectAccess assure la connectivité entre les utilisateurs distants et les ressources réseau de l'organisation. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
   <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="dns/dns-top.md">DNS (Domain Name System)</a></h3>
                        <hr />
                        <p>Le système DNS (Domain Name System) est l’une des gammes de protocoles standard qui composent le protocole TCP/IP, et le client DNS et le serveur DNS fournissent des services de résolution de noms de nom d’ordinateur à adresse IP aux ordinateurs et aux utilisateurs.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/dhcp/dhcp-top.md">DHCP DHCP (Dynamic &#40;Host Configuration Protocol)&#41;</a></h3>
                        <hr />
                        <p>Le protocole DHCP (Dynamic Host Configuration Protocol) est un protocole client/serveur qui fournit automatiquement un hôte IP (Internet Protocol) avec son adresse IP et d’autres informations de configuration associées, telles que le masque de sous-réseau et la passerelle par défaut.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">Virtualisation de réseau Hyper-V</a></h3>
                        <hr />
                        <p>La virtualisation de réseau Hyper-V (HNV) permet la virtualisation de réseaux clients sur une infrastructure de réseau physique partagée.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">Commutateur virtuel Hyper-V</a></h3>
                        <hr />
                        <p>Le commutateur virtuel Hyper-V désigne un commutateur réseau Ethernet de couche 2 logiciel disponible dans le Gestionnaire Hyper-V lorsque vous installez le rôle serveur Hyper-V. Le commutateur offre des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique à la fois. Qui plus est, le commutateur virtuel Hyper-V assure l’application de la stratégie de sécurité et d’isolement, ainsi que des niveaux de service. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/ipam/ipam-top.md">Gestion &#40;des adresses IP IPAM&#41;</a></h3>
                        <hr />
                        <p>La gestion des adresses IP (IPAM) est une suite d’outils intégrés permettant d’activer la planification, le déploiement, la gestion et la surveillance de bout en bout de votre infrastructure d’adresses IP, avec une expérience utilisateur enrichie. La fonction IPAM découvre automatiquement les serveurs d’infrastructure d’adresses IP et DNS sur votre réseau et vous permet de les gérer depuis une interface centrale.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/Network-Load-Balancing.md">Équilibrage de la charge réseau</a></h3>
                        <hr />
                        <p>L’équilibrage de la charge réseau distribue le trafic sur plusieurs serveurs à l’aide du protocole réseau TCP/IP. Pour les déploiements non-SDN, l’équilibrage de la charge réseau garantit que les applications sans État, telles que les serveurs Web exécutant Internet Information Services (IIS), sont évolutives en ajoutant des serveurs supplémentaires à mesure que la charge augmente.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/hpn/hpn-top.md">Mise en réseau hautes performances</a></h3>
                        <hr />
                        <p>Les technologies de déchargement et d’optimisation du réseau dans Windows Server 2016 incluent des fonctions et des technologies purement logicielles, des fonctions et des technologies intégrées à la fois logicielles et matérielles, ainsi que des fonctions et des technologies purement matérielles.</p>
                        <p>La documentation suivante sur les technologies de déchargement et d'optimisation est également disponible :<p>
                        <hr />
                        <a href="technologies/conv-nic/cnic-top.md">Carte d’interface réseau (NIC) convergée</a>
                        <hr />
                         
                        <a href="technologies/dcb/dcb-top.md">de Data Center Bridging (DCB)</a><hr />
                     
                        <a href="technologies/vrss/vrss-top.md">de la mise à l’échelle côté réception virtuelle (vRSS)</a></div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nps/nps-top.md">NPS (Network Policy Server)</a></h3>
                        <hr />
                        <p>Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/netsh/netsh.md">Environnement réseau (Netsh)</a></h3>
                        <hr />
                        <p>Vous pouvez utiliser l’utilitaire de mise en réseau de l’environnement réseau (Netsh) pour gérer les technologies de mise en réseau dans Windows Server 2016 et Windows 10.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">Réglage des performances du sous-système réseau</a></h3>
                        <hr />
                        <p>Cette rubrique fournit des informations sur le choix de la carte réseau appropriée pour la charge de travail de votre serveur, la commande des interfaces réseau, les compteurs de performances liés au réseau et le réglage des performances des cartes réseau et des technologies de mise en réseau associées, telles que La mise à l’échelle côté réception (RSS), la fusion du trafic entrant (RSC) et d’autres.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/nic-teaming/NIC-Teaming.md">Association de cartes réseau</a></h3>
                        <hr />
                        <p>La fonction d’association de cartes réseau vous permet de regrouper plusieurs cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelle à définition logicielle. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/qos/qos-policy-top.md">Stratégie de qualité de service (QoS)</a></h3>
                        <hr />
                        <p>Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure Active Directory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="technologies/wins/wins-top.md">Service WINS Windows Internet Name Service)</a></h3>
                        <hr />
                        <p>Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adresses IP. Il est recommandé d'utiliser DNS plutôt que WINS.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/remote-access.md">Accès à distance</a></h3>
                        <hr />
                        <p>Vous pouvez utiliser les technologies d’accès à distance, telles que DirectAccess et le réseau privé virtuel (VPN) pour fournir aux employés à distance une connectivité aux ressources réseau internes. En outre, vous pouvez utiliser l’accès à distance pour le routage de réseau local (LAN) et pour le proxy d’application Web. Celui-ci fournit la fonctionnalité de proxy inverse pour les applications web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil depuis l’extérieur du réseau d’entreprise.</p>
                        <p>Pour plus d’informations sur le proxy d’application Web, qui est un service de rôle du rôle de serveur d’accès à distance, voir <a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">proxy d’application Web dans Windows server 2016</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture">Mise en réseau de conteneur Windows</a></h3>
                        <hr />
                        <p>La mise en réseau de conteneurs Windows vous permet de créer et gérer des réseaux de manière à connecter les points de terminaison de conteneur, aussi bien aux hôtes Windows 10 et Windows Server à l’aide d’outils conformes aux normes du secteur et de flux de travail. Les réseaux de conteneurs Windows prennent en charge plusieurs topologies, notamment privée, à plat-couche 2 et routé-couche 3.</p>
                        <p>Les superpositions que vous pouvez créer localement sur l’hôte sont également prises en charge à l’aide de Dockr, Kubernetes ou Windows PowerShell via les plug-ins qui communiquent avec le service de mise en réseau d’hôte Windows (HNS). Vous pouvez créer et gérer des réseaux de clusters à plusieurs nœuds via des systèmes d’orchestration de niveau supérieur en communiquant via un agent local à chaque HNS de nœud.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <hr />
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-network.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3><a href="../remote/remote-access/vpn/vpn-top.md">Réseau privé virtuel (VPN)</a></h3>
                        <hr />
                        <p>DirectAccess et VPN est un service de rôle du rôle de serveur d'accès à distance.</p>
                        <p>Lorsque vous installez l’accès à distance en tant que serveur VPN, vous pouvez utiliser le réseau privé virtuel (VPN) pour fournir à vos employés distants des connexions au réseau de votre organisation via Internet, tout en maintenant la confidentialité des informations avec des connexions chiffrées. .</p>
                        <p> Avec un VPN Accès à distance Windows Server (et des ordinateurs clients Windows 10), vous pouvez déployer un VPN Toujours actif (AlwaysOn). Un VPN Toujours actif (AlwaysOn) vous donne la possibilité de gérer les clients VPN distants qui sont connectés en permanence, tout en se montrant très pratique pour les collaborateurs distants qui n’ont plus besoin de se connecter ou déconnecter manuellement du VPN du réseau de votre organisation.</p>
                        <p>Pour plus d’informations, consultez <a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">Guide de déploiement de l’accès à distance Always on VPN pour Windows Server 2016 et Windows 10</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="additional-resources"></a>Ressources supplémentaires

Vous trouverez des ressources de mise en réseau pour les systèmes antérieurs à Windows Server 2016 aux emplacements suivants.

- [Vue d’ensemble de la mise en réseau](https://technet.microsoft.com/library/hh831357.aspx) Windows Server 2012 et Windows Server 2012 R2
- [Mise en réseau](https://technet.microsoft.com/library/cc753940) Windows Server 2008 et Windows Server 2008 R2
- Contenu retiré de Windows Server 2003 [Windows server 2003/2003 R2](https://www.microsoft.com/download/details.aspx?id=53314)
