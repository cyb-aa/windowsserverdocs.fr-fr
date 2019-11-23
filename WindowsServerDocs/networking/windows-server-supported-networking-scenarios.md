---
title: Scénarios de mise en réseau pris en charge par Windows Server
description: Cette rubrique fournit des informations sur les nouveaux scénarios de mise en réseau pris en charge dans Windows Server 2016 et versions ultérieures.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f338ddf0a7d3a4fe41277ddbf49b0c3db34ae11b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395697"
---
# <a name="windows-server-supported-networking-scenarios"></a>Scénarios de mise en réseau pris en charge par Windows Server

>S’applique à :\)de canal semi-annuel Windows Server \(, Windows Server 2016

Cette rubrique fournit des informations sur les scénarios pris en charge et non pris en charge que vous pouvez ou ne pouvez pas effectuer avec cette version de Windows Server 2016.  
>[!IMPORTANT]
>Pour tous les scénarios de production, utilisez les derniers pilotes matériels signés de votre fabricant d’équipements d’origine \(OEM\) ou fournisseur de matériel indépendant \(\)IHV.
  
## <a name="bkmk_supp"></a>Scénarios de mise en réseau pris en charge

Cette section contient des informations sur les scénarios de mise en réseau pris en charge pour Windows Server 2016 et comprend les catégories de scénarios suivantes.  
  
-   [Scénarios de mise en réseau à définition logicielle (SDN)](#bkmk_sdn)  
  
-   [Scénarios de plateforme réseau](#bkmk_netp)  
  
-   [Scénarios de serveur DNS](#bkmk_dns)  
  
-   [Scénarios IPAM avec DHCP et DNS](#bkmk_ipam)  
  
-   [Scénarios d’association de cartes réseau](#bkmk_nicteam)

- [Basculer l’Association incorporée \(définir des scénarios de\)](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Scénarios de mise en réseau à définition logicielle (SDN)
 
Vous pouvez utiliser la documentation suivante pour déployer des scénarios SDN avec Windows Server 2016.  
  
  
-   [Déployer une infrastructure réseau définie par logiciel à l’aide de scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Pour plus d’informations, voir [Software Defined &#40;Networking SDN&#41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Scénarios de contrôleur de réseau

Les scénarios de contrôleur de réseau vous permettent d’effectuer les opérations suivantes :  
  
-   Déployez et gérez une instance à plusieurs nœuds du contrôleur de réseau. Pour plus d’informations, consultez [déployer un contrôleur de réseau à l’aide de Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Utilisez le contrôleur de réseau pour définir par programmation la stratégie réseau à l’aide de l’API REST Northbound.  
  
-   Utilisez le contrôleur de réseau pour créer et gérer des réseaux virtuels avec la virtualisation de réseau Hyper-V, à l’aide de NVGRE ou de l’encapsulation VXLAN.  
  
Pour plus d’informations, voir [Contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Scénarios de virtualisation de fonction réseau (NFV)  
Les scénarios NFV vous permettent d’effectuer les opérations suivantes :  
  
-   Déployez et utilisez un équilibreur de charge logiciel pour distribuer le trafic Northbound et Southbound.  
  
-   Déployez et utilisez un équilibreur de charge logiciel pour distribuer le trafic Eastbound et Westbound pour les réseaux virtuels créés avec la virtualisation de réseau Hyper-V.  
  
-   Déployez et utilisez un équilibreur de charge logiciel NAT pour les réseaux virtuels créés avec la virtualisation de réseau Hyper-V.  
  
-   Déployer et utiliser une passerelle de transfert de couche 3  
  
-   Déployer et utiliser une passerelle de réseau privé virtuel (VPN) pour les tunnels IPsec de site à site (IKEv2)  
  
-   Déployez et utilisez une passerelle de l’encapsulation générique de routage (GRE).  
  
-   Déployez et configurez le routage dynamique et le routage de transit entre les sites à l’aide d’Border Gateway Protocol (BGP).  
  
-   Configurez la redondance M + N pour les passerelles de couche 3 et de site à site, ainsi que pour le routage BGP.  
  
-   Utilisez le contrôleur de réseau pour spécifier des listes de contrôle d’accès sur des réseaux virtuels et des interfaces réseau.  
  
Pour plus d’informations, consultez [Network Function Virtualization](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Scénarios de plateforme réseau

Pour les scénarios de cette section, l’équipe de mise en réseau Windows Server prend en charge l’utilisation de n’importe quel pilote certifié Windows Server 2016. Vérifiez auprès de votre carte d’interface réseau \(fabricant de la carte réseau\) pour vous assurer que vous disposez des mises à jour les plus récentes du pilote.
  
Les scénarios de plateforme réseau vous permettent d’effectuer les opérations suivantes :  
  
-   Utilisez une carte réseau convergée pour combiner le trafic RDMA et Ethernet à l’aide d’une seule carte réseau.  
  
-   Créez un chemin de données à faible latence en utilisant Packet direct, activé dans le commutateur virtuel Hyper-V et une seule carte réseau.  
  
-   Configurer SET pour répartir les flux de trafic SMB direct et RDMA entre jusqu’à deux cartes réseau.  
  
Pour plus d’informations, [ &#40;consultez accès direct à la&#41; mémoire à distance RDMA et &#40;Switch&#41;Embedded Teaming Set](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Scénarios de commutateur virtuel Hyper-V

Les scénarios de commutateur virtuel Hyper-V vous permettent d’effectuer les opérations suivantes :  
  
-   Créer un commutateur virtuel Hyper-V avec un accès direct à la mémoire à distance (RDMA) carte réseau virtuelle  
  
-   Créer un commutateur virtuel Hyper-V avec switch Embedded Teaming (SET) et RDMA cartes réseau virtuelles  
  
-   Créer une équipe de jeu dans le commutateur virtuel Hyper-V  
  
-   Gérer une équipe de jeu à l’aide de commandes Windows PowerShell  
  
Pour plus d’informations, [ &#40;consultez accès direct à la&#41; mémoire à distance RDMA et &#40;Switch&#41; Embedded Teaming Set](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Scénarios de serveur DNS

Les scénarios de serveur DNS vous permettent d’effectuer les opérations suivantes :  
  
-   Spécifier la gestion du trafic basée sur la géolocalisation à l’aide de stratégies DNS  
  
-   Configurer le DNS split-brain à l’aide de stratégies DNS  
  
-   Appliquer des filtres sur des requêtes DNS à l’aide de stratégies DNS  
  
-   Configurer l’équilibrage de charge des applications à l’aide de stratégies DNS  
  
-   Spécifier les réponses DNS intelligentes en fonction de l’heure de la journée  
  
-   Configurer des stratégies de transfert de zone DNS  
  
-   Configurer des stratégies de serveur DNS sur des zones intégrées Active Directory Domain Services (AD DS)  
  
-   Configurer la limitation du taux de réponses  
  
-   Spécifier l’authentification DNS des entités nommées (n)  
  
-   Configurer la prise en charge des enregistrements inconnus dans DNS  
  
Pour plus d’informations, consultez les rubriques [Nouveautés du client DNS dans Windows server 2016](dns/What-s-New-in-DNS-Client.md) et [Nouveautés du serveur DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Scénarios IPAM avec DHCP et DNS

Les scénarios IPAM vous permettent d’effectuer les opérations suivantes :  
  
-   Détection et administration des serveurs DNS et DHCP et de l’adressage IP sur plusieurs forêts Active Directory fédérées  
  
-   Utilisez IPAM pour la gestion centralisée des propriétés DNS, y compris les zones et les enregistrements de ressources.  
  
-   Définir des stratégies granulaires de contrôle d’accès en fonction du rôle et déléguer des utilisateurs ou des groupes d’utilisateurs IPAM pour gérer l’ensemble des propriétés DNS que vous spécifiez.  
  
-   Utilisez les commandes Windows PowerShell pour IPAM afin d’automatiser la configuration du contrôle d’accès pour DHCP et DNS.  
  
    Pour plus d’informations, consultez [gérer IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Scénarios d’association de cartes réseau

Les scénarios d’association de cartes réseau vous permettent d’effectuer les opérations suivantes :  
  
-   Créer une association de cartes réseau dans une configuration prise en charge  
  
-   Supprimer une association de cartes réseau  
  
-   Ajouter des cartes réseau à l’Association de cartes réseau dans une configuration prise en charge  
  
-   Supprimer les cartes réseau de l’Association de cartes réseau  
  
> [!NOTE]  
> Dans Windows Server 2016, vous pouvez utiliser l’Association de cartes réseau dans Hyper-V. Toutefois, dans certains cas, les files d’attente d’ordinateurs virtuels peuvent ne pas être activées automatiquement sur les cartes réseau sous-jacentes lorsque vous créez une association de cartes réseau. Si cela se produit, vous pouvez utiliser la commande Windows PowerShell suivante pour vous assurer que la carte réseau est activée sur les adaptateurs de membres de l’équipe de cartes réseau : `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Pour plus d’informations, consultez [Association de cartes réseau](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Basculer l’Association incorporée \(définir des scénarios de\)

SET est une autre solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile SDN (Software Defined Networking) dans Windows Server 2016. SET intègre certaines fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. 

Pour plus d’informations, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Scénarios de mise en réseau non pris en charge  
Les scénarios de mise en réseau suivants ne sont pas pris en charge dans Windows Server 2016.  
  
-   Réseaux virtuels locataires basés sur un réseau local virtuel.  
  
-   IPv6 n’est pas pris en charge dans Underlay ou Overlay.  
  


