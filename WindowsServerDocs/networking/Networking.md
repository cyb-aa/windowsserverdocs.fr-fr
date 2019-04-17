---
title: Réseaux
description: Cette rubrique fournit une vue d’ensemble des technologies de mise en réseau SDN (Software Defined Networking) et de plateforme réseau qui sont disponibles dans WindowsServer2016.
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
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121398"
---
# Réseaux

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de WindowsServer? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<HR />

La mise en réseau est un aspect fondamental de la plateforme de centre de données à définition logicielle \(SDDC, Software-Defined Datacenter\). De ce fait, WindowsServer2016 propose des technologiesSDN \(Software-Defined Networking\) nouvelles et améliorées, afin de vous aider à mettre en place la solutionSDDC la plus adaptée à votre entreprise.

Lorsque vous gérez les réseaux sous la forme de ressources à définition logicielle, vous pouvez décrire une seule fois les exigences d’une application en matière d’infrastructure, puis déterminer à quel emplacement cette application sera exécutée (de manière locale ou dans le cloud). 

Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle; vous pouvez exécuter des applications en toute transparence, n’importe où, en sachant que la sécurité, les performances, la qualité de service et la disponibilité seront toujours aussi fiables.

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

<h2>SDN (Software-Defined Networking)</h2>

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
                                        <h3><a href="https://docs.microsoft.com/windows-server/networking/sdn/">Mise en réseau SDN (Software-DefinedNetworking)</a><hr /></h3>Vous pouvez utiliser cette rubrique pour découvrir les technologies de mise en réseauSDN (Software-DefinedNetworking) offertes par WindowsServer, SystemCenter et MicrosoftAzure.</p>
                        
                                        <p><b>Remarque:</b> pour les hôtes Hyper-V et les ordinateurs virtuels qui exécutent des serveurs d'infrastructure SDN, tels que les nœuds de contrôleur de réseau et d'équilibrage de la charge logicielle, vous devez installer WindowsServer, édition Datacenter. Pour les hôtes Hyper-V qui contiennent uniquement des ordinateurs virtuels de charge de travail cliente connectés aux réseaux contrôlés par SDN, vous pouvez exécuter WindowsServer, édition Standard.</p>                                        </div>
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
                                        <h3><a href="sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md">Déployer une infrastructure SDN (Software-DefinedNetworking) avec des scripts</a><hr /></h3>Ce guide fournit des instructions permettant de déployer le Contrôleur réseau via des passerelles et des réseaux virtuels dans un environnement de laboratoire de test.</p>                                        </div>
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
                                        <h3><a href="sdn/technologies/network-controller/Network-Controller.md">Contrôleur réseau</a><hr /></h3>LeContrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.</p>                                        </div>
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md">Équilibrage de la charge logicielle &#40;SLB&#41; pourSDN</a><hr /></h3>Les fournisseurs de services cloud et les entreprises qui déploient la mise en réseau SDN dans WindowsServer2016 peuvent utiliser la fonction d’équilibrage de la charge logicielle pour répartir de manière équitable le trafic réseau des locataires et clients locataires entre les différentes ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de WindowsServer vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.</p>                                        </div>
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md">Passerelle du serveur d’accès à distance pour SDN</a><hr /></h3>Cette passerelle, définie par logiciel et multi-locataire, peut gérer le routage du protocole de passerelle frontièreBGP \(Border Gateway Protocol\) dans WindowsServer2016. Elle est conçue pour les fournisseurs de servicescloud et les entreprises hébergeant des réseaux virtuels multi\-locataires qui utilisent la fonction de virtualisation du réseau d’Hyper\-V.</p>                                        </div>
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md">Virtualisation de fonction réseau</a><hr /></h3>Dans les centres de données avec définition logicielle, les fonctions réseau effectuées par les dispositifs matériel \(équilibreurs de charge, pare-feu, routeurs, commutateurs, etc.\) sont de plus en plus virtualisées, de façon à jouer le rôle de dispositif virtuel. Cette «virtualisation des fonctions réseau» est une évolution naturelle de la virtualisation des serveurs et des réseaux.</p>                                        </div>
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
                                        <h3><a href="sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md">Présentation du pare-feu de centre de données</a><hr /></h3>Le pare-feu de centre de données est un pare-feu de couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et de destination) avec état et multi-locataire.</p>
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
                                        <p>BranchCache est une technologie d’optimisation de la bande passante du réseau étendu. Pour optimiser la bande passante d’un réseau étendu lorsque des utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache extrait le contenu des serveurs de contenu de votre siège social ou du cloud hébergé et le met en cache sur les systèmes des filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.</p>
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
                                        <h3><a href="core-network-guide/core-network-guide-windows-server.md">Guide du réseau principal</a><hr /></h3>
                                        <p>Découvrez comment déployer un réseau WindowsServer avec le Guide du réseau de base, ainsi que comment ajouter des fonctionnalités à votre déploiement de réseau avec les Guides d’accompagnement du réseau de base.</p>
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
                                        <h3><a href="dns/dns-top.md">Nom de domaine (DNS)"></a><hr /></h3>
                                        <p>Le système DNS \(Domain Name System\) fait partie d’une série de protocoles répondant aux normes du secteur, qui inclut le protocoleTCP/IP standard. Lorsqu’ils sont associés, le clientDNS et le serveurDNS fournissent des services de résolution des noms pour le mappage des noms d’ordinateurs et des adressesIP aux utilisateurs et aux ordinateurs.</p>
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
                                        <h3><a href="technologies/dhcp/dhcp-top.md">ProtocoleDynamic Host Configuration Protocol &#40;DHCP&#41;</a><hr /></h3>
                                        <p>Le protocoleDynamic Host Configuration Protocol \(DHCP\) est un protocole client/serveur qui fournit automatiquement une adresseInternet Protocol \(IP\) et d’autres informations de configuration pertinentes à un hôteIP \(par exemple, masque de sous-réseau et passerelle par défaut\).</p>
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
                                        <h3><a href="sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md">Virtualisation de réseau Hyper\-V</a><hr /></h3>
                                        <p>La fonction de virtualisation de réseau Hyper\-V \(HNV\) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée.</p>
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
                                        <h3><a href="technologies/ipam/ipam-top.md">Gestion des adressesIP &#40;IPAM&#41;</a><hr /></h3>
                                        <p>La fonction de gestion des adressesIP \(IPAM\) est une suite d’outils intégrés permettant d’activer la planification, le déploiement, la gestion et la surveillance de bout en bout de votre infrastructure d’adressesIP, avec une Expérience de bureau enrichie. La fonctionIPAM découvre automatiquement les serveurs d’infrastructure d’adressesIP et DNS sur votre réseau et vous permet de les gérer depuis une interface centrale. </p>
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
                                        <h3><a href="technologies/Network-Load-Balancing.md">Équilibrage de la charge réseau</a><hr /></h3>
                                        <p>L’équilibrage de la charge réseau ou Network Load Balancing \(NLB\) répartit le trafic sur plusieurs serveurs à l’aide du protocole réseauTCP/IP. Dans le cas des déploiements qui ne sont pas de typeSDN, l’équilibrage de la charge réseau s’assure que les applications sans état, par exemple les serveursweb qui exécutent Internet Information Services \(IIS\), sont évolutives, via l’ajout de serveurs supplémentaires au fur et à mesure de l’augmentation de la charge.</p>
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
                                        <h3><a href="technologies/hpn/hpn-top.md">Réseaux haute performance</a><hr /></h3>
                                        <p>Les technologies de déchargement et d’optimisation du réseau dans WindowsServer2016 incluent des fonctions et des technologies purement logicielles, des fonctions et des technologies intégrées à la fois logicielles et matérielles, ainsi que des fonctions et des technologies purement matérielles.</p>

                                        <p>La documentation suivante sur les technologies de déchargement et d'optimisation est également disponible:<p>
                                        <hr />
                                        <a href="technologies/conv-nic/cnic-top.md">Réseaux haute performance</a><hr />
                                        <a href="technologies/dcb/dcb-top.md">DCB (Data Center Bridging)</a><hr />
                                        <a href="technologies/vrss/vrss-top.md">Partage du trafic entrant virtuel \(VRSS\)</a>
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
                                        <h3><a href="technologies/nps/nps-top.md">Serveur NPS \(Network Policy Server\)</a><hr /></h3>
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
                                        <h3><a href="technologies/netsh/netsh.md">Network Shell (netsh)</a><hr /></h3>
                                        <p>
