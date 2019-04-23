---
title: Mise en réseau
description: Cette rubrique fournit une vue d’ensemble des technologies de mise en réseau SDN (Software Defined Networking) et de plateforme réseau qui sont disponibles dans Windows Server 2016.
ms.prod: windows-server-threshold
layout: LandingPage
ms.technology: networking
ms.topic: landing-page
ms.assetid: daaf6b61-5953-4c2d-b6b8-7c885b552646
manager: dougkim
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bb5a605ef6438bfa6a2afe4963b8206f9dc84a3a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869350"
---
# <a name="networking"></a>Mise en réseau

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de Windows Server ? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<HR />

Mise en réseau est un aspect fondamental du centre de données défini par logiciel \(SDDC\) plateforme et Windows Server 2016 fournit de nouvelles et améliorées Sdn \(SDN\) technologies pour vous aider à déplacer vers une solution SDDC entièrement réalisée pour votre organisation.

Lorsque vous gérez les réseaux sous la forme de ressources à définition logicielle, vous pouvez décrire une seule fois les exigences d’une application en matière d’infrastructure, puis déterminer à quel emplacement cette application sera exécutée (de manière locale ou dans le cloud). 

Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle ; vous pouvez exécuter des applications en toute transparence, n’importe où, en sachant que la sécurité, les performances, la qualité de service et la disponibilité seront toujours aussi fiables.

<HR />

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
                                        <h2><a href="../networking/What-s-New-in-Networking.md">Nouveautés de la mise en réseau</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

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
                                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">Software Defined Networking (SDN)</a><hr /></h3>Vous pouvez utiliser cette rubrique pour découvrir les technologies de mise en réseau SDN (Software-Defined Networking) offertes par Windows Server, System Center et Microsoft Azure.</p>
                        
                                        <p><b>Remarque :</b> Pour les hôtes Hyper-V et les machines virtuelles \(machines virtuelles\) que vous exécutez des serveurs d’infrastructure SDN, telles que les nœuds de contrôleur de réseau et l’équilibrage de charge, vous devez installer Windows Server Datacenter edition. Pour Hyper-V hôtes qui contiennent uniquement des locataires la charge de travail des machines virtuelles qui sont connectés à SDN\-contrôlé de réseaux, vous pouvez exécuter Windows Server Standard edition.</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">Déployer une infrastructure de réseau à définition logicielle à l’aide de scripts</a><hr /></h3>Ce guide fournit des instructions permettant de déployer le Contrôleur réseau via des passerelles et des réseaux virtuels dans un environnement de laboratoire de test.</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">Contrôleur de réseau</a><hr /></h3>Le Contrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">L’équilibrage de charge logiciel &#40;SLB&#41; pour SDN</a><hr /></h3>Fournisseurs de services cloud \(CSP\) et les entreprises qui déploient la mise en réseau SDN (Software Defined) dans Windows Server 2016 peuvent utiliser l’équilibrage de charge logiciel \(SLB\) pour répartir uniformément le client et client trafic réseau client entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">Passerelle RAS pour SDN</a><hr /></h3>Passerelle RAS, c'est-à-dire un multilocataire basé sur le logiciel, Border Gateway Protocol \(BGP\) routeur capable de Windows Server 2016, est conçu pour les fournisseurs de services Cloud \(CSP\) et les entreprises qui hébergent plusieurs réseaux virtuels des locataires à l’aide de la virtualisation de réseau Hyper-V.</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">Virtualisation de réseau (fonction)</a><hr /></h3>Dans les centres de données software-defined réseau des fonctions qui sont effectuées par les appliances matérielles \(telles que les équilibreurs de charge, les pare-feux, routeurs, commutateurs et ainsi de suite\) sont plus en plus virtualisées, comme des équipements virtuels. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux.</p>                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<HR />
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">Vue d’ensemble du pare-feu de centre de données</a><hr /></h3>Le pare-feu de centre de données est un pare-feu de couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et de destination) avec état et multi-locataire.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

<HR />

