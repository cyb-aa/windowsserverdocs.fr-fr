---
title: Scénarios de mise en réseau pris en charge par Windows Server
description: Cette rubrique fournit des informations sur les nouveaux scénarios de mise en réseau pris en charge dans Windows Server 2016 et versions ultérieures
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85f73f1f7caf833d23d3d693c0d754f52c4aa27d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812230"
---
# <a name="windows-server-supported-networking-scenarios"></a>Scénarios de mise en réseau pris en charge par Windows Server

>S’applique à : Windows Server \(canal semi-annuel\), Windows Server 2016

Cette rubrique fournit des informations sur les scénarios pris en charge et non pris en charge que vous pouvez ou ne pouvez pas effectuer avec cette version de Windows Server 2016.  
>[!IMPORTANT]
>Pour tous les scénarios de production, utilisez les dernières signé pilotes à partir de votre fabricant \(OEM\) ou fournisseur de matériel indépendants \(IHV\).
  
## <a name="bkmk_supp"></a>Scénarios de mise en réseau pris en charge

Cette section inclut des informations sur les scénarios de mise en réseau pris en charge pour Windows Server 2016 et inclut les catégories suivantes de scénario.  
  
-   [Scénarios de Software Defined Networking (SDN)](#bkmk_sdn)  
  
-   [Scénarios de réseau de plateforme](#bkmk_netp)  
  
-   [Scénarios de serveur DNS](#bkmk_dns)  
  
-   [Scénarios d’IPAM avec DHCP et DNS](#bkmk_ipam)  
  
-   [Association de cartes réseau de scénarios](#bkmk_nicteam)

- [Switch Embedded Teaming \(définir\) scénarios](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Scénarios de Software Defined Networking (SDN)
 
Vous pouvez utiliser la documentation suivante pour déployer des scénarios SDN avec Windows Server 2016.  
  
  
-   [Déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Pour plus d’informations, consultez [Sdn &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Scénarios de contrôleur de réseau

Les scénarios de contrôleur de réseau vous permettent de :  
  
-   Déployer et gérer une instance de plusieurs nœuds de contrôleur de réseau. Pour plus d’informations, consultez [déployer de contrôleur de réseau à l’aide de Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Contrôleur de réseau permet de définir par programme la stratégie de réseau à l’aide de l’API Northbound REST.  
  
-   Utiliser le contrôleur de réseau pour créer et gérer des réseaux virtuels avec virtualisation de réseau Hyper-V - à l’aide de l’encapsulation NVGRE ou VXLAN.  
  
Pour plus d’informations, voir [Contrôleur de réseau](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Réseau de scénarios de virtualisation (fonction) (NFV)  
Les scénarios NFV vous permettent de :  
  
-   Déployez et utilisez un équilibreur de charge logiciel pour répartir le trafic northbound et southbound.  
  
-   Déployez et utilisez un équilibreur de charge logiciel pour répartir le trafic eastbound et westbound pour les réseaux virtuels créés avec la virtualisation de réseau Hyper-V.  
  
-   Déployez et utilisez un équilibreur de charge logiciel NAT pour les réseaux virtuels créés avec la virtualisation de réseau Hyper-V.  
  
-   Déployer et utiliser une passerelle de transfert de couche 3  
  
-   Déployer et utiliser une passerelle de réseau privé virtuel (VPN) pour les tunnels IPsec (IKEv2) site à site  
  
-   Déployer et utiliser une passerelle d’Encapsulation GRE (Generic Routing).  
  
-   Déployer et configurer le routage dynamique et le routage de transit entre les sites à l’aide du protocole BGP (Border Gateway).  
  
-   Configurer une redondance M + N pour couche 3 et les passerelles de site à site et pour le routage BGP.  
  
-   Contrôleur de réseau permet de spécifier des ACL sur les réseaux virtuels et des interfaces réseau.  
  
Pour plus d’informations, consultez [virtualisation de fonction réseau](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Scénarios de réseau de plateforme

Pour les scénarios de cette section de la mise en réseau Windows Server team prend en charge l’utilisation de n’importe quel pilote certifiés Windows Server 2016. Vérifiez avec votre carte d’interface réseau \(carte réseau\) fabricant pour vous assurer les dernières mises à jour de pilote.
  
Les scénarios de plate-forme réseau vous permettent de :  
  
-   Utiliser une carte réseau convergence pour combiner de trafic RDMA et Ethernet à l’aide d’une seule carte réseau.  
  
-   Créer un chemin d’accès de données à faible latence à l’aide de paquets Direct, sont activés dans le commutateur virtuel Hyper-V et une seule carte réseau.  
  
-   Configurer ensemble pour répartir les flux de trafic SMB Direct et RDMA entre les deux cartes réseau.  
  
Pour plus d’informations, consultez [Remote Direct Memory Access &#40;RDMA&#41; et Switch Embedded Teaming &#40;définir&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Scénarios de commutateur virtuel Hyper-V

Les scénarios de commutateur virtuel Hyper-V vous permettent de :  
  
-   Créer un commutateur virtuel Hyper-V avec une carte réseau virtuelle de l’accès de mémoire Direct à distance (RDMA)  
  
-   Créer un commutateur virtuel Hyper-V avec des cartes VNIC RDMA et SET Switch Embedded Teaming)  
  
-   Créer une équipe de jeu dans le commutateur virtuel Hyper-V  
  
-   Gérer une équipe de jeu à l’aide de commandes Windows PowerShell  
  
Pour plus d’informations, consultez [Remote Direct Memory Access &#40;RDMA&#41; et Switch Embedded Teaming &#40;définie&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Scénarios de serveur DNS

Scénarios de serveur DNS vous autorise à :  
  
-   Spécifiez le Qu'emplacement géographique en fonction de gestion du trafic à l’aide de stratégies de DNS  
  
-   Configurer le DNS « split brain » à l’aide de stratégies de DNS  
  
-   Appliquer des filtres sur les requêtes DNS à l’aide de stratégies de DNS  
  
-   Configurer l’équilibrage de charge Application à l’aide de stratégies de DNS  
  
-   Spécifiez que les réponses DNS intelligentes basées sur l’heure du jour  
  
-   Configurer des stratégies de transfert de Zone DNS  
  
-   Configurer des zones de stratégies sur les Services de domaine Active Directory (AD DS) intégrés de serveur DNS  
  
-   Configurer les taux de réponse de limitation  
  
-   Spécifier l’authentification basée sur DNS d’entités nommées (DANE)  
  
-   Configurer la prise en charge pour les enregistrements inconnus dans DNS  
  
Pour plus d’informations, consultez les rubriques [What ' s New in Client DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Client.md) et [What ' s New in serveur DNS dans Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Scénarios d’IPAM avec DHCP et DNS

Les scénarios d’IPAM vous permettent de :  
  
-   Découvrir et d’administrer des serveurs DNS et DHCP et l’adressage IP sur plusieurs forêts Active Directory fédérés  
  
-   Utiliser IPAM pour la gestion centralisée des propriétés DNS, notamment les zones et enregistrements de ressources.  
  
-   Définir des stratégies de contrôle d’accès en fonction du rôle granulaire et déléguer les groupes d’utilisateurs ou les utilisateurs IPAM pour gérer l’ensemble de propriétés DNS que vous spécifiez.  
  
-   Utilisez les commandes Windows PowerShell pour IPAM pour automatiser la configuration de contrôle d’accès pour DHCP et DNS.  
  
    Pour plus d’informations, consultez [gérer IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Association de cartes réseau de scénarios

Les scénarios d’association de cartes réseau vous permettent de :  
  
-   Créer une association de cartes réseau dans une configuration prise en charge  
  
-   Supprimer une association de cartes réseau  
  
-   Ajouter des cartes réseau à l’association de cartes réseau dans une configuration prise en charge  
  
-   Supprimer des cartes réseau à partir de l’association de cartes réseau  
  
> [!NOTE]  
> Dans Windows Server 2016, vous pouvez utiliser association de cartes réseau dans Hyper-V, mais dans certains cas files d’attente (ordinateurs virtuels) ne peut pas activer automatiquement sur les cartes réseau sous-jacents lorsque vous créez une association de cartes réseau. Si cela se produit, vous pouvez utiliser la commande Windows PowerShell suivante pour vous assurer que VMQ est activé sur les cartes de membre de carte réseau : `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Pour plus d’informations, consultez [association de cartes réseau](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Switch Embedded Teaming \(définir\) scénarios

JEU est une autre solution association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile de mise en réseau SDN (Software Defined) dans Windows Server 2016. JEU intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. 

Pour plus d’informations, consultez [accès de mémoire Direct à distance (RDMA) et SET Switch Embedded Teaming ()](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Scénarios de mise en réseau non pris en charge  
Les scénarios de mise en réseau suivantes ne sont pas pris en charge dans Windows Server 2016.  
  
-   Réseaux virtuels du client basée sur un VLAN.  
  
-   IPv6 n’est pas pris en charge dans la sous-couche ou un segment de recouvrement.  
  


