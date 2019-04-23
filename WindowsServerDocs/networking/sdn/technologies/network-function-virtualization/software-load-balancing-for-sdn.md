---
title: Équilibrage de la charge logicielle pour SDN
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur l’équilibrage de charge logicielle pour Sdn dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97abf182-4725-4026-801c-122db96964ed
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 26fb4aa21e80618c4c63bd9edbf8731bf886db62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853760"
---
# <a name="software-load-balancing-slb-for-sdn"></a>L’équilibrage de charge logiciel \(SLB\) pour SDN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’équilibrage de charge logicielle pour Sdn dans Windows Server 2016.  

Fournisseurs de services cloud (CSP) et les entreprises qui déploient la mise en réseau SDN (Software Defined) dans Windows Server 2016 peuvent utiliser l’équilibrage de charge logiciel (SLB) pour répartir uniformément le client et le trafic réseau de client client entre les ressources du réseau virtuel. La fonctionnalité d’équilibrage de charge de Windows Server vous permet d’activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.
  
SLB Windows Server inclut les fonctionnalités suivantes.  
  
-   Pour « Nord-sud » et le trafic TCP/UDP de « Est-ouest » les services d’équilibrage de charge de couche 4 (L4).  
  
-   Publics et internes équilibrage de charge du trafic réseau.  
  
-   Prend en charge les adresses IP dynamiques (DIP) sur les réseaux locaux virtuels (VLAN) et sur les réseaux virtuels que vous créez à l’aide de la virtualisation de réseau Hyper-V.  
  
-   Prise en charge de la sonde d’intégrité.  
  
-   Prêt pour l’échelle du cloud, y compris la capacité de montée en puissance et augmenter la capacité de multiplexeurs et des Agents de l’hôte.  
  