Vous pouvez utiliser l’utilitaire de mise en réseau netsh \(Network Shell\) pour gérer les technologies de mise en réseau dans WindowsServer2016 et Windows10.</p>
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
                                        <h3><a href="technologies/network-subsystem/net-sub-performance-top.md">Réglage des performances du sous-système réseau</a><hr /></h3>
                                        <p>
Cette rubrique fournit des informations sur le choix de la carte réseau appropriée pour la charge de votre serveur, l’ordonnancement des interfaces réseau, les compteurs de performance liés au réseau, ainsi que les cartes réseau de réglage de performance et technologies de mise en réseau associées telles que le partage du trafic entrant \(RSS\), la fusion du trafic entrant \(RSC\), etc.</p>
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
La fonction d’association de cartes réseau vous permet de regrouper plusieurs cartes réseauEthernet physiques dans une ou plusieurs cartes réseau virtuelles à définition logicielle. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.</p>
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
                                        <h3><a href="technologies/qos/qos-policy-top.md">Stratégie de qualité de service \(QoS, Quality of Service\)</a><hr /></h3>
                                        <p>
Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure ActiveDirectory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.</p>
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
                                        <h3><a href="technologies/wins/wins-top.md">WINS (Windows Internet Name Service)</a><hr /></h3>
                                        <p>
Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adressesIP. Il est recommandé d'utiliser DNS plutôt que WINS.</p>
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
Vous pouvez utiliser les technologies d’accès à distance, telles que DirectAccess et un réseau privé virtuel \(VPN, Virtual Private Networking\) pour fournir aux utilisateurs distants la connectivité aux ressources réseau internes. En outre, vous pouvez utiliser l’accès à distance pour le routage du réseau local \(LAN, Local Area Network\) et pour le proxy d’application web. Celui-ci fournit la fonctionnalité de proxy inverse pour les applications web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil depuis l’extérieur du réseau d’entreprise.</p>

                                        <p>Pour plus d'informations sur le proxy d'application web, qui est un service de rôle du rôle de serveur d'accès à distance, voir <a href="https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server">Proxy d'application web dans WindowsServer2016</a></p>
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
La mise en réseau de conteneurs Windows vous permet de créer et gérer des réseaux de manière à connecter les points de terminaison de conteneur, aussi bien aux hôtes Windows10 et WindowsServer à l’aide d’outils conformes aux normes du secteur et de flux de travail. Les réseaux de conteneurs Windows prennent en charge plusieurs topologies, notamment privée, à plat-couche2 et routé-couche3.</p>

                                        <p>Sont également prises en charge les superpositions créées localement sur l’hôte à l’aide de Docker, Kubernetes ou Windows PowerShell, par l’intermédiaire de plug-ins qui communiquent avec le service de mise en réseau d’hôte Windows \(HNS\). Vous pouvez créer et gérer des réseaux de clusters à plusieurs nœuds à l'aide de systèmes d'orchestration de niveau plus élevé, en communiquant au travers d'un agent local avec le HNS de chaque nœud.</p>
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

                                        <p>Lorsque vous installez Accès à distance en tant que serveur VPN, vous pouvez utiliser la mise en réseau virtuel privé \(VPN\) pour fournir à vos collaborateurs distants la connexion au réseau de votre organisation par Internet \(tout en préservant la confidentialité de vos informations par le chiffrement des connexions\).</p>

                                       <p> Avec un VPN Accès à distance WindowsServer (et des ordinateurs clients Windows10), vous pouvez déployer un VPN Toujours actif (AlwaysOn). Un VPN Toujours actif (AlwaysOn) vous donne la possibilité de gérer les clients VPN distants qui sont connectés en permanence, tout en se montrant très pratique pour les collaborateurs distants qui n’ont plus besoin de se connecter ou déconnecter manuellement du VPN du réseau de votre organisation.</p>

                                       <p>Pour plus d'informations, voir <a href="https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy">Guide de déploiement de l'accès à distance Toujours actif sur VPN pour WindowsServer2016 et Windows10</a></p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## Ressources supplémentaires

Vous trouverez des ressources de mise en réseau pour les systèmes antérieurs à WindowsServer2016 aux emplacements suivants.

- WindowsServer2012 et WindowsServer2012R2 [Vue d’ensemble de la mise en réseau](https://technet.microsoft.com/library/hh831357.aspx)
- WindowsServer2008 et WindowsServer2008R2 [Réseaux](https://technet.microsoft.com/library/cc753940)
- WindowsServer2003 [WindowsServer2003/2003R2 - contenu retiré ](https://www.microsoft.com/download/details.aspx?id=53314)