## <a name="bkmk_networking"></a>Technologies de mise en réseau

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
                                        <h3><a href="branchcache/BranchCache.md">BranchCache</a><hr /></h3>
                                        <p>BranchCache est un réseau étendu \(WAN\) technologie d’optimisation de la bande passante. Pour optimiser la bande passante d’un réseau étendu lorsque des utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache extrait le contenu des serveurs de contenu de votre siège social ou du cloud hébergé et le met en cache sur les systèmes des filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">Guide du réseau</a><hr /></h3>
                                        <p>Découvrez comment déployer un réseau Windows Server avec le Guide du réseau de base, ainsi que comment ajouter des fonctionnalités à votre déploiement de réseau avec les Guides d’accompagnement du réseau de base.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../remote/remote-access/directaccess/DirectAccess.md">DirectAccess</a><hr /></h3>
                                        <p>DirectAccess assure la connectivité entre les utilisateurs distants et les ressources réseau de l'organisation. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="dns/dns-top.md">Domain Name System (DNS) "></a><hr /></h3>
                                        <p>Le système \(DNS\) fait partie d’une série de protocoles répondant aux normes du secteur, qui inclut le protocole TCP/IP standard. Lorsqu’ils sont associés, le client DNS et le serveur DNS fournissent des services de résolution des noms pour le mappage des noms d’ordinateurs et des adresses IP aux utilisateurs et aux ordinateurs.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/dhcp/dhcp-top.md">Dynamic Host Configuration Protocol &#40;DHCP&#41;</a><hr /></h3>
                                        <p>La fonction Dynamic Host Configuration Protocol \(DHCP\) est un protocole client/serveur qui fournit automatiquement une adresse Internet Protocol \(IP\) et d’autres informations de configuration pertinentes à un hôte IP (par exemple, masque de sous-réseau et passerelle par défaut).</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">Virtualisation de réseau Hyper-V</a><hr /></h3>
                                        <p>La fonction de virtualisation de réseau Hyper-V \(HNV\) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md">Commutateur virtuel Hyper-V</a><hr /></h3>
                                        <p>Le commutateur virtuel Hyper-V désigne un commutateur réseau Ethernet de couche 2 logiciel disponible dans le Gestionnaire Hyper-V lorsque vous installez le rôle serveur Hyper-V. Le commutateur offre des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique à la fois. Qui plus est, le commutateur virtuel Hyper-V assure l’application de la stratégie de sécurité et d’isolement, ainsi que des niveaux de service. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/ipam/ipam-top.md">Gestion des adresses IP &#40;IPAM&#41;</a><hr /></h3>
                                        <p>La fonction de gestion des adresses IP \(IPAM\) est une suite d’outils intégrés permettant d’activer la planification, le déploiement, la gestion et la surveillance de bout en bout de votre infrastructure d’adresses IP, avec une expérience utilisateur enrichie. IPAM découvre automatiquement les serveurs d’infrastructure d’adresses IP et Domain Name System \(DNS\) serveurs sur votre réseau et vous permet de les gérer depuis une interface centrale. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/Network-Load-Balancing.md">Équilibrage de charge réseau</a><hr /></h3>
                                        <p>L’équilibrage de la charge réseau ou Network Load Balancing \(NLB\) répartit le trafic sur plusieurs serveurs à l’aide du protocole réseau TCP/IP. Dans le cas de déploiements qui ne sont pas de type SDN, l’équilibrage de la charge réseau s’assure que les applications sans état, par exemple les serveurs web qui exécutent Internet Information Services \(IIS\), sont évolutives, via l’ajout de serveurs supplémentaires au fur et à mesure de l’augmentation de la charge.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/hpn/hpn-top.md">Mise en réseau hautes performances</a><hr /></h3>
                                        <p>Les technologies de déchargement et d’optimisation du réseau dans Windows Server 2016 incluent des fonctions et des technologies purement logicielles, des fonctions et des technologies intégrées à la fois logicielles et matérielles, ainsi que des fonctions et des technologies purement matérielles.</p>

                                        <p>La documentation suivante sur les technologies de déchargement et d'optimisation est également disponible :<p>
                                        <hr />
                                        <a href="technologies/conv-nic/cnic-top.md">Mise en réseau hautes performances</a><hr />
                                        <a href="technologies/dcb/dcb-top.md">Data Center Bridging (DCB)</a><hr />
                                        <a href="technologies/vrss/vrss-top.md">Virtuel l’échelle côté réception (vRSS)</a>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/nps/nps-top.md">Serveur de stratégie réseau</a><hr /></h3>
                                        <p>
Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/netsh/netsh.md">Network Shell (Netsh)</a><hr /></h3>
                                        <p>
Vous pouvez utiliser l’interpréteur de commandes réseau \(netsh\) mise en réseau de l’utilitaire pour gérer des technologies de mise en réseau dans Windows Server 2016 et Windows 10.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">Performances du sous-système réseau paramétrage</a><hr /></h3>
                                        <p>