Pour plus d’informations, consultez [fonctionnalités d’équilibrage de charge logiciel](#bkmk_features) dans cette rubrique.  
  
> [!NOTE]  
> Architecture mutualisée pour réseaux locaux virtuels n’est pas pris en charge par le contrôleur de réseau, toutefois vous pouvez utiliser des réseaux locaux virtuels avec SLB pour fournisseur de services gérés des charges de travail, telles que l’infrastructure de centre de données et les serveurs Web de haute densité.  
  
À l’aide de Windows Server SLB, vous pouvez faire évoluer vos capacités à l’aide de machines virtuelles SLB sur les mêmes serveurs de calcul Hyper-V que vous utilisez pour vos autres charges de travail de machine virtuelle d’équilibrage de charge. Pour cette raison, SLB prend en charge la création rapide et la suppression de points de terminaison d’équilibrage de charge qui est requise pour les opérations du fournisseur de services cryptographiques. En outre, SLB Windows Server prend en charge des dizaines de gigaoctets par cluster fournit un modèle d’approvisionnement simple et est facile de mise à l’échelle des instances.  
  
**Fonctionne de SLB**  
  
SLB fonctionne en mappant les adresses IP virtuelles (VIP) pour les adresses IP dynamiques (adresses IP dynamiques) qui font partie d’un ensemble de service cloud de ressources dans le centre de données.  
  
Adresses IP virtuelles sont des adresses IP uniques qui fournissent l’accès public à un pool de charge équilibrée des machines virtuelles. Par exemple, les adresses IP virtuelles sont des adresses IP qui sont exposés sur Internet afin que les clients et les clients locataires peuvent se connecter aux ressources du client dans le centre de données cloud.  
  
Adresses IP dynamiques sont les adresses IP du membre d’un pool d’équilibrage de charge derrière l’adresse IP virtuelle des machines virtuelles. Adresses IP dynamiques sont affectées au sein de l’infrastructure de cloud pour les ressources du client.  
  
Adresses IP virtuelles sont situés dans le SLB multiplexeur (MUX).  Le MUX se compose d’un ou plusieurs machines virtuelles (VM).  Contrôleur de réseau offre chaque MUX avec chaque adresse IP virtuelle et chaque MUX à son tour utilise protocole BGP (Border Gateway) pour publier chaque adresse IP virtuelle aux routeurs sur le réseau physique comme un /32 itinéraire.  BGP permet les routeurs du réseau physique pour :  
  
-   Découvrez qu’une adresse IP virtuelle est disponible sur chaque MUX, même si les multiplexeurs se trouvent sur des sous-réseaux différents dans un réseau de couche 3.  
  
-   Répartir la charge pour chaque adresse IP virtuelle sur tous les multiplexeurs disponibles à l’aide du routage égal Cost Multi-Path ECMP ().  
  
-   Automatiquement détecter un dysfonctionnement MUX ou le retrait et arrêter l’envoi du trafic vers le MUX ayant échoué.  
  
-   Répartir la charge à partir de la MUX ayant échoué ou supprimé sur les multiplexeurs d’intégrité.  
  
Lorsque le trafic public arrive à partir d’Internet, le SLB MUX examine le trafic, qui contient l’adresse IP virtuelle en tant que destination, puis mappe et réécrit le trafic afin qu’il vous parviendra à une adresse DIP individuelle. Pour le trafic réseau entrant, cette transaction est exécutée dans un processus en deux étapes qui est partagé entre les machines virtuelles MUX (machines virtuelles) et l’hôte Hyper-V où se trouve la DIP de destination :  
  
-   Équilibrage de charge - les utilisations MUX l’adresse IP virtuelle pour sélectionner une adresse DIP, encapsule le paquet et transfère le trafic à l’hôte Hyper-V sur lequel se trouve l’adresse IP dédiée.  
  
-   Traduction d’adresses réseau (NAT) - l’hôte Hyper-V supprime l’encapsulation de paquet traduit l’adresse IP virtuelle vers une adresse DIP, remappe les ports et transfère le paquet à la machine virtuelle DIP.  
  
Le MUX sait comment mapper les adresses IP virtuelles pour les adresses IP dynamiques correctes en raison des stratégies que vous définissez à l’aide du contrôleur de réseau d’équilibrage de charge. Ces règles incluent le protocole, Port de front-end, port Back-end et l’algorithme de distribution (2, 3 ou 5 tuples).  
  
Lorsque locataire répondent de machines virtuelles et envoi sortant le trafic réseau à l’Internet ou les emplacements des clients distants, car la NAT est effectuée par l’hôte Hyper-V, le trafic contourne le MUX et va directement vers le routeur de périphérie de l’hôte Hyper-V. Ce processus de contournement MUX est appelé retour serveur Direct (DSR).  
  
Et une fois le flux de trafic réseau initiale établi, le trafic réseau entrant contourne le SLB MUX complètement.  
  
Dans l’illustration suivante, un ordinateur client effectue une requête DNS pour l’adresse IP d’un entreprise SharePoint site - dans ce cas, une société fictive nommée Contoso. Le processus suivant se produit.  
  
-   Le serveur DNS renvoie l’adresse IP virtuelle 107.105.47.60 au client.  
  
-   Le client envoie une requête HTTP à l’adresse IP virtuelle.  
  
-   Le réseau physique a plusieurs chemins d’accès disponibles pour atteindre l’adresse IP virtuelle situé sur n’importe quel MUX.  Chaque routeur présent utilise ECMP pour choisir le segment du chemin d’accès suivant jusqu'à ce que la demande arrive à un MUX.  
  
-   Vérifie les stratégies configurées MUX qui reçoit la demande et voit qu’il n’y a deux adresses IP dynamiques disponibles, 10.10.10.5 et 10.10.20.5, sur un réseau virtuel pour traiter la demande de l’adresse IP virtuelle 107.105.47.60  
  
-   Le MUX sélectionne l’adresse DIP 10.10.10.5 et encapsule les paquets à l’aide de VXLAN, et il peut l’envoyer à l’hôte contenant l’adresse IP dédiée à l’aide d’hôtes adresse physique du réseau.  
  
-   L’hôte reçoit le paquet encapsulé et elle inspecte.  Il supprime l’encapsulation et réécrit le paquet afin que la destination est désormais l’adresse DIP 10.10.10.5 au lieu de l’adresse IP virtuelle et envoie le trafic vers la machine virtuelle DIP.  
  
-   La demande a atteint maintenant le site SharePoint de Contoso dans Server 2 de batterie de serveurs. Le serveur génère une réponse et l’envoie au client, à l’aide de sa propre adresse IP comme source.  
  
-   L’hôte intercepte le paquet sortant dans le commutateur virtuel qui se souvient que le client, maintenant à la destination, qui a fait la demande d’origine à l’adresse IP virtuelle.  L’hôte réécrit la source du paquet à l’adresse IP virtuelle afin que le client ne voit pas l’adresse DIP.  
  
-   L’hôte envoie le paquet directement à la passerelle par défaut pour le réseau physique qui utilise sa table de routage standard pour transférer le paquet d’une session sur le client qui finalement reçoit la réponse.  
  
![Processus d’équilibrage de charge logiciel](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**Équilibrage du trafic interne de centre de données**  
  
Lorsque la charge du réseau trafic interne au centre de données, telles qu’entre les ressources de locataire qui sont exécutent sur des serveurs différents et sont membres du même réseau virtuel, le commutateur virtuel Hyper-V auquel les machines virtuelles sont connectées effectue NAT.  
  
Avec l’équilibrage de charge du trafic interne, la première demande est envoyée à et traitée par le multiplexeur, qui sélectionne l’adresse DIP approprié et achemine le trafic vers l’adresse IP dédiée. À partir de là, le flux de trafic établies contourne le MUX et accède directement à partir de la machine virtuelle à la machine virtuelle.  
  
**Sondes d’intégrité**  
  
SLB inclut des sondes d’intégrité pour valider l’intégrité de l’infrastructure réseau, notamment les suivantes.  
  
-   Sonde TCP au port  
  
-   Sonde HTTP sur port et l’URL  
  
Contrairement à une appliance à équilibrage de charge traditionnel où la sonde provenance de l’appliance et transite sur le câble à l’adresse IP dédiée, la sonde SLB provenance de l’hôte où l’adresse IP dédiée se trouve et accède directement à partir de l’agent hôte SLB à l’adresse DIP, distribution davantage le les données sur les ordinateurs hôtes.  
  
## <a name="bkmk_infrastructure"></a>Infrastructure d’équilibrage de charge logicielle  
Pour déployer le SLB Windows Server, vous devez tout d’abord déployer le contrôleur de réseau dans Windows Server 2016 et un ou plusieurs machines virtuelles SLB MUX.  
  
En outre, vous devez configurer des hôtes Hyper-V avec le commutateur virtuel Hyper-V activé de SDN et vous assurer que l’Agent hôte SLB est en cours d’exécution.  Les routeurs qui desservent les hôtes doivent prendre en charge de routage de même coût Multipath i/o (ECMP) et le protocole BGP (Border Gateway) et doivent être configurés pour accepter les demandes d’homologation BGP à partir de la MUX SLB.  
  
Voici une vue d’ensemble de l’infrastructure SLB.  

![Infrastructure d’équilibrage de charge logicielle](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Les sections suivantes fournissent plus d’informations sur ces éléments de l’infrastructure SLB.  
  
### <a name="scvmm"></a>SCVMM  
Avec System Center 2016, vous pouvez configurer le contrôleur de réseau sur Windows Server 2016, y compris le gestionnaire SLB et le moniteur d’intégrité. Vous pouvez également utiliser System Center pour déployer SLB MUXs et installer des Agents d’hôte SLB sur les ordinateurs qui exécutent Windows Server 2016 et Hyper-V.  
  
Pour plus d’informations sur System Center 2016, consultez [System Center 2016](https://www.microsoft.com/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Si vous ne souhaitez pas utiliser System Center 2016, vous pouvez utiliser Windows PowerShell ou une autre application de gestion pour installer et configurer le contrôleur de réseau et d’autres infrastructures SLB. Pour plus d’informations, consultez [déployer de contrôleur de réseau à l’aide de Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Contrôleur de réseau  
Contrôleur de réseau héberge le gestionnaire SLB et effectue les actions suivantes pour SLB.  
  
-   Traite les commandes SLB qui proviennent via l’API Northbound de System Center, Windows PowerShell ou une autre application de gestion de réseau.  
  
-   Calcule la stratégie pour la distribution aux hôtes Hyper-V et MUX SLB.  
  
-   Fournit l’état d’intégrité de l’infrastructure SLB.  
  
### <a name="slb-mux"></a>SLB MUX  
Le SLB MUX traite le trafic réseau entrant et mappe les adresses IP virtuelles aux adresses IP dynamiques, puis transfère le trafic vers l’adresse DIP approprié. Chaque MUX utilise également le protocole BGP pour publier des itinéraires de l’adresse IP virtuelle sur les routeurs de périphérie. BGP Keep Alive notifie les multiplexeurs d’un MUX échoue, ce qui permet les multiplexeurs d’active redistribuer la charge en cas de défaillance MUX - fournissant essentiellement l’équilibrage de charge pour les équilibreurs de charge.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Ordinateurs hôtes qui exécutent Hyper-V  
Vous pouvez utiliser SLB avec les ordinateurs qui exécutent Windows Server 2016 et Hyper-V. Les machines virtuelles sur l’hôte Hyper-V peuvent exécuter n’importe quel système d’exploitation pris en charge par Hyper-V.  
  
### <a name="slb-host-agent"></a>Agent hôte SLB  
Lorsque vous déployez SLB, vous devez utiliser System Center, Windows PowerShell ou une autre application de gestion pour déployer l’Agent hôte de SLB sur chaque ordinateur hôte de Hyper-V. Vous pouvez installer l’Agent hôte SLB sur toutes les versions de Windows Server 2016 qui prennent en charge Hyper-V, y compris Nano Server.  
  
L’Agent hôte SLB écoute les mises à jour de la stratégie SLB à partir du contrôleur de réseau. En outre, l’agent hôte programmes des règles pour SLB dans les commutateurs virtuels Hyper-V activé de SDN qui sont configurés sur l’ordinateur local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN activé le commutateur virtuel Hyper-V  
Pour un commutateur virtuel être compatible avec SLB, vous devez utiliser des commandes de gestionnaire de commutateur virtuel Hyper-V ou Windows PowerShell pour créer le commutateur, et vous devez ensuite activer la plateforme de filtrage virtuel (VFP) pour le commutateur virtuel.  
  
Pour plus d’informations sur l’activation de VFP sur les commutateurs virtuels, consultez les commandes Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/library/hh848603.aspx) et [Enable-VMSwitchExtension](https://technet.microsoft.com/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
Le SDN activé le commutateur virtuel Hyper-V effectue les actions suivantes pour SLB.  
  
-   Traite le chemin d’accès de données pour SLB.  
  
-   Reçoit le trafic réseau entrant à partir de la MUX.  
  
-   Ignore le multiplexeur pour le trafic réseau sortant, envoyer vers le routeur à l’aide de DSR.  
  
-   S’exécute sur des instances de Nano Server d’Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Le protocole BGP activé routeur  
Le routeur BGP effectue les actions suivantes pour SLB.  
  
-   Itinéraires du trafic entrant pour le multiplexeur à l’aide du routage ECMP.  
  
-   Pour le trafic réseau sortant, utilise l’itinéraire fourni par l’hôte.  
  
-   Écoute des mises à jour de l’itinéraire pour les adresses IP virtuelles à partir de SLB MUX.  
  
-   Supprime MUX SLB à partir de la rotation de SLB si Keep Alive échoue.  
  
## <a name="bkmk_features"></a>Fonctionnalités d’équilibrage de charge logiciel  
Voici quelques-unes des fonctionnalités et capacités de SLB.  
  
**Fonctionnalités principales**  
  
-   SLB fournit des services pour « Nord-sud » et le trafic TCP/UDP de « Est-ouest » d’équilibrage de charge de couche 4  
  
-   Vous pouvez utiliser SLB sur un réseau basé sur la virtualisation de réseau Hyper-V  
  
-   Vous pouvez utiliser SLB avec un réseau basé sur le réseau local virtuel pour les machines virtuelles DIP connectée à un SDN activé Hyper-V commutateur virtuel.  
  
-   Une seule instance SLB peut gérer plusieurs locataires  
  
-   SLB et DIP prennent en charge un chemin de retour évolutif et à faible latence, tel qu’implémenté retourner par serveur Direct (DSR)  
  
-   Fonctions SLB lorsque vous utilisez également l’association de cartes SET (Switch Embedded) ou la virtualisation d’entrée/sortie une racine unique (SR-IOV)  
  
-   SLB inclut Internet Protocol version 4 (IPv4) prennent en charge  
  
-   Pour les scénarios de passerelle de site à site, SLB propose une fonctionnalité NAT pour activer toutes les connexions de site à site à utiliser une adresse IP publique unique  
  
-   Vous pouvez installer SLB, y compris l’Agent hôte et le multiplexeur, sur Windows Server 2016, complète, Core et Nano Install.  
  
**Mise à l’échelle et performances**  
  
-   Prêt pour l’échelle du cloud, y compris la capacité de montée en puissance et augmenter la capacité pour les multiplexeurs d’et les Agents de l’hôte.  
  
-   Un module de contrôleur de réseau de gestionnaire SLB actif peut prendre en charge les 8 instances MUX  
  
**Haute disponibilité**  
  
-   Vous pouvez déployer SLB à plus de 2 nœuds dans une configuration actif/actif  
  
-   Les multiplexeurs de peuvent être ajoutées et supprimées à partir du pool MUX sans affecter le service SLB. Cela maintient la disponibilité SLB lorsque   
    multiplexeurs individuels sont en cours corrigées.  
  
-   Les instances individuelles MUX ont une durée de fonctionnement de 99 %  
  
-   Analyse des données d’intégrité est disponible pour les entités de gestion  
  
**Alignement**  
  
-   Vous pouvez déployer et configurer SLB avec SCVMM  
  
-   SLB fournit un bord unifié mutualisé en étroite collaboration avec les appliances de Microsoft telles que la passerelle mutualisée de RAS, le pare-feu de centre de données et le réflecteur d’itinéraire.  
  

