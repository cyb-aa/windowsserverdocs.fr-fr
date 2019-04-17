---
title: Réseaux
description: Cette rubrique fournit une vue d’ensemble des technologies de mise en réseau SDN (Software Defined Networking) et de plateforme réseau qui sont disponibles dans WindowsServer2016.
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066839"
---
# Réseaux

>S’applique à: Windows Server (canal semi-annuel), WindowsServer2016

>[!TIP]
> Vous recherchez des informations sur des versions plus anciennes de WindowsServer? Consultez nos autres [bibliothèques Windows Server](/previous-versions/windows/) sur docs.microsoft.com. Vous pouvez également [rechercher dans ce site](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) des informations spécifiques.

<img src="../media/landing-icons/network.png" style='float:left; padding:.5em;' alt="Icon depicting two networked computers"> La mise en réseau est un aspect fondamental de la plateforme de centre de données à définition logicielle \(SDDC, Software-Defined Datacenter\). De ce fait, WindowsServer2016 propose des technologiesSDN \(Software-Defined Networking\) nouvelles et améliorées, afin de vous aider à mettre en place la solutionSDDC la plus adaptée à votre entreprise.

Lorsque vous gérez les réseaux sous la forme de ressources à définition logicielle, vous pouvez décrire une seule fois les exigences d’une application en matière d’infrastructure, puis déterminer à quel emplacement cette application sera exécutée (de manière locale ou dans le cloud). 

Cette cohérence signifie que vos applications sont désormais plus faciles à mettre à l’échelle; vous pouvez exécuter des applications en toute transparence, n’importe où, en sachant que la sécurité, les performances, la qualité de service et la disponibilité seront toujours aussi fiables.

