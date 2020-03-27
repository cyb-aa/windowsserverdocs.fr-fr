---
title: Tunneling GRE dans Windows Server 2016
description: Vous pouvez utiliser cette rubrique pour mieux comprendre les mises à jour de la fonctionnalité de tunnel GRE (Generic Routing Encapsulation) pour la passerelle RAS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: df2023bf-ba64-481e-b222-6f709edaa5c1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d246f0e56681f75e4336ed225d1557a0e05c581b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308557"
---
# <a name="gre-tunneling-in-windows-server-2016"></a>Tunneling GRE dans Windows Server 2016

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Windows Server 2016 fournit des mises à jour de l’encapsulation générique de routage \(la fonctionnalité de tunneling GRE\) pour la passerelle RAS.  
  
GRE est un protocole de tunneling léger qui peut encapsuler une grande variété de protocoles de la couche réseau dans les liaisons point à point virtuelles sur un réseau d’interconnexion IP. L’implémentation Microsoft GRE peut encapsuler IPv4 et IPv6.  
  
Les tunnels GRE sont utiles dans de nombreux scénarios, car :  
  
-   Ils sont conformes à la norme Lightweight et RFC 2890, ce qui les rend interopérables avec différents appareils de fournisseurs  
  
-   Vous pouvez utiliser Border Gateway Protocol \(\) BGP pour le routage dynamique  
  
-   Vous pouvez configurer des passerelles RAS multi-locataires GRE pour une utilisation avec un réseau à définition logicielle \(SDN\)
  
-   Vous pouvez utiliser System Center Virtual Machine Manager pour gérer les passerelles RAS\-basées sur GRE
  
-   Vous pouvez atteindre jusqu’à 2,0 Gbits/s de débit sur une machine virtuelle 6 cœurs configurée en tant que passerelle RAS GRE
  
-   Une seule passerelle prend en charge plusieurs modes de connexion  
  
Les tunnels GRE activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Étant donné que le protocole GRE est léger et que la prise en charge de GRE est disponible sur la plupart des périphériques réseau, il constitue un choix idéal pour le tunneling où le chiffrement des données n’est pas nécessaire. 

La prise en charge de GRE dans les tunnels de site à site (S2S) résout le problème de transfert entre les réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée, comme décrit plus loin dans cette rubrique.  
  
La fonctionnalité de tunnel GRE est conçue pour répondre aux exigences suivantes :  
  
-   Un fournisseur d’hébergement doit être en mesure de créer des réseaux virtuels à transférer sans modifier la configuration du commutateur physique.  
  
-   Un fournisseur d’hébergement doit être en mesure d’ajouter des sous-réseaux à leurs réseaux connectés en externe sans modifier la configuration des commutateurs physiques au sein de leur infrastructure.  
La fonctionnalité de tunnel GRE active ou améliore plusieurs scénarios clés pour l’hébergement de fournisseurs de services à l’aide des technologies Microsoft pour implémenter la mise en réseau définie par logiciel dans leurs offres de service.  
  
Voici quelques exemples de scénarios :  
  