Cette rubrique fournit des informations sur le choix de la carte réseau appropriée pour votre charge de travail de serveur, classement des interfaces réseau, les compteurs de performances associées au réseau et optimisation des performances des cartes réseau et les technologies de mise en réseau associées, telles que Trafic entrant \(RSS\), fusion de côté réception \(RSC\)et d’autres.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/nic-teaming/NIC-Teaming.md">Association de cartes réseau</a><hr /></h3>
                                        <p>
La fonction d’association de cartes réseau vous permet de regrouper plusieurs cartes réseau Ethernet physiques dans une ou plusieurs cartes réseau virtuelle à définition logicielle. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/qos/qos-policy-top.md">Qualité de la stratégie de Service (QoS)</a><hr /></h3>
                                        <p>
Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure Active Directory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="technologies/wins/wins-top.md">Windows Internet Name Service (WINS)</a><hr /></h3>
                                        <p>
Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adresses IP. Il est recommandé d'utiliser DNS plutôt que WINS.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../remote/remote-access/remote-access.md">Accès à distance</a><hr /></h3>
                                        <p>
Vous pouvez utiliser des technologies de l’accès à distance, tels que DirectAccess et les réseaux privés virtuels \(VPN\) pour fournir des travailleurs distants avec une connectivité aux ressources réseau internes. En outre, vous pouvez utiliser l’accès à distance pour réseau local \(LAN\) et de routage pour le Proxy d’Application Web. Celui-ci fournit la fonctionnalité de proxy inverse pour les applications web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil depuis l’extérieur du réseau d’entreprise.</p>

                                        <p>Pour plus d’informations sur le Proxy d’Application Web, qui est un service de rôle du rôle de serveur d’accès à distance, consultez <a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">Proxy d’Application Web dans Windows Server 2016</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture">Mise en réseau de conteneur Windows</a><hr /></h3>
                                        <p>
La mise en réseau de conteneurs Windows vous permet de créer et gérer des réseaux de manière à connecter les points de terminaison de conteneur, aussi bien aux hôtes Windows 10 et Windows Server à l’aide d’outils conformes aux normes du secteur et de flux de travail. Les réseaux de conteneurs Windows prennent en charge plusieurs topologies, notamment privée, à plat-couche 2 et routé-couche 3.</p>

                                        <p>Également pris en charge sont des superpositions que vous pouvez créer localement sur l’ordinateur hôte à l’aide de Docker, Kubernetes ou Windows PowerShell via plug-ins qui communiquent avec le Service de mise en réseau Windows hôte \(HNS\). Vous pouvez créer et gérer plusieurs\-réseaux de cluster nœud par le biais des systèmes d’orchestration de niveau supérieur en communiquant via un agent local HNS de chaque nœud.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<HR />
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
                                        <h3><a href="../remote/remote-access/vpn/vpn-top.md">Réseau privé virtuel (VPN)</a><hr /></h3>
                                        <p>
DirectAccess et VPN est un service de rôle du rôle de serveur d'accès à distance.</p>

                                        <p>Lorsque vous installez accès à distance comme un serveur VPN, vous pouvez utiliser de réseaux privés virtuels \(VPN\) pour fournir à vos employés à distance avec des connexions à un réseau de votre organisation sur Internet - tout en conservant les informations confidentialité des données avec des connexions chiffrées.</p>

                                       <p> Avec un VPN Accès à distance Windows Server (et des ordinateurs clients Windows 10), vous pouvez déployer un VPN Toujours actif (AlwaysOn). Un VPN Toujours actif (AlwaysOn) vous donne la possibilité de gérer les clients VPN distants qui sont connectés en permanence, tout en se montrant très pratique pour les collaborateurs distants qui n’ont plus besoin de se connecter ou déconnecter manuellement du VPN du réseau de votre organisation.</p>

                                       <p>Pour plus d’informations, consultez <a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">distant accès toujours sur VPN déploiement Guide pour Windows Server 2016 et Windows 10</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="additional-resources"></a>Ressources complémentaires

Vous trouverez des ressources de mise en réseau pour les systèmes antérieurs à Windows Server 2016 aux emplacements suivants.

- [Vue d’ensemble de la mise en réseau](https://technet.microsoft.com/library/hh831357.aspx) Windows Server 2012 et Windows Server 2012 R2
- [Mise en réseau](https://technet.microsoft.com/library/cc753940) Windows Server 2008 et Windows Server 2008 R2
- Windows Server 2003 [Windows Server 2003/2003 R2 - contenu retiré ](https://www.microsoft.com/download/details.aspx?id=53314)
