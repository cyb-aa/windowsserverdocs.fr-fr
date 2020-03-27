---
title: Équilibrage de la charge logicielle pour SDN
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur l’équilibrage de charge logiciel pour la mise en réseau définie par logiciel dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1e7870e045f9af79ed46ec1ad998dbc1f1474afd
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312904"
---
# <a name="software-load-balancing-slb-for-sdn"></a>Équilibrage de charge logiciel \(\) SLB pour SDN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’équilibrage de charge logiciel pour la mise en réseau définie par logiciel dans Windows Server 2016.  

Les fournisseurs de services Cloud (CSP) et les entreprises qui déploient des réseaux à définition logicielle (SDN) dans Windows Server 2016 peuvent utiliser l’équilibrage de charge logiciel (SLB) pour distribuer uniformément le trafic réseau des clients et des locataires entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.
  
Windows Server SLB comprend les fonctionnalités suivantes.  
  
-   Les services d’équilibrage de charge de couche 4 (L4) pour le trafic TCP/UDP « Nord-Sud » et « est-ouest ».  
  
-   Équilibrage de charge du trafic réseau public et interne.  
  
-   Prend en charge les adresses IP dynamiques (DIP) sur les réseaux locaux virtuels (VLAN) et les réseaux virtuels que vous créez à l’aide de la virtualisation de réseau Hyper-V.  
  
-   Prise en charge des sondes d’intégrité.  
  
-   Prêt pour la mise à l’échelle du Cloud, y compris la capacité de montée en puissance parallèle et la capacité de montée en puissance des multiplexeurs et des agents hôtes.  
  