-   [Accès à partir de réseaux virtuels locataires à des réseaux physiques locataires](#BKMK_Access)  
  
-   [Connectivité à grande vitesse](#BKMK_Speed)  
  
-   [Intégration avec l’isolation basée sur un réseau local virtuel](#BKMK_Integration)  
  
-   [Accéder aux ressources partagées](#BKMK_Shared)  
  
-   [Services d’appareils tiers aux locataires](#BKMK_thirdparty)  
  
## <a name="key-scenarios"></a>Principaux scénarios

Voici les principaux scénarios que la fonctionnalité de tunnel GRE adresse.  
  
### <a name="access-from-tenant-virtual-networks-to-tenant-physical-networks"></a><a name="BKMK_Access"></a>Accès à partir de réseaux virtuels locataires à des réseaux physiques locataires

Ce scénario offre un moyen évolutif de fournir un accès à partir de réseaux virtuels locataires à des réseaux physiques clients situés sur le site du fournisseur de services d’hébergement. Un point de terminaison de tunnel GRE est établi sur la passerelle mutualisée. l’autre point de terminaison de tunnel GRE est établi sur un appareil tiers sur le réseau physique. Le trafic de couche 3 est routé entre les machines virtuelles du réseau virtuel et l’appareil tiers sur le réseau physique.  
  
![Tunnel GRE connectant le réseau physique de l’hébergeur et le réseau virtuel du locataire](../../media/gre-tunneling-in-windows-server/GRE_.png)  
  
### <a name="high-speed-connectivity"></a><a name="BKMK_Speed"></a>Connectivité à grande vitesse

Ce scénario offre une méthode évolutive pour fournir une connectivité à haut débit à partir du réseau local du client vers son réseau virtuel situé dans le réseau du fournisseur de services d’hébergement. Un locataire se connecte au réseau du fournisseur de services via MPLS (Multiprotocol Label Switching), où un tunnel GRE est établi entre le routeur de périphérie du fournisseur de services d’hébergement et la passerelle mutualisée sur le réseau virtuel du locataire.  
  
![Tunnel GRE connectant le réseau client MPLS et le réseau virtuel du locataire de l’entreprise](../../media/gre-tunneling-in-windows-server/GRE-.png)  
  
### <a name="integration-with-vlan-based-isolation"></a><a name="BKMK_Integration"></a>Intégration avec l’isolation basée sur un réseau local virtuel

Ce scénario vous permet d’intégrer l’isolation basée sur un réseau local virtuel avec la virtualisation de réseau Hyper-V. Un réseau physique sur le réseau du fournisseur d’hébergement contient un équilibreur de charge à l’aide de l’isolation basée sur un réseau local virtuel. Une passerelle mutualisée établit des tunnels GRE entre l’équilibreur de charge sur le réseau physique et la passerelle mutualisée sur le réseau virtuel.  
  
Il est possible d’établir plusieurs tunnels entre la source et la destination, et la clé GRE est utilisée pour distinguer les tunnels.  
  
![Plusieurs tunnels GRE connectant des réseaux virtuels locataires](../../media/gre-tunneling-in-windows-server/GRE-VLANIsolation.png)  
  
### <a name="access-shared-resources"></a><a name="BKMK_Shared"></a>Accéder aux ressources partagées

Ce scénario vous permet d’accéder à des ressources partagées sur un réseau physique situé sur le réseau du fournisseur d’hébergement.  
  
Vous pouvez avoir un service partagé situé sur un serveur sur un réseau physique situé dans le réseau du fournisseur d’hébergement que vous souhaitez partager avec plusieurs réseaux virtuels locataires.  
  
Les réseaux locataires avec des sous-réseaux qui ne se chevauchent pas accèdent au réseau commun via un tunnel GRE. Une passerelle de locataire unique achemine les paquets entre les tunnels GRE, acheminant ainsi les paquets vers les réseaux de locataire appropriés.  
  
Dans ce scénario, la passerelle de locataire unique peut être remplacée par des appliances matérielles tierces.  
  
![Une passerelle à locataire unique utilisant plusieurs tunnels pour connecter plusieurs réseaux virtuels](../../media/gre-tunneling-in-windows-server/GRE-SharedResource.png)  
  
### <a name="services-of-third-party-devices-to-tenants"></a><a name="BKMK_thirdparty"></a>Services d’appareils tiers aux locataires

Ce scénario peut être utilisé pour intégrer des appareils tiers (tels que des équilibreurs de charge matérielle) au flux de trafic de réseau virtuel du locataire. Par exemple, le trafic provenant d’un site d’entreprise passe par un tunnel S2S à la passerelle mutualisée. Le trafic est acheminé vers l’équilibreur de charge via un tunnel GRE. L’équilibreur de charge achemine le trafic vers plusieurs machines virtuelles sur le réseau virtuel de l’entreprise. La même chose se produit pour un autre client avec des adresses IP qui se chevauchent potentiellement dans les réseaux virtuels. Le trafic réseau est isolé sur l’équilibreur de charge à l’aide de réseaux locaux virtuels et s’applique à tous les appareils de couche 3 qui prennent en charge les réseaux locaux virtuels.  
  
![Plusieurs tunnels GRE connectant des réseaux virtuels à des appareils tiers](../../media/gre-tunneling-in-windows-server/GREThirdParty.png)  
  
## <a name="configuration-and-deployment"></a>Configuration et déploiement

Un tunnel GRE est exposé en tant que protocole supplémentaire au sein d’une interface S2S. Elle est implémentée de façon similaire à un tunnel IPSec S2S décrit dans le blog sur la mise en réseau suivant : [passerelle VPN de site à site (S2S) mutualisée avec Windows Server 2012 R2](https://blogs.technet.com/b/networking/archive/2013/09/29/multi-tenant-site-to-site-s2s-vpn-gateway-with-windows-server-2012-r2.aspx)  
  
Consultez la rubrique suivante pour obtenir un exemple de déploiement de passerelles, notamment les passerelles de tunnel GRE :  
  
[Déployer une infrastructure réseau définie par logiciel à l’aide de scripts](../../../networking/sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
  
## <a name="more-information"></a>Informations supplémentaires

Pour plus d’informations sur le déploiement de passerelles S2S, consultez les rubriques suivantes :  
  
-   [Passerelle du serveur d’accès à distance](RAS-Gateway.md)  
  
-   [Border Gateway Protocol &#40;BGP&#41;](../bgp/Border-Gateway-Protocol-BGP.md)  
  
-   [Nouveau! Guide de déploiement de la passerelle mutualisée RAS Windows Server 2012 R2](https://blogs.technet.com/b/wsnetdoc/archive/2014/03/26/new-windows-server-2012-r2-RAS-multitenant-gateway-deployment-guide.aspx)  
  
-   [Déployer Border Gateway Protocol (BGP) avec la passerelle mutualisée RAS](https://blogs.technet.com/b/wsnetdoc/archive/2014/04/03/deploy-border-gateway-protocol-bgp-with-the-RAS-multitenant-gateway.aspx)  
  


