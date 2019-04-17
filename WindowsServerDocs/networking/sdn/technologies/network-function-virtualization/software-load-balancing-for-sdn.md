---
title: Logiciel équilibrage de charge (SLB) pour SDN
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur l’équilibrage de charge logicielle pour le logiciel défini de mise en réseau dans Windows Server2016.
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
ms.openlocfilehash: 5e08afddde9c7be8d955a0cfdaf44f0fc31b8155
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="software-load-balancing-slb-for-sdn"></a>Logiciel équilibrage de charge \(SLB\) pour SDN

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur l’équilibrage de charge logicielle pour le logiciel défini de mise en réseau dans Windows Server2016.  

Fournisseurs de services cloud (CSP) et les entreprises qui déploient des logiciels défini de mise en réseau (SDN) dans Windows Server2016 peuvent utiliser l’équilibrage de charge logiciels (SLB) pour répartir les clients et le trafic réseau de client client entre les ressources du réseau virtuel. L’équilibrage du serveur Windows permet à plusieurs serveurs pour héberger la même charge de travail, fournissant une évolutivité et haute disponibilité.
  
WindowsServerSLB inclut les fonctionnalités suivantes.  
  
-   Couche 4 (N4) charge équilibrage services pour «Nord sud» et le trafic «Est-ouest» TCP/UDP.  
  
-   Public et internes équilibrage de la charge du trafic réseau.  
  
-   Prend en charge les adresses IP dynamiques (DIP) sur les réseaux locaux virtuels (VLAN) et sur les réseaux virtuels que vous créez à l’aide de la virtualisation de réseau Hyper-V.  
  
-   Prise en charge de la sonde d’intégrité.  
  
-   Prêt à l’échelle du cloud, y compris la fonctionnalité de montée en puissance parallèle et évoluer fonctionnalité pour multiplexeur et les Agents de l’ordinateur hôte.  
  