>[!Note]
> Pour télécharger WindowsServer, accédez à [WindowsServer-Évaluations](https://www.microsoft.com/evalcenter/evaluate-windows-server-technical-preview).

WindowsServer2016 ajoute les nouvelles technologies de mise en réseau suivantes:

- FonctionnalitéSDN (Software-Defined Networking): le contrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données. LeContrôleur de réseau vous permet d’utiliser la virtualisation de fonction réseau afin de déployer facilement des machines virtuelles à des fins d’équilibrage de charge pour optimiser les charges du trafic réseau de vos locataires, ainsi que des passerelles de serveur d’accès à distance pour que ces derniers bénéficient des options de connectivité qu’il leur faut entre les ressources Internet, locales et du cloud. CeContrôleur vous permet également de gérer le pare-feu du centre de données sur les machines virtuelles et les hôtesHyper-V.

- Plate-forme réseau: à l’aide des nouvelles fonctionnalités des technologies de plate-forme réseau existantes, vous pouvez utiliser la stratégieDNS pour personnaliser les réponses du serveurDNS aux requêtes, recourir à une carteNIC convergée gérant le trafic de la fonction d’accès direct à la mémoire à distance \(RDMA\) et le traficEthernet, voire utiliser la technologie d’association de commutateurs imbriqués \(SET, Switch Embedded Teaming\) pour créer des commutateurs virtuelsHyper\-V connectés à des cartesNICRDMA. Enfin, vous pouvez gérer des serveurs et des zonesDNS et des adressesIP et DHCP à l’aide de la gestion d’adresses IP \(IPAM\).

Pour en savoir plus, voir [Scénarios de réseaux pris en charge par WindowsServer](windows-server-supported-networking-scenarios.md).

Les sections suivantes fournissent des informations sur les technologies SDN et les technologies de plateforme réseau.

## Technologies de mise en réseauSDN (Software-DefinedNetworking)

### [Mise en réseauSDN (Software-Defined Networking) &#40;SDN&#41;](sdn/software-defined-networking.md)

Vous pouvez utiliser cette rubrique pour découvrir les technologies de mise en réseauSDN (Software-DefinedNetworking) offertes par WindowsServer, System Center et Microsoft Azure.

>[!NOTE]
>Pour les hôtes Hyper\-V et les ordinateurs virtuels qui exécutent des serveurs d’infrastructure SDN, tels que les nœuds de contrôleur de réseau et d’équilibrage de la charge logicielle, vous devez installer WindowsServer2016, édition Datacenter. Pour les hôtes Hyper\-V qui contiennent uniquement des ordinateurs virtuels de charge de travail cliente connectés aux réseaux contrôlés par SDN, vous pouvez exécuter WindowsServer2016, édition Standard.

### [Déployer une infrastructure SDN (Software Defined Networking) avec des scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
Ce guide fournit des instructions permettant de déployer le Contrôleur réseau via des passerelles et des réseaux virtuels dans un environnement de laboratoire de test.

### [Contrôleur réseau](sdn/technologies/network-controller/Network-Controller.md)

LeContrôleur de réseau offre une fonction d’automatisation programmable et centralisée pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.

### [Équilibrage de la charge logicielle &#40;SLB&#41; pourSDN](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)

Les fournisseurs de services cloud et les entreprises qui déploient la mise en réseau SDN dans WindowsServer2016 peuvent utiliser la fonction d’équilibrage de la charge logicielle pour répartir de manière équitable le trafic réseau des locataires et clients locataires entre les différentes ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de WindowsServer vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.

### [Passerelle du serveur d’accès à distance pour SDN](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)

Cette passerelle, définie par logiciel et multi-locataire, peut gérer le routage du protocole de passerelle frontièreBGP \(Border Gateway Protocol\) dans WindowsServer2016. Elle est conçue pour les fournisseurs de servicescloud et les entreprises hébergeant des réseaux virtuels multi\-locataires qui utilisent la fonction de virtualisation du réseau d’Hyper\-V. 

### [Virtualisation de fonction réseau](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)

Dans les centres de données avec définition logicielle, les fonctions réseau effectuées par les dispositifs matériel \(équilibreurs de charge, pare-feu, routeurs, commutateurs, etc.\) sont de plus en plus virtualisées, de façon à jouer le rôle de dispositif virtuel. Cette «virtualisation des fonctions réseau» est une évolution naturelle de la virtualisation des serveurs et des réseaux.

### [Présentation du pare-feu de centre de données](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)

Le pare-feu de centre de données est un pare-feu de couche réseau, 5-tuple (protocole, numéros de port source et de destination, adresses IP source et de destination) avec état et multi-locataire.

## <a name="bkmk_networking"></a>Technologies de mise en réseau

Le tableau suivant fournit des liens vers certaines technologies de mise en réseau de WindowsServer2016.

### [Nouveautés de la mise en réseau](../networking/What-s-New-in-Networking.md)

Les sections suivantes vous permettent de découvrir les nouvelles technologies de mise en réseau et les nouvelles fonctionnalités des technologies existantes dans WindowsServer2016.

### [BranchCache](branchcache/BranchCache.md)

BranchCache est une technologie d’optimisation de la bande passante du réseau étendu. Pour optimiser la bande passante d’un réseau étendu lorsque des utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache extrait le contenu des serveurs de contenu de votre siège social ou du cloud hébergé et le met en cache sur les systèmes des filiales, permettant ainsi aux ordinateurs clients des filiales d’accéder localement au contenu au lieu de passer par le réseau étendu.

### [Guide du réseau de base pour WindowsServer2016](core-network-guide/core-network-guide-windows-server.md)

Découvrez comment déployer un réseau WindowsServer avec le Guide du réseau de base, ainsi que comment ajouter des fonctionnalités à votre déploiement de réseau avec les Guides d’accompagnement du réseau de base.

### [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md)

DirectAccess assure la connectivité entre les utilisateurs distants et les ressources réseau de l’organisation. 

La documentation de DirectAccess se trouve désormais dans la section [Accès à distance et gestion de serveur](https://docs.microsoft.com/windows-server/remote/) de la table des matières de WindowsServer2016, sous [Accès à distance](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access). Pour plus d’informations, voir [DirectAccess](../remote/remote-access/directaccess/DirectAccess.md).

### [Domain Name System &#40;DNS&#41;](dns/dns-top.md)

Le système DNS \(Domain Name System\) fait partie d’une série de protocoles répondant aux normes du secteur, qui inclut le protocoleTCP/IP standard. Lorsqu’ils sont associés, le clientDNS et le serveurDNS fournissent des services de résolution des noms pour le mappage des noms d’ordinateurs et des adressesIP aux utilisateurs et aux ordinateurs.

### [ProtocoleDynamic Host Configuration Protocol &#40;DHCP&#41;](technologies/dhcp/dhcp-top.md)

Le protocoleDynamic Host Configuration Protocol \(DHCP\) est un protocole client/serveur qui fournit automatiquement une adresseInternet Protocol \(IP\) et d’autres informations de configuration pertinentes à un hôteIP \(par exemple, masque de sous-réseau et passerelle par défaut\).

### [Virtualisation de réseau Hyper\-V](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)

La fonction de virtualisation de réseau Hyper\-V \(HNV\) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée.

### [Commutateur virtuel Hyper-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md)

Le commutateur virtuel Hyper-V désigne un commutateur réseau Ethernet de couche 2 logiciel disponible dans le Gestionnaire Hyper-V lorsque vous installez le rôle serveur Hyper-V. Le commutateur offre des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique à la fois. Qui plus est, le commutateur virtuel Hyper\-V assure l’application de la stratégie de sécurité et d’isolement, ainsi que des niveaux de service. 

La documentation du commutateur virtuel Hyper\-V se trouve désormais dans la section **Virtualisation** de la table des matières de WindowsServer2016. Pour plus d’informations, voir [Commutateur virtuel Hyper\-V](../virtualization/hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).

### [Gestion des adressesIP &#40;IPAM&#41;](technologies/ipam/ipam-top.md)

La fonction de gestion des adressesIP \(IPAM\) est une suite d’outils intégrés permettant d’activer la planification, le déploiement, la gestion et la surveillance de bout en bout de votre infrastructure d’adressesIP, avec une Expérience de bureau enrichie. La fonctionIPAM découvre automatiquement les serveurs d’infrastructure d’adressesIP et DNS sur votre réseau et vous permet de les gérer depuis une interface centrale.

### [Équilibrage de la charge réseau](technologies/Network-Load-Balancing.md)

L’équilibrage de la charge réseau ou Network Load Balancing \(NLB\) répartit le trafic sur plusieurs serveurs à l’aide du protocole réseauTCP/IP. Dans le cas des déploiements qui ne sont pas de typeSDN, l’équilibrage de la charge réseau s’assure que les applications sans état, par exemple les serveursweb qui exécutent Internet Information Services \(IIS\), sont évolutives, via l’ajout de serveurs supplémentaires au fur et à mesure de l’augmentation de la charge.

### [Réseaux haute performance](technologies/hpn/hpn-top.md)

Les technologies de déchargement et d’optimisation du réseau dans WindowsServer2016 incluent des fonctions et des technologies purement logicielles, des fonctions et des technologies intégrées à la fois logicielles et matérielles, ainsi que des fonctions et des technologies purement matérielles.

La documentation suivante sur les technologies de déchargement et d’optimisation est également disponible.

- [Guide de configuration de carte d’interface réseau convergé](technologies/conv-nic/cnic-top.md)
- [DCB \(Data Center Bridging\)](technologies/dcb/dcb-top.md)
- [Partage du trafic entrant virtuel \(VRSS\)](technologies/vrss/vrss-top.md)


### [Serveur NPS \(Network Policy Server\)](technologies/nps/nps-top.md)

Un serveur NPS \(Network Policy Server\) vous permet de créer et d’appliquer des stratégies d’accès réseau valides pour toute l’organisation, qui régissent les demandes d’authentification et l’autorisation des demandes de connexion.

### [Network Shell (netsh)](technologies/netsh/netsh.md)

Vous pouvez utiliser l’utilitaire de mise en réseau netsh \(Network Shell\) pour gérer les technologies de mise en réseau dans WindowsServer2016 et Windows10.

### [Réglage des performances du sous-système réseau](technologies/network-subsystem/net-sub-performance-top.md)

Cette rubrique fournit des informations sur le choix de la carte réseau appropriée pour la charge de votre serveur, l’ordonnancement des interfaces réseau, les compteurs de performance liés au réseau, ainsi que les cartes réseau de réglage de performance et technologies de mise en réseau associées telles que le partage du trafic entrant \(RSS\), la fusion du trafic entrant \(RSC\), etc.

### [Association de cartes réseau](technologies/nic-teaming/NIC-Teaming.md)

La fonction d’association de cartes réseau vous permet de regrouper plusieurs cartes réseauEthernet physiques dans une ou plusieurs cartes réseau virtuelles à définition logicielle. Ces cartes réseau virtuelles fournissent des performances élevées et une tolérance de panne importante en cas de défaillance de la carte réseau.

### [Stratégie de qualité de service \(QoS, Quality of Service\)](technologies/qos/qos-policy-top.md)

Vous pouvez utiliser une stratégie de qualité de service comme pilier de la gestion de bande passante réseau dans toute votre infrastructure ActiveDirectory, en créant des profils de qualité de service dont les paramètres sont distribués à l’aide de la stratégie de groupe.

### [Accès à distance](../remote/remote-access/remote-access.md)

Vous pouvez utiliser les technologies d’accès à distance, telles que DirectAccess et un réseau privé virtuel \(VPN, Virtual Private Networking\) pour fournir aux utilisateurs distants la connectivité aux ressources réseau internes. En outre, vous pouvez utiliser l’accès à distance pour le routage du réseau local \(LAN, Local Area Network\) et pour le proxy d’application web. Celui-ci fournit la fonctionnalité de proxy inverse pour les applications web au sein de votre réseau d’entreprise pour permettre aux utilisateurs d’y accéder à partir de n’importe quel appareil depuis l’extérieur du réseau d’entreprise.

La documentation de l’accès à distance se trouve désormais dans la section [Accès à distance et gestion de serveur](https://docs.microsoft.com/windows-server/remote/) de la table des matières de WindowsServer2016. Pour plus d’informations, voir [Accès àdistance](../remote/remote-access/remote-access.md).

Pour plus d’informations sur le proxy d’application web, qui est un service de rôle du rôle serveur Accès à distance, voir [Proxy d’application web dans WindowsServer2016](https://docs.microsoft.com/windows-server/remote/remote-access/web-application-proxy/web-application-proxy-windows-server).

### [Réseau privé virtuel \(VPN, Virtual Private Networking\)](../remote/remote-access/vpn/vpn-top.md)

Dans WindowsServer2016, **DirectAccess et VPN** un service du rôle serveur **Accès à distance**.

Lorsque vous installez Accès à distance en tant que serveur VPN, vous pouvez utiliser la mise en réseau virtuel privé \(VPN\) pour fournir à vos collaborateurs distants la connexion au réseau de votre organisation par Internet \(tout en préservant la confidentialité de vos informations par le chiffrement des connexions\).

Avec un VPN Accès à distance WindowsServer2016\(et des ordinateurs clients Windows10\), vous avez la possibilité de déployer un VPN Toujours actif (AlwaysOn). Un VPN Toujours actif (AlwaysOn) vous donne la possibilité de gérer les clients VPN distants qui sont connectés en permanence, tout en se montrant très pratique pour les collaborateurs distants qui n’ont plus besoin de se connecter ou déconnecter manuellement du VPN du réseau de votre organisation.

Pour plus d’informations, voir [Guide de déploiement de l’accès à distance Toujours actif sur VPN pour WindowsServer2016 et Windows10](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy).

>[!NOTE]
>La documentation VPN se trouve désormais dans la section [Accès à distance et gestion de serveur](https://docs.microsoft.com/windows-server/remote/) de la table des matières de WindowsServer2016, sous [Accès à distance](https://docs.microsoft.com/windows-server/remote/remote-access/remote-access).

Pour plus d’informations sur le VPN, voir [Virtual Private Networking (VPN)](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-top).

### [Mise en réseau de conteneur Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/container-networking)

La mise en réseau de conteneurs Windows vous permet de créer et gérer des réseaux de manière à connecter les points de terminaison de conteneur, aussi bien aux hôtes Windows10 et WindowsServer à l’aide d’outils conformes aux normes du secteur et de flux de travail. Les réseaux de conteneurs Windows prennent en charge plusieurs topologies, notamment privée, à plat-couche2 et routé-couche3.

Sont également prises en charge les superpositions créées localement sur l’hôte à l’aide de Docker, Kubernetes ou Windows PowerShell, par l’intermédiaire de plug-ins qui communiquent avec le service de mise en réseau d’hôte Windows \(HNS\). Vous pouvez créer et gérer des réseaux de clusters à plusieurs nœuds à l’aide de systèmes d’orchestration de niveau plus élevé, en communiquant au travers d’un agent local avec le HNS de chaque nœud.

### [Service WINS \(Windows Internet Name Service\)](technologies/wins/wins-top.md)

Le service WINS (Windows Internet Name Service) représente un service hérité d’inscription et de résolution de noms d’ordinateurs qui mappe les noms NetBIOS d’ordinateurs à des adressesIP. Il est recommandé d’utiliser DNS plutôt que WINS.

## Ressources supplémentaires

Vous trouverez des ressources de mise en réseau pour les systèmes antérieurs à WindowsServer2016 aux emplacements suivants.

- WindowsServer2012 et WindowsServer2012R2 [Vue d’ensemble de la mise en réseau](https://technet.microsoft.com/library/hh831357.aspx)
- WindowsServer2008 et WindowsServer2008R2 [Réseaux](https://technet.microsoft.com/library/cc753940)
- WindowsServer2003 [WindowsServer2003/2003R2 - contenu retiré ](https://www.microsoft.com/download/details.aspx?id=53314)