Pour plus d’informations, consultez [fonctionnalités d’équilibrage de charge logiciel](#bkmk_features) dans cette rubrique.  
  
> [!NOTE]  
> L’architecture mutualisée pour les réseaux locaux virtuels n’est pas prise en charge par le contrôleur de réseau. Toutefois, vous pouvez utiliser des réseaux locaux virtuels avec SLB pour les charges de travail gérées par le fournisseur de services, telles que l’infrastructure de centre de centres et les serveurs Web à haute densité.  
  
À l’aide de Windows Server SLB, vous pouvez faire évoluer vos capacités d’équilibrage de charge à l’aide de machines virtuelles SLB sur les mêmes serveurs de calcul Hyper-V que ceux que vous utilisez pour vos autres charges de travail de machine virtuelle. Pour cette raison, SLB prend en charge la création et la suppression rapides des points de terminaison d’équilibrage de charge requis pour les opérations CSP. En outre, Windows Server SLB prend en charge des dizaines de gigaoctets par cluster, fournit un modèle de provisionnement simple et est facile à mettre à l’échelle et dans.  
  
**Fonctionnement de SLB**  
  
SLB fonctionne en mappant les adresses IP virtuelles (VIP) aux adresses IP dynamiques (DIP) qui font partie d’un ensemble de ressources de service Cloud dans le centre de ressources.  
  
Les adresses IP virtuelles sont des adresses IP uniques qui fournissent un accès public à un pool de machines virtuelles à charge équilibrée. Par exemple, les adresses IP virtuelles sont des adresses IP qui sont exposées sur Internet afin que les clients et les clients de locataire puissent se connecter aux ressources du client dans le centre de Cloud.  
  
Les adresses IP virtuelles sont les adresses IP des machines virtuelles membres d’un pool à charge équilibrée derrière l’adresse IP virtuelle. Les adresses IP (DIP) sont affectées dans l’infrastructure cloud aux ressources du locataire.  
  
Les adresses IP virtuelles sont situées dans le multiplexeur SLB (MUX).  Le MULTIPLEXeur est constitué d’une ou plusieurs machines virtuelles (VM).  Le contrôleur de réseau fournit chaque MULTIPLEXeur avec chaque adresse IP virtuelle, et chaque MULTIPLEXeur utilise à son tour le Border Gateway Protocol (BGP) pour publier chaque adresse IP virtuelle sur les routeurs sur le réseau physique en tant que route/32.  BGP permet aux routeurs de réseau physique d’effectuer les opérations suivantes :  
  
-   Découvrez qu’une adresse IP virtuelle est disponible sur chaque MUX, même si les multiplexeurs se trouvent sur des sous-réseaux différents dans un réseau de couche 3.  
  
-   Répartissez la charge pour chaque adresse IP virtuelle sur l’ensemble des multiplexeurs disponibles à l’aide du routage à chemins d’accès multiples (ECMP) égal.  
  
-   Détecte automatiquement un échec ou une suppression du MULTIPLEXeur et arrête l’envoi du trafic vers le MUX défaillant.  
  
-   Répartissez la charge à partir du MULTIPLEXeur en échec ou supprimé sur le multiplexeurs sain.  
  
Lorsque le trafic public arrive à partir d’Internet, le MULTIPLEXeur SLB examine le trafic, qui contient l’adresse IP virtuelle en tant que destination, et mappe et réécrit le trafic pour qu’il arrive à un DIP individuel. Pour le trafic réseau entrant, cette transaction est exécutée dans un processus en deux étapes qui est réparti entre les machines virtuelles MUX et l’hôte Hyper-V dans lequel se trouve l’adresse DIP de destination :  
  
-   Équilibrage de charge : le MULTIPLEXeur utilise l’adresse IP virtuelle pour sélectionner un DIP, encapsuler le paquet et transférer le trafic vers l’hôte Hyper-V où se trouve l’adresse DIP.  
  
-   Traduction d’adresses réseau (NAT) : l’hôte Hyper-V supprime l’encapsulation du paquet, convertit l’adresse IP virtuelle en adresse DIP, remappe les ports et transfère le paquet à la machine virtuelle DIP.  
  
Le MULTIPLEXeur sait comment mapper les adresses IP virtuelles aux adresses IP virtuelles correctes en raison des stratégies d’équilibrage de charge que vous définissez à l’aide du contrôleur de réseau. Ces règles incluent le protocole, le port frontal, le port principal et l’algorithme de distribution (5, 3 ou 2 tuples).  
  
Lorsque les machines virtuelles clientes répondent et envoient le trafic réseau sortant vers Internet ou les emplacements distants, étant donné que le NAT est exécuté par l’hôte Hyper-V, le trafic contourne le MULTIPLEXeur et passe directement au routeur de périphérie de l’hôte Hyper-V. Ce processus de contournement du MULTIPLEXeur est appelé « retour direct du serveur » (DSR).  
  
Après l’établissement du flux de trafic réseau initial, le trafic réseau entrant contourne complètement le MULTIPLEXeur SLB.  
  
Dans l’illustration suivante, un ordinateur client exécute une requête DNS pour l’adresse IP d’un site SharePoint d’entreprise. dans ce cas, il s’agit d’une société fictive nommée contoso. Le processus suivant se produit.  
  
-   Le serveur DNS renvoie l’adresse IP virtuelle 107.105.47.60 au client.  
  
-   Le client envoie une requête HTTP à l’adresse IP virtuelle.  
  
-   Le réseau physique dispose de plusieurs chemins d’accès disponibles pour atteindre l’adresse IP virtuelle située sur un MULTIPLEXeur.  Chaque routeur utilisé utilise ECMP pour choisir le segment suivant du chemin d’accès jusqu’à ce que la demande arrive sur un MULTIPLEXeur.  
  
-   Le MULTIPLEXeur qui reçoit la demande vérifie les stratégies configurées et constate qu’il y a deux DIP disponibles, 10.10.10.5 et 10.10.20.5, sur un réseau virtuel pour traiter la demande à l’adresse VIP 107.105.47.60  
  
-   Le MULTIPLEXeur sélectionne le 10.10.10.5 DIP et encapsule les paquets à l’aide de VXLAN pour qu’il puisse les envoyer à l’hôte contenant l’DIP à l’aide de l’adresse réseau physique des hôtes.  
  
-   L’hôte reçoit le paquet encapsulé et l’inspecte.  Elle supprime l’encapsulation et réécrit le paquet afin que la destination soit maintenant le 10.10.10.5 DIP au lieu de l’adresse VIP et envoie le trafic à la machine virtuelle DIP.  
  
-   La demande a maintenant atteint le site contoso SharePoint dans la batterie de serveurs 2. Le serveur génère une réponse et l’envoie au client, en utilisant sa propre adresse IP comme source.  
  
-   L’hôte intercepte le paquet sortant dans le commutateur virtuel qui se souvient que le client, à présent la destination, a effectué la demande d’origine à l’adresse IP virtuelle.  L’hôte réécrit la source du paquet comme adresse IP virtuelle, de sorte que le client ne voit pas l’adresse DIP.  
  
-   L’hôte transfère le paquet directement à la passerelle par défaut pour le réseau physique qui utilise sa table de routage standard pour transférer le paquet au client qui reçoit finalement la réponse.  
  
![Processus d’équilibrage de charge logiciel](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Équilibrage de charge du trafic interne du centre de charge**  
  
Lors de l’équilibrage de charge du trafic réseau interne au centre de ressources, par exemple entre les ressources de locataire qui s’exécutent sur des serveurs différents et qui sont membres du même réseau virtuel, le commutateur virtuel Hyper-V auquel les machines virtuelles sont connectées exécute NAT.  
  
Avec l’équilibrage de charge du trafic interne, la première demande est envoyée et traitée par le MULTIPLEXeur, qui sélectionne l’adresse DIP appropriée et achemine le trafic vers l’adresse DIP. À partir de là, le flux de trafic établi contourne le MULTIPLEXeur et passe directement de la machine virtuelle à la machine virtuelle.  
  
**Sondes d’intégrité**  
  
SLB inclut des sondes d’intégrité pour valider l’intégrité de l’infrastructure réseau, y compris les éléments suivants.  
  
-   Sonde TCP vers Port  
  
-   Sonde HTTP sur le port et l’URL  
  
Contrairement à une appliance d’équilibrage de charge classique dans laquelle la sonde provient de l’appareil et circule à travers le câble vers l’adresse DIP, la sonde SLB provient de l’hôte où se trouve l’adresse DIP et passe directement de l’agent hôte SLB à l’adresse DIP, en répartissant davantage la travaillez sur les hôtes.  
  
## <a name="software-load-balancing-infrastructure"></a><a name="bkmk_infrastructure"></a>Infrastructure d’équilibrage de la charge logicielle  
Pour déployer Windows Server SLB, vous devez d’abord déployer le contrôleur de réseau dans Windows Server 2016 et une ou plusieurs machines virtuelles MUX MUX.  
  
En outre, vous devez configurer les ordinateurs hôtes Hyper-V avec le commutateur virtuel Hyper-V compatible avec SDN et vérifier que l’agent hôte SLB est en cours d’exécution.  Les routeurs qui desservent les ordinateurs hôtes doivent prendre en charge le routage et le Border Gateway Protocol (BGP) égal à un ECMP et doivent être configurés pour accepter les demandes d’homologation BGP provenant du multiplexeurs SLB.  
  
Vous trouverez ci-dessous une vue d’ensemble de l’infrastructure SLB.  

![Infrastructure d’équilibrage de la charge logicielle](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Les sections suivantes fournissent des informations supplémentaires sur ces éléments de l’infrastructure SLB.  
  
### <a name="scvmm"></a>SCVMM  
Avec System Center 2016, vous pouvez configurer le contrôleur de réseau sur Windows Server 2016, y compris le gestionnaire SLB et le moniteur d’intégrité. Vous pouvez également utiliser System Center pour déployer SLB MUXs et installer des agents hôtes SLB sur des ordinateurs qui exécutent Windows Server 2016 et Hyper-V.  
  
Pour plus d’informations sur System Center 2016, voir [System center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Si vous ne souhaitez pas utiliser System Center 2016, vous pouvez utiliser Windows PowerShell ou une autre application de gestion pour installer et configurer un contrôleur de réseau et une autre infrastructure SLB. Pour plus d’informations, consultez [déployer un contrôleur de réseau à l’aide de Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Contrôleur de réseau  
Le contrôleur de réseau héberge le gestionnaire SLB et effectue les actions suivantes pour SLB.  
  
-   Traite les commandes SLB qui arrivent via l’API Northbound à partir de System Center, de Windows PowerShell ou d’une autre application de gestion de réseau.  
  
-   Calcule la stratégie de distribution pour les hôtes Hyper-V et SLB multiplexeurs.  
  
-   Fournit l’état d’intégrité de l’infrastructure SLB.  
  
### <a name="slb-mux"></a>MUX SLB  
Le MULTIPLEXeur SLB traite le trafic réseau entrant et mappe les adresses IP virtuelles aux adresses IP virtuelles, puis transfère le trafic à l’adresse DIP appropriée. Chaque MULTIPLEXeur utilise également le protocole BGP pour publier les itinéraires VIP sur les routeurs de périphérie. Le protocole BGP Keep Alive notifie multiplexeurs lorsqu’un MULTIPLEXeur échoue, ce qui permet à Active multiplexeurs de redistribuer la charge en cas de défaillance d’un MULTIPLEXeur, ce qui fournit principalement un équilibrage de la charge pour les équilibrages de charge.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Ordinateurs hôtes exécutant Hyper-V  
Vous pouvez utiliser SLB avec les ordinateurs qui exécutent Windows Server 2016 et Hyper-V. Les machines virtuelles sur l’hôte Hyper-V peuvent exécuter n’importe quel système d’exploitation pris en charge par Hyper-V.  
  
### <a name="slb-host-agent"></a>Agent hôte SLB  
Lorsque vous déployez SLB, vous devez utiliser System Center, Windows PowerShell ou une autre application de gestion pour déployer l’agent hôte SLB sur chaque ordinateur hôte Hyper-V. Vous pouvez installer l’agent hôte SLB sur toutes les versions de Windows Server 2016 qui fournissent la prise en charge d’Hyper-V, y compris nano Server.  
  
L’agent hôte SLB écoute les mises à jour de la stratégie SLB à partir du contrôleur de réseau. En outre, l’agent hôte règle les règles de SLB dans les commutateurs virtuels Hyper-V compatibles SDN configurés sur l’ordinateur local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>Commutateur virtuel Hyper-V activé par SDN  
Pour qu’un commutateur virtuel soit compatible avec SLB, vous devez utiliser le gestionnaire de commutateur virtuel Hyper-V ou les commandes Windows PowerShell pour créer le commutateur, puis activer la plateforme de filtrage virtuel (VFP) pour le commutateur virtuel.  
  
Pour plus d’informations sur l’activation de VFP sur des commutateurs virtuels, consultez les commandes Windows PowerShell [VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) et [Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
Le commutateur virtuel Hyper-V activé par SDN effectue les actions suivantes pour SLB.  
  
-   Traite le chemin de données pour SLB.  
  
-   Reçoit le trafic réseau entrant à partir du MULTIPLEXeur.  
  
-   Contourne le MULTIPLEXeur pour le trafic réseau sortant, en l’envoyant au routeur à l’aide de DSR.  
  
-   S’exécute sur les instances nano Server d’Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Routeur BGP activé  
Le routeur BGP effectue les actions suivantes pour SLB.  
  
-   Achemine le trafic entrant vers le MULTIPLEXeur à l’aide de ECMP.  
  
-   Pour le trafic réseau sortant, utilise l’itinéraire fourni par l’hôte.  
  
-   Écoute les mises à jour d’itinéraires pour les adresses IP virtuelles à partir de SLB MUX.  
  
-   Supprime les multiplexeurs SLB de la rotation SLB si Keep Alive échoue.  
  
## <a name="software-load-balancing-features"></a><a name="bkmk_features"></a>Fonctionnalités d’équilibrage de la charge logicielle  
Voici quelques-unes des fonctions et des fonctionnalités de SLB.  
  
**Fonctionnalités principales**  
  
-   SLB fournit des services d’équilibrage de charge de couche 4 pour le trafic TCP/UDP « Nord-Sud » et « est-ouest ».  
  
-   Vous pouvez utiliser SLB sur un réseau basé sur la virtualisation de réseau Hyper-V  
  
-   Vous pouvez utiliser SLB avec un réseau basé sur un réseau local virtuel pour les machines virtuelles DIP connectées à un commutateur virtuel Hyper-V compatible avec SDN.  
  
-   Une instance SLB peut gérer plusieurs locataires  
  
-   SLB et DIP prennent en charge un chemin de retour évolutif et à faible latence, tel qu’implémenté par le retour direct du serveur (DSR)  
  
-   Fonctions SLB quand vous utilisez également Switch Embedded Teaming (SET) ou une virtualisation d’entrée/sortie à racine unique (SR-IOV)  
  
-   SLB comprend la prise en charge IPv4 (Internet Protocol version 4).  
  
-   Pour les scénarios de passerelle de site à site, SLB fournit des fonctionnalités NAT pour permettre à toutes les connexions de site à site d’utiliser une seule adresse IP publique.  
  
-   Vous pouvez installer SLB, y compris l’agent hôte et le MULTIPLEXeur, sur Windows Server 2016, Full, Core et nano install.  
  
**Mise à l’échelle et performances**  
  
-   Prêt pour la mise à l’échelle du Cloud, y compris la capacité de montée en puissance parallèle et la capacité de montée en puissance pour les agents multiplexeurs et hôtes.  
  
-   Un module de contrôleur de réseau SLB Manager actif peut prendre en charge 8 instances MUX  
  
**Haute disponibilité**  
  
-   Vous pouvez déployer SLB sur plus de 2 nœuds dans une configuration active/active  
  
-   Vous pouvez ajouter et supprimer des multiplexeurs dans le pool de MULTIPLEXeurs sans affecter le service SLB. Cela garantit la disponibilité de SLB lorsque   
    les multiplexeurs individuels sont corrigés.  
  
-   Les instances de MULTIPLEXeur individuelles ont une durée de fonctionnement de 99%  
  
-   Les données de contrôle d’intégrité sont disponibles pour les entités de gestion  
  
**Repère**  
  
-   Vous pouvez déployer et configurer SLB avec SCVMM  
  
-   SLB fournit une périphérie unifiée mutualisée en s’intégrant de manière transparente aux appliances Microsoft, telles que la passerelle mutualisée RAS, le pare-feu de centre de connaissances et le réflecteur d’itinéraire.  
  