Pour plus d’informations, voir [fonctionnalité d’équilibrage de charge logicielle](#bkmk_features) dans cette rubrique.  
  
> [!NOTE]  
> Architecture mutualisée pour les réseaux locaux virtuels n’est pas pris en charge par le contrôleur de réseau, toutefois vous pouvez utiliser des réseaux locaux virtuels avec les différentes pour le fournisseur de services gérés des charges de travail, telles que l’infrastructure de centre de données et des serveurs Web de haute densité.  
  
À l’aide de WindowsServerSLB, vous pouvez faire évoluer votre fonctions à l’aide de machines virtuelles SLB sur les mêmes serveurs de calcul Hyper-V que vous utilisez pour vos autres charges de travail de machine virtuelle d’équilibrage de charge. Pour cette raison, les SLB prend en charge la création rapide et la suppression de l’équilibrage de charge les points de terminaison qui est requise pour les opérations de fournisseur de services cryptographiques. En outre, WindowsServerSLB prend en charge des dizaines de gigaoctets par cluster, fournit un modèle d’approvisionnement simple et est facile de montée en puissance parallèle, puis dans.  
  
**Comment fonctionne SLB**  
  
SLB fonctionne en mappant des adresses IP virtuelles (VIP) à des adresses IP dynamiques (DIP) qui font partie d’un ensemble de services cloud de ressources dans le centre de données.  
  
Adresses IP est des adresses IP uniques qui fournissent un accès public à un pool de charge équilibrée d’ordinateurs virtuels. Par exemple, adresses IP est des adresses IP qui sont exposés sur Internet afin que les clients et les clients de client peuvent se connecter aux ressources du locataire dans le centre de données cloud.  
  
DIP est les adresses IP du membre machines virtuelles d’un pool d’équilibrage de charge derrière l’adresse IP virtuelle. DIP est affectés au sein de l’infrastructure de cloud pour les ressources du locataire.  
  
VIP se trouvent dans le SLB multiplexeur MUX ().  Le multiplexage se compose d’un ou plusieurs ordinateurs virtuels (VM).  Contrôleur de réseau offre chaque MUX avec chaque adresse IP virtuelle et chaque MUX à son tour utilise protocole BGP (Border Gateway) pour publier chaque adresse IP virtuelle aux routeurs sur le réseau physique comme un /32 itinéraire.  Protocole BGP permet les routeurs du réseau physique pour:  
  
-   Découvrez qu’une adresse IP virtuelle est disponible sur chaque MUX, même si les MUXes se trouvent sur des sous-réseaux différents dans un réseau de couche 3.  
  
-   Répartir la charge pour chaque adresse IP virtuelle sur tous les MUXes disponibles à l’aide de routage égale coût Multi-Path (ECMP).  
  
-   Détecter un échec MUX ou la suppression et arrêter l’envoi du trafic à l’échec MUX automatiquement.  
  
-   Répartir la charge de l’échec ou supprimé MUX sur le MUXes sain.  
  
Lorsque le trafic publique arrive à partir d’Internet, le MUX SLB examine le trafic, qui contient l’adresse IP virtuelle en tant que destination et mappe réécrit le trafic afin qu’il arriverez à une adresse DIP individuelle. Pour le trafic réseau entrant, cette opération est effectuée dans un processus en deux étapes qui est fractionné entre les machines virtuelles MUX (VM) et l’ordinateur hôte Hyper-V sur lequel se trouve la destination DIP:  
  
-   Équilibrage de la charge - les utilisations MUX l’adresse IP virtuelle pour sélectionner une unité DIP, encapsule les paquets et transfère le trafic vers l’hôte Hyper-V sur lequel se trouve l’adresse IP directe.  
  
-   Traduction d’adresses réseau (NAT) - l’hôte Hyper-V supprime encapsulation de paquet se traduit par l’adresse IP virtuelle pour un DIP, reconfigure les ports et transmet le paquet à l’ordinateur virtuel DIP.  
  
Le multiplexage connaît le mappage VIP à la DIP approprié en raison des stratégies que vous définissez à l’aide du contrôleur de réseau d’équilibrage de charge. Ces règles incluent le protocole, Port frontal, port principal et l’algorithme de distribution (2, 3 ou 5tuples).  
  
Lorsque client répondent de machines virtuelles et les envoyer sortant le trafic réseau vers l’Internet ou les emplacements des clients distants, car le NAT est effectué par l’hôte Hyper-V, le trafic contourne le MUX et accède directement au routeur edge à partir de l’ordinateur hôte Hyper-V. Ce processus de contournement MUX est appelé Direct serveur renvoyer DSR ().  
  
Et une fois le flux de trafic réseau initiale établi, le trafic réseau entrant contourne la MUX SLB complètement.  
  
Dans l’illustration suivante, un ordinateur client effectue une requête DNS pour l’adresse IP d’un société site Sharepoint - dans ce cas, une société fictive nommée Contoso. Le processus suivant se produit.  
  
-   Le serveur DNS retourne l’adresse IP virtuelle 107.105.47.60 au client.  
  
-   Le client envoie une requête HTTP à l’adresse IP virtuelle.  
  
-   Le réseau physique a plusieurs chemins d’accès disponibles pour atteindre l’adresse IP virtuelle situé sur n’importe quel MUX.  Chaque routeur tout au long du processus utilise ECMP pour choisir le segment du chemin d’accès suivant jusqu'à ce que la demande arrive à un MUX.  
  
-   Le multiplexage qui reçoit la demande vérifie configuré des stratégies et constate qu’il existe deux DIP disponibles, 10.10.10.5 et 10.10.20.5, sur un réseau virtuel pour gérer la demande à l’adresse IP virtuelle 107.105.47.60  
  
-   Le multiplexage sélectionne DIP 10.10.10.5 et encapsule les paquets à l’aide de VXLAN, et il peut envoyer à l’hôte contenant l’adresse IP directe à l’aide d’ordinateurs hôtes adresse réseau physique.  
  
-   L’ordinateur hôte reçoit le paquet encapsulé et inspecte.  Il supprime l’encapsulation et réécrit le paquet afin que la destination est désormais l’adresse IP directe 10.10.10.5 au lieu de l’adresse IP virtuelle et envoie le trafic à l’ordinateur virtuel DIP.  
  
-   La demande a atteint désormais le site Contoso Sharepoint dans le serveur 2 de la batterie de serveurs. Le serveur génère une réponse et l’envoie au client, à l’aide de sa propre adresse IP comme source.  
  
-   L’ordinateur hôte intercepte les paquets sortants dans le commutateur virtuel qui se souvient que le client, la destination maintenant, fait la demande d’origine à l’adresse IP virtuelle.  L’ordinateur hôte réécrit la source du paquet à l’adresse IP virtuelle afin que le client ne voit pas l’adresse DIP.  
  
-   L’hôte envoie le paquet directement à la passerelle par défaut pour le réseau physique qui utilise sa table de routage standard pour transférer le paquet une session sur le client, ce qui finalement reçoit la réponse.  
  
![Processus d’équilibrage de charge logicielle](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_process.jpg)  
  
**La charge du centre de données interne trafic**  
  
Lorsque la charge du réseau trafic interne du centre de données, par exemple entre les ressources client qui sont exécutent sur différents serveurs et qui sont membres du même réseau virtuel, le commutateur virtuel Hyper-V sur lesquels les ordinateurs virtuels sont connectés effectue NAT.  
  
Avec l’équilibrage de charge le trafic interne, la première demande est envoyée à et traitée par le multiplexage, qui sélectionne le DIP approprié et achemine le trafic vers l’adresse IP directe. À partir de ce point, le flux de trafic établie contourne la MUX et passe directement à partir de la machine virtuelle à l’ordinateur virtuel.  
  
**Sondes de contrôle d’intégrité**  
  
SLB inclut les sondes d’intégrité pour valider l’intégrité de l’infrastructure réseau, notamment les suivantes.  
  
-   Sonde port TCP  
  
-   Sonde HTTP pour le port et l’URL  
  
Contrairement à un dispositif d’équilibrage de charge traditionnel où la sonde provient de l’application et se rend la transmission à l’adresse IP directe, la sonde SLB provient de l’ordinateur hôte sur lequel l’adresse IP directe se trouve et accède directement à partir de l’agent hôte SLB à l’adresse IP directe, davantage en répartissant le travail entre les hôtes.  
  
## <a name="bkmk_infrastructure"></a>Infrastructure de l’équilibrage de charge logicielle  
Pour déployer WindowsServerSLB, vous devez tout d’abord déployer le contrôleur de réseau dans Windows Server2016 et un ou plusieurs ordinateurs virtuels SLB MUX.  
  
En outre, vous devez configurer les ordinateurs hôtes Hyper-V avec le commutateur virtuel Hyper-V activé-SDN et assurez-vous que l’Agent hôte SLB est en cours d’exécution.  Les routeurs qui desservent les hôtes doivent prendre en charge de routage de même coût MPIO (ECMP) et le protocole BGP (Border Gateway) et doivent être configurés pour accepter les demandes d’homologation BGP à partir de la MUXes SLB.  
  
Voici une vue d’ensemble de l’infrastructure SLB.  

![Infrastructure de l’équilibrage de charge logicielle](../../../media/Software-Load-Balancing--SLB--for-SDN/slb_overview1.png)  
  
Les sections suivantes fournissent plus d’informations sur ces éléments de l’infrastructure SLB.  
  
### <a name="scvmm"></a>SCVMM  
Avec SystemCenter2016, vous pouvez configurer le contrôleur de réseau sur Windows Server2016, y compris les SLB Manager et le moniteur d’intégrité. Vous pouvez également utiliser SystemCenter pour déployer les différentes MUXs et pour installer des Agents d’hôte SLB sur les ordinateurs qui exécutent Windows Server2016 et Hyper-V.  
  
Pour plus d’informations sur SystemCenter2016, voir [SystemCenter2016](https://www.microsoft.com/en-us/server-cloud/products/system-center-2016/).  
  
> [!NOTE]  
> Si vous ne souhaitez pas utiliser SystemCenter2016, vous pouvez utiliser Windows PowerShell ou une autre application de gestion pour installer et configurer le contrôleur de réseau et toute autre infrastructure SLB. Pour plus d’informations, voir [déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../../sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="network-controller"></a>Contrôleur de réseau  
Contrôleur de réseau héberge le gestionnaire SLB et effectue les actions suivantes pour les différentes.  
  
-   Traite les différentes commandes via l’API Northbound à partir de SystemCenter, Windows PowerShell ou une autre application de gestion de réseau.  
  
-   Calcule la stratégie pour la distribution aux ordinateurs hôtes Hyper-V et les différentes MUXes.  
  
-   Fournit l’état d’intégrité de l’infrastructure SLB.  
  
### <a name="slb-mux"></a>SLB MUX  
Le MUX SLB traite le trafic réseau entrant et mappe VIP vers DIP, puis transfère le trafic vers DIP approprié. Chaque MUX utilise également BGP pour publier les itinéraires d’adresses IP virtuelles vers les routeurs de bord. Protocole BGP persistantes informe MUXes un MUX échoue, ce qui permet de MUXes active redistribuer la charge en cas de défaillance MUX - fournissant essentiellement d’équilibrage de charge pour les équilibreurs de charge.  
  
### <a name="hosts-that-are-running-hyper-v"></a>Ordinateurs hôtes exécutant Hyper-V  
Vous pouvez utiliser les différentes avec les ordinateurs qui exécutent Windows Server2016 et Hyper-V. Les ordinateurs virtuels sur l’ordinateur hôte Hyper-V peuvent exécuter n’importe quel système d’exploitation est pris en charge par Hyper-V.  
  
### <a name="slb-host-agent"></a>Agent d’ordinateur hôte SLB  
Lorsque vous déployez les différentes, vous devez utiliser SystemCenter, Windows PowerShell ou une autre application de gestion pour déployer l’Agent d’ordinateur hôte SLB sur tous les ordinateurs hôtes Hyper-V. Vous pouvez installer l’Agent hôte SLB sur toutes les versions de Windows Server2016 qui prennent en charge Hyper-V, y compris Nano Server.  
  
L’Agent hôte SLB écoute les mises à jour de la stratégie SLB à partir du contrôleur de réseau. En outre, l’agent hôte programmes des règles pour les différentes dans les commutateurs virtuels Hyper-V activé-SDN qui sont configurés sur l’ordinateur local.  
  
### <a name="sdn-enabled-hyper-v-virtual-switch"></a>SDN activé le commutateur virtuel Hyper-V  
Pour un commutateur virtuel être compatible avec les différentes, vous devez utiliser des commandes de gestionnaire de commutateur virtuel Hyper-V ou Windows PowerShell pour créer le commutateur et, vous devez activer la plateforme de filtrage virtuel (VFP) pour le commutateur virtuel.  
  
Pour plus d’informations sur l’activation de VFP sur les commutateurs virtuels, voir les commandes Windows PowerShell [Get-VMSystemSwitchExtension](https://technet.microsoft.com/en-us/library/hh848603.aspx) et [Enable-VMSwitchExtension](https://technet.microsoft.com/en-us/library/hh848541.aspx?f=255&MSPPError=-2147217396).  
  
Le SDN activé le commutateur virtuel Hyper-V effectue les actions suivantes pour les différentes.  
  
-   Traite le chemin d’accès de données pour les différentes.  
  
-   Reçoit le trafic réseau entrant à partir de la MUX.  
  
-   Ignore le multiplexage pour le trafic réseau sortant, envoyer vers le routeur à l’aide de DSR.  
  
-   S’exécute sur des instances de Nano Server de Hyper-V.  
  
### <a name="bgp-enabled-router"></a>Protocole BGP activé routeur  
Le routeur BGP effectue les actions suivantes pour les différentes.  
  
-   Itinéraires le trafic entrant pour le multiplexage à l’aide de routage ECMP.  
  
-   Pour le trafic réseau sortant, utilise l’itinéraire fourni par l’hôte.  
  
-   Écoute les mises à jour de l’itinéraire pour les adresses IP à partir de SLB MUX.  
  
-   Supprime MUXes SLB la rotation SLB en cas d’échec persistantes.  
  
## <a name="bkmk_features"></a>Fonctionnalité d’équilibrage de charge logicielle  
Voici certaines des fonctionnalités et des fonctionnalités de SLB.  
  
**Fonctionnalités principales**  
  
-   SLB fournit des services pour «Nord sud» et le trafic «Est-ouest» TCP/UDP d’équilibrage de charge de couche 4  
  
-   Vous pouvez utiliser les différentes sur un réseau basé sur la virtualisation de réseau Hyper-V  
  
-   Vous pouvez utiliser les différentes avec un réseau basé sur un réseau local virtuel pour les ordinateurs virtuels DIP connecté à un SDN activé Hyper-V Virtual Switch.  
  
-   Une seule instance SLB peut gérer plusieurs clients  
  
-   Les différentes et DIP prennent en charge un chemin de retour évolutif et à faible latence, tel qu’implémenté renvoyer par serveur Direct (DSR)  
  
-   Fonctions SLB lorsque vous utilisez également commutateur SET (Embedded Teaming) ou la virtualisation d’entrée/sortie racine unique (SR-IOV)  
  
-   SLB inclut Internet Protocol version4 (IPv4) prennent en charge  
  
-   Pour les scénarios de passerelle de site, SLB fournit la fonctionnalité NAT pour activer toutes les connexions de site à utiliser une adresse IP publique unique  
  
-   Vous pouvez installer les différentes, y compris l’Agent hôte et le multiplexage, sur Windows Server2016, complète, Core et installation Nano.  
  
**Performances et évolutivité**  
  
-   Prêt à l’échelle du cloud, y compris la fonctionnalité de montée en puissance parallèle et évoluer fonctionnalité pour MUXes et les Agents de l’ordinateur hôte.  
  
-   Un module de contrôleur de réseau SLB Manager actif peut prendre en charge des instances MUX 8  
  
**Haute disponibilité**  
  
-   Vous pouvez déployer les différentes sur plus de 2nœuds dans une configuration actif/actif  
  
-   MUXes peuvent être ajoutés et supprimés à partir du pool MUX sans affecter le service SLB. Cela maintient la disponibilité SLB lorsque   
    MUXes individuels sont en cours corrigés.  
  
-   Les instances individuelles MUX ont un temps d’activité de 99%  
  
-   Analyse des données de l’intégrité est disponible pour les entités de gestion  
  
**Alignement**  
  
-   Vous pouvez déployer et configurer SLB avec SCVMM  
  
-   SLB fournit une architecture mutualisée edge unifiée en intégrant de façon transparente avec les appareils Microsoft tels que la passerelle mutualisée, pare-feu de centre de données et réflecteur d’itinéraire.  
  

