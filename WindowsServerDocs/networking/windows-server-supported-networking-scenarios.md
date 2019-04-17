---
title: Windows Server pris en charge les scénarios de mise en réseau
description: Cette rubrique fournit des informations sur les nouveaux scénarios de mise en réseau pris en charge dans Windows Server2016 et versions ultérieures
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 70198f97c4ec39de4b78de28ab196dc3e86a684c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="windows-server-supported-networking-scenarios"></a>Windows Server pris en charge les scénarios de mise en réseau

>S’applique à: De Windows Server \(Semi-Annual Channel\), Windows Server2016

Cette rubrique fournit des informations sur les scénarios pris en charge et non pris en charge que vous pouvez ou que vous ne pouvez pas effectuer avec cette version de Windows Server 2016.  
>[!IMPORTANT]
>Pour tous les scénarios de production, utilisez les dernières signé pilotes à partir de votre fabricant \(OEM\) ou fournisseur de matériel indépendants \(IHV\).
  
## <a name="bkmk_supp"></a>Scénarios de mise en réseau pris en charge

Cette section comprend des informations sur les scénarios de mise en réseau pris en charge pour Windows Server 2016 et inclut les catégories de scénario suivantes.  
  
-   [Scénarios de mise en réseau définie (SDN) de logiciels](#bkmk_sdn)  
  
-   [Scénarios de plate-forme réseau](#bkmk_netp)  
  
-   [Scénarios de serveur DNS](#bkmk_dns)  
  
-   [Scénarios d’IPAM avec le protocole DHCP et DNS](#bkmk_ipam)  
  
-   [Scénarios d’association de cartes réseau](#bkmk_nicteam)

- [Commutateur \(SET\) Embedded Teaming scénarios](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Scénarios de mise en réseau définie (SDN) de logiciels
 
Vous pouvez utiliser la documentation suivante pour déployer des scénarios SDN avec Windows Server 2016.  
  
  
-   [Déployer une infrastructure réseau à définition logicielle à l’aide de scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Pour plus d’informations, voir [logiciel défini de mise en réseau & #40; SDN & #41; ](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Scénarios de contrôleur de réseau

Les scénarios de contrôleur de réseau vous permettent de:  
  
-   Déployer et gérer une instance de plusieurs nœuds de contrôleur de réseau. Pour plus d’informations, voir [déployer le contrôleur de réseau à l’aide de Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Contrôleur de réseau permet de définir par programmation la stratégie de réseau à l’aide de l’API northbound du reste.  
  
-   Contrôleur de réseau permet de créer et gérer des réseaux virtuels avec Hyper-V virtualisation de réseau - encapsulation NVGRE ou VXLAN.  
  
Pour plus d’informations, voir [contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Scénarios de réseau de la virtualisation de fonction (NFV)  
Les scénarios NFV vous permettent de:  
  
-   Déployer et utiliser un équilibrage de charge logicielle pour distribuer le trafic northbound et southbound.  
  
-   Déployer et utiliser un équilibrage de charge logicielle pour distribuer le trafic eastbound et westbound pour les réseaux virtuels créés avec la virtualisation de réseau Hyper-V.  
  
-   Déployer et utiliser un équilibreur de charge logicielle NAT pour les réseaux virtuels créés avec la virtualisation de réseau Hyper-V.  
  
-   Déployer et utiliser une passerelle de transfert de couche 3  
  
-   Déployer et utiliser une passerelle de réseau privé virtuel (VPN) pour les tunnels IPsec (IKEv2) de site à site  
  
-   Déployer et utiliser une passerelle Encapsulation GRE (Generic Routing).  
  
-   Déployer et configurer le routage dynamique et le routage de transit entre sites à l’aide du protocole BGP (Border Gateway).  
  
-   Configurer la redondance M + N de couche 3 et des passerelles site-à-site et pour le routage BGP.  
  
-   Contrôleur de réseau permet de spécifier des ACL sur les réseaux virtuels et les interfaces réseau.  
  
Pour plus d’informations, voir [la virtualisation de fonction réseau](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Scénarios de plate-forme réseau

Pour les scénarios de cette section de la mise en réseau Windows Server équipe prend en charge l’utilisation de n’importe quel pilote certifiés Windows Server 2016. Contactez votre fabricant de \(NIC\) carte réseau pour vous assurer de qu'avoir les dernières mises à jour du pilote.
  
Les scénarios de plate-forme réseau vous permettent de:  
  
-   Utiliser une carte réseau convergence pour combiner RDMA et Ethernet le trafic à l’aide d’une seule carte réseau.  
  
-   Créer un chemin d’accès de données à faible latence à l’aide de paquets Direct, activé dans le commutateur virtuel Hyper-V et une seule carte réseau.  
  
-   Configurer pour répartir les flux de trafic SMB Direct et RDMA entre les deux cartes réseau.  
  
Pour plus d’informations, voir [Remote Direct Memory Access & #40; RDMA & #41; et Switch Embedded Teaming & #40; SET & #41; ](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Scénarios de commutateur virtuel Hyper-V

Les scénarios de commutateur virtuel Hyper-V vous permettent de:  
  
-   Créer un commutateur virtuel Hyper-V avec une carte réseau virtuelle de l’accès à la mémoire Direct à distance (RDMA)  
  
-   Créer un commutateur virtuel Hyper-V avec des cartes VNIC RDMA et commutateur SET (Embedded Teaming)  
  
-   Créer une équipe de jeu dans le commutateur virtuel Hyper-V  
  
-   Gérer une équipe de jeu à l’aide de commandes Windows PowerShell  
  
Pour plus d’informations, voir [Remote Direct Memory Access & #40; RDMA & #41; et Switch Embedded Teaming & #40; SET & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Scénarios de serveur DNS

Scénarios de serveur DNS vous permettent de:  
  
-   Spécifiez la que géolocalisation en fonction de gestion du trafic à l’aide de stratégies DNS  
  
-   Configurer le DNS «split brain», à l’aide de stratégies DNS  
  
-   Appliquer des filtres sur les requêtes DNS à l’aide de stratégies DNS  
  
-   Configurer l’équilibrage de charge Application à l’aide de stratégies DNS  
  
-   Spécifiez que les réponses DNS intelligentes basées sur l’heure du jour  
  
-   Configurer des stratégies de transfert de Zone DNS  
  
-   Configurez les zones de stratégies sur les Services de domaine Active Directory (AD DS) intégrées de serveur DNS  
  
-   Configurer les taux de limitation  
  
-   Spécifier l’authentification basée sur DNS d’entités nommées (DANE)  
  
-   Configurer la prise en charge pour les enregistrements inconnus dans DNS  
  
Pour plus d’informations, consultez les rubriques [Nouveautés dans le Client DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Client.md) et [Nouveautés dans le serveur DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Scénarios d’IPAM avec le protocole DHCP et DNS

Les scénarios d’IPAM vous permettent de:  
  
-   Découvrir et d’administrer des serveurs DNS et DHCP et l’adressage IP dans plusieurs forêts Active Directory fédérés  
  
-   Utilisation d’IPAM pour la gestion centralisée des propriétés DNS, notamment les zones et les enregistrements de ressource.  
  
-   Définir des stratégies de contrôle granulaire de l’accès basé sur les rôles et de déléguer des utilisateurs IPAM ou les groupes d’utilisateurs pour gérer l’ensemble des propriétés DNS que vous spécifiez.  
  
-   Utilisez les commandes Windows PowerShell pour IPAM pour automatiser la configuration de contrôle d’accès pour le protocole DHCP et DNS.  
  
    Pour plus d’informations, voir [gérer IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Scénarios d’association de cartes réseau

Les scénarios d’association de cartes réseau vous permettent de:  
  
-   Créer une association de cartes réseau dans une configuration prise en charge  
  
-   Supprimer une association de cartes réseau  
  
-   Ajouter des cartes réseau à l’équipe de cartes réseau dans une configuration prise en charge  
  
-   Suppression de l’association de cartes réseau  
  
> [!NOTE]  
> Dans Windows Server 2016, vous pouvez utiliser l’association de cartes réseau dans Hyper-V, mais dans certains cas files d’attente de l’ordinateur virtuel (VMQ) ne peuvent pas activer automatiquement sur les cartes réseau sous-jacente lorsque vous créez une association. Si cela se produit, vous pouvez utiliser la commande Windows PowerShell suivante pour vous assurer que VMQ est activé sur les cartes NIC membre: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Pour plus d’informations, voir [association de cartes réseau](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Commutateur \(SET\) Embedded Teaming scénarios

JEU est une solution alternative association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile logicielle définie de mise en réseau (SDN) dans Windows Server 2016. ENSEMBLE intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. 

Pour plus d’informations, voir [accès à la mémoire Direct à distance (RDMA) et le commutateur SET (Embedded Teaming)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Scénarios de mise en réseau non pris en charge  
Les scénarios de mise en réseau suivantes ne sont pas pris en charge dans Windows Server 2016.  
  
-   Réseaux virtuels client basée sur un réseau local virtuel.  
  
-   IPv6 n’est pas pris en charge dans le segment de recouvrement ou sous-jacent.  
  


