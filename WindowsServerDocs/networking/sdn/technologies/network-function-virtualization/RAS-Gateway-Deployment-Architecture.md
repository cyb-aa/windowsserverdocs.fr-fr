---
title: Architecture de déploiement de la passerelle du serveur d’accès à distance
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur le déploiement du fournisseur de services Cloud (CSP) de la passerelle RAS dans Windows Server 2016, y compris les pools de passerelle RAS, les réflecteurs d’itinéraires et le déploiement de plusieurs passerelles pour des locataires individuels.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 91d8081261d3cbc5e2da61cc2b5a9737e76a0dc7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309803"
---
# <a name="ras-gateway-deployment-architecture"></a>Architecture de déploiement de la passerelle du serveur d’accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur le déploiement du fournisseur de services Cloud (CSP) de la passerelle RAS, y compris les pools de passerelle RAS, les réflecteurs de route et le déploiement de plusieurs passerelles pour des locataires individuels.  
  
Les sections suivantes fournissent de brèves vues d’ensemble des nouvelles fonctionnalités de la passerelle RAS afin que vous puissiez comprendre comment utiliser ces fonctionnalités dans la conception de votre déploiement de passerelle.  
  
En outre, un exemple de déploiement est fourni, y compris des informations sur le processus d’ajout de nouveaux locataires, la synchronisation d’itinéraires et le routage de plan de données, le basculement de passerelle et de Reflector de routage, et bien plus encore.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Utilisation des nouvelles fonctionnalités de la passerelle RAS pour concevoir votre déploiement](#bkmk_new)  
  
-   [Exemple de déploiement](#bkmk_example)  
  
-   [Ajout de nouveaux locataires et de l’espace d’adressage du client (CA) EBGP appairage](#bkmk_tenant)  
  
-   [Synchronisation des itinéraires et routage du plan de données](#bkmk_route)  
  
-   [Comment le contrôleur de réseau répond à la passerelle RAS et au basculement de réflecteur d’itinéraire](#bkmk_failover)  
  
-   [Avantages de l’utilisation de nouvelles fonctionnalités de passerelle RAS](#bkmk_advantages)  
  
## <a name="using-ras-gateway-new-features-to-design-your--deployment"></a><a name="bkmk_new"></a>Utilisation des nouvelles fonctionnalités de la passerelle RAS pour concevoir votre déploiement  
La passerelle RAS comprend plusieurs nouvelles fonctionnalités qui changent et améliorent la manière dont vous déployez votre infrastructure de passerelle dans votre centre de donnes.  
  
### <a name="bgp-route-reflector"></a>Réflecteur d’itinéraires BGP  
La fonctionnalité de réflecteur d’itinéraire Border Gateway Protocol (BGP) est désormais incluse avec la passerelle RAS et fournit une alternative à la topologie de maillage complet BGP qui est normalement requise pour la synchronisation d’itinéraires entre les routeurs. Avec la synchronisation de maillage complète, tous les routeurs BGP doivent se connecter à tous les autres routeurs de la topologie de routage. Toutefois, lorsque vous utilisez le réflecteur d’itinéraire, le réflecteur d’itinéraire est le seul routeur qui se connecte à tous les autres routeurs, appelés clients de réflecteur d’itinéraires BGP, ce qui simplifie la synchronisation des itinéraires et réduit le trafic réseau. Le réflecteur d’itinéraire apprend tous les routages, calcule les meilleurs itinéraires et redistribue les meilleurs itinéraires à ses clients BGP.  
  
Pour plus d’informations, consultez [Nouveautés de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="gateway-pools"></a><a name="bkmk_pools"></a>Pools de passerelle  
Dans Windows Server 2016, vous pouvez créer plusieurs pools de passerelles de différents types. Les pools de passerelle contiennent de nombreuses instances de passerelle RAS et acheminent le trafic réseau entre les réseaux physiques et virtuels.  
  
Pour plus d’informations, consultez [Nouveautés de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [haute disponibilité de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="gateway-pool-scalability"></a><a name="bkmk_gps"></a>Extensibilité du pool de passerelles  
Vous pouvez facilement mettre à l’échelle un pool de passerelle en ajoutant ou en supprimant des machines virtuelles de passerelle dans le pool. La suppression ou l’ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer des pools complets de passerelles.  
  
Pour plus d’informations, consultez [Nouveautés de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [haute disponibilité de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="mn-gateway-pool-redundancy"></a><a name="bkmk_m"></a>Redondance du pool de passerelle M + N  
Chaque pool de passerelle est M + N redondant. Cela signifie qu’un nombre « m » de machines virtuelles de passerelle actives est sauvegardé par un nombre « N » de machines virtuelles de passerelle de secours. La redondance M + N offre une plus grande flexibilité pour déterminer le niveau de fiabilité dont vous avez besoin lorsque vous déployez la passerelle RAS.  
  
Pour plus d’informations, consultez [Nouveautés de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [haute disponibilité de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="example-deployment"></a><a name="bkmk_example"></a>Exemple de déploiement  
L’illustration suivante fournit un exemple d’homologation eBGP sur des connexions VPN de site à site configurées entre deux locataires, contoso et Woodgrove, et le centre de donnés de Fabrikam CSP.  
  
![homologation eBGP sur un réseau VPN de site à site](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
Dans cet exemple, contoso requiert une bande passante de passerelle supplémentaire, conduisant à la décision de conception de l’infrastructure de la passerelle pour mettre fin au site contoso Los Angeles sur GW3 au lieu de GW2. Pour cette raison, les connexions VPN contoso à partir de différents sites se terminent dans le centre de déchiffreur CSP sur deux passerelles différentes.  
  
Ces deux passerelles, GW2 et GW3, étaient les premières passerelles RAS configurées par le contrôleur de réseau lorsque le CSP a ajouté les locataires contoso et Woodgrove à leur infrastructure. Pour cette raison, ces deux passerelles sont configurées en tant que réflecteurs d’itinéraires pour ces clients (ou clients) correspondants. GW2 est le réflecteur de l’itinéraire contoso et GW3 est le réflecteur de l’itinéraire de Woodgrove, en plus d’être le point de terminaison de la passerelle RAS du CSP pour la connexion VPN avec le site contoso Los Angeles HQ.  
  
> [!NOTE]  
> Une passerelle RAS peut acheminer le trafic réseau virtuel et physique pour jusqu’à 100 clients différents, en fonction des besoins en bande passante de chaque locataire.  
  
En tant que réflecteurs d’itinéraires, GW2 envoie des itinéraires de l’espace CA contoso au contrôleur de réseau, et GW3 envoie des itinéraires de l’espace CA Woodgrove au contrôleur de réseau.  
  
Le contrôleur de réseau pousse les stratégies de virtualisation de réseau Hyper-V vers les réseaux virtuels contoso et Woodgrove, ainsi que les stratégies RAS vers les passerelles RAS et les stratégies d’équilibrage de charge vers les multiplexeurs (multiplexeurs) qui sont configurés en tant qu’équilibrage de la charge logicielle. pool.  
  
## <a name="adding-new-tenants-and-customer-address-ca-space-ebgp-peering"></a><a name="bkmk_tenant"></a>Ajout de nouveaux locataires et de l’espace d’adressage du client (CA) eBGP appairage  
Quand vous signez un nouveau client et que vous ajoutez le client en tant que nouveau locataire dans votre centre de code, vous pouvez utiliser le processus suivant, dont la plupart sont effectuées automatiquement par les routeurs eBGP du contrôleur de réseau et de la passerelle RAS.  
  
1.  Approvisionnez un nouveau réseau virtuel et des charges de travail en fonction des exigences de votre locataire.  
  
2.  Si nécessaire, configurez la connectivité à distance entre le site client distant de l’entreprise et son réseau virtuel dans votre centre de. Lorsque vous déployez une connexion VPN de site à site pour le locataire, le contrôleur de réseau sélectionne automatiquement une machine virtuelle de passerelle RAS disponible dans le pool de passerelles disponibles et configure la connexion.  
  
3.  Lors de la configuration de la machine virtuelle de la passerelle RAS pour le nouveau locataire, le contrôleur de réseau configure également la passerelle RAS en tant que routeur BGP et la désigne comme un réflecteur d’itinéraire pour le locataire. Cela est vrai même dans les cas où la passerelle RAS fait office de passerelle, ou en tant que réflecteur de passerelle et de routage, pour d’autres locataires.  
  
4.  Selon que le routage de l’espace de l’autorité de certification est configuré pour utiliser des réseaux configurés statiquement ou le routage BGP dynamique, le contrôleur de réseau configure les itinéraires statiques correspondants, les voisins BGP ou les deux sur la machine virtuelle de la passerelle RAS et le réflecteur de routage.  
  
    > [!NOTE]  
    > -   Une fois que le contrôleur de réseau a configuré une passerelle RAS et un réflecteur d’itinéraire pour le locataire, chaque fois que le même locataire requiert une nouvelle connexion VPN de site à site, le contrôleur de réseau vérifie la capacité disponible sur cette machine virtuelle de passerelle RAS. Si la passerelle d’origine peut traiter la capacité requise, la nouvelle connexion réseau est également configurée sur la même machine virtuelle de passerelle RAS. Si la machine virtuelle de la passerelle RAS ne peut pas gérer la capacité supplémentaire, le contrôleur de réseau sélectionne une nouvelle machine virtuelle de passerelle RAS disponible et configure la nouvelle connexion sur celle-ci. Cette nouvelle machine virtuelle de passerelle RAS associée au locataire devient le client de réflecteur d’itinéraire du réflecteur d’itinéraire de la passerelle RAS du client d’origine.  
    > -   Étant donné que les pools de passerelle RAS se trouvent derrière des équilibrages de charge logicielle (SLBs), les adresses VPN de site à site des locataires utilisent chacune une adresse IP publique unique, appelée adresse IP virtuelle (VIP), qui est traduite par SLBs en adresse IP interne de centre de donnée, appelée adresse IP dynamique (DIP), pour une passerelle RAS qui achemine le trafic pour le locataire de l’entreprise. Ce mappage de l’adresse IP publique-privée par SLB garantit que les tunnels VPN de site à site sont correctement établis entre les sites d’entreprise et les passerelles RAS et les réflecteurs d’itinéraires du CSP.  
    >   
    >     Pour plus d’informations sur SLB, les adresses IP virtuelles et les adresses IP virtuelles, consultez [équilibrage &#40;de charge logiciel&#41; SLB pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Une fois que le tunnel VPN de site à site entre le site d’entreprise et la passerelle RAS du centre CSP est établi pour le nouveau locataire, les itinéraires statiques associés aux tunnels sont automatiquement approvisionnés sur les côtés entreprise et CSP du tunnel. .  
  
6.  Avec le routage BGP de l’espace de l’autorité de certification, l’homologation eBGP entre les sites d’entreprise et le réflecteur de l’itinéraire de la passerelle RAS du CSP est également établie.  
  
## <a name="route-synchronization-and-data-plane-routing"></a><a name="bkmk_route"></a>Synchronisation des itinéraires et routage du plan de données  
Une fois l’homologation eBGP établie entre les sites d’entreprise et le réflecteur de l’itinéraire de la passerelle RAS du CSP, le réflecteur d’itinéraire apprend tous les itinéraires de l’entreprise à l’aide du routage BGP dynamique. Le réflecteur d’itinéraire synchronise ces itinéraires entre tous les clients de réflecteur d’itinéraire afin qu’ils soient tous configurés avec le même ensemble d’itinéraires.  
  
Le réflecteur de routage met également à jour ces itinéraires consolidés, à l’aide de la synchronisation d’itinéraire, au contrôleur de réseau. Le contrôleur de réseau convertit ensuite les itinéraires en stratégies de virtualisation de réseau Hyper-V et configure le réseau de l’infrastructure pour garantir que le routage du chemin des données de bout en bout est approvisionné. Ce processus rend le réseau virtuel du locataire accessible à partir des sites de l’entreprise locataire.  
  
Pour le routage du plan de données, les paquets qui atteignent les machines virtuelles de la passerelle RAS sont directement routés vers le réseau virtuel du locataire, car les itinéraires requis sont désormais disponibles avec toutes les machines virtuelles de la passerelle RAS participante.  
  
De même, avec les stratégies de virtualisation de réseau Hyper-V en place, le réseau virtuel du locataire achemine les paquets directement vers les machines virtuelles de la passerelle RAS (sans avoir à connaître le réflecteur d’itinéraire), puis aux sites d’entreprise sur les tunnels VPN de site à site. .  
  
De plus,. le retour du trafic du réseau virtuel client au site d’entreprise locataire distant ignore le SLBs, un processus appelé DSR (direct Server Return).  
  
## <a name="how-network-controller-responds-to-ras-gateway-and-route-reflector-failover"></a><a name="bkmk_failover"></a>Comment le contrôleur de réseau répond à la passerelle RAS et au basculement de réflecteur d’itinéraire  
Voici deux scénarios possibles de basculement : un pour les clients de réflecteur de passerelle RAS et un pour les réflecteurs d’itinéraires de passerelle RAS, y compris les informations sur la façon dont le contrôleur de réseau gère le basculement pour les machines virtuelles dans les deux configurations.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Échec de la machine virtuelle d’une passerelle RAS client de réflecteur d’itinéraire BGP  
Le contrôleur de réseau effectue les actions suivantes lorsqu’un client de réflecteur d’itinéraire de passerelle RAS échoue.  
  
> [!NOTE]  
> Lorsqu’une passerelle RAS n’est pas un réflecteur d’itinéraire pour l’infrastructure BGP d’un locataire, il s’agit d’un client de réflecteur d’itinéraire dans l’infrastructure BGP du locataire.  
  
-   Le contrôleur de réseau sélectionne une machine virtuelle de passerelle RAS disponible en attente et configure la nouvelle machine virtuelle de la passerelle RAS avec la configuration de la machine virtuelle de la passerelle RAS qui a échoué.  
  
-   Le contrôleur de réseau met à jour la configuration SLB correspondante pour s’assurer que les tunnels VPN de site à site des sites clients vers la passerelle RAS défaillante sont correctement établis avec la nouvelle passerelle RAS.  
  
-   Le contrôleur de réseau configure le client de réflecteur d’itinéraires BGP sur la nouvelle passerelle.  
  
-   Le contrôleur de réseau configure le nouveau client de réflecteur d’itinéraire BGP de la passerelle RAS comme étant actif. La passerelle RAS commence immédiatement l’homologation avec le réflecteur d’itinéraire du locataire pour partager les informations de routage et pour activer l’homologation eBGP pour le site d’entreprise correspondant.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Échec de la machine virtuelle pour une passerelle RAS réflecteur d’itinéraire BGP  
Le contrôleur de réseau effectue les actions suivantes lorsqu’un réflecteur d’itinéraire BGP de la passerelle RAS échoue.  
  
-   Le contrôleur de réseau sélectionne une machine virtuelle de passerelle RAS disponible en attente et configure la nouvelle machine virtuelle de la passerelle RAS avec la configuration de la machine virtuelle de la passerelle RAS qui a échoué.  
  
-   Le contrôleur de réseau configure le réflecteur d’itinéraire sur la nouvelle machine virtuelle de la passerelle RAS et affecte à la nouvelle machine virtuelle la même adresse IP que celle qui a été utilisée par la machine virtuelle défaillante, fournissant ainsi l’intégrité de l’itinéraire en dépit de l’échec de la machine virtuelle.  
  
-   Le contrôleur de réseau met à jour la configuration SLB correspondante pour s’assurer que les tunnels VPN de site à site des sites clients vers la passerelle RAS défaillante sont correctement établis avec la nouvelle passerelle RAS.  
  
-   Le contrôleur de réseau configure la nouvelle machine virtuelle réflecteur de l’itinéraire BGP de la passerelle RAS comme active.  
  
-   Le réflecteur d’itinéraire devient immédiatement actif. Le tunnel VPN de site à site vers l’entreprise est établi et le réflecteur d’itinéraire utilise l’homologation eBGP et échange des itinéraires avec les routeurs de site d’entreprise.  
  
-   Après la sélection de l’itinéraire BGP, le réflecteur d’itinéraire BGP de la passerelle RAS met à jour les clients de réflecteur d’itinéraire du locataire dans le centre de données et synchronise les itinéraires avec le contrôleur de réseau, ce qui rend le chemin de données de bout en bout disponible pour le trafic de locataire.  
  
## <a name="advantages-of-using-new-ras-gateway-features"></a><a name="bkmk_advantages"></a>Avantages de l’utilisation de nouvelles fonctionnalités de passerelle RAS  
Voici quelques-uns des avantages de l’utilisation de ces nouvelles fonctionnalités de passerelle RAS lors de la conception de votre déploiement de passerelle RAS.  
  
**Évolutivité de la passerelle RAS**  
  
Étant donné que vous pouvez ajouter autant de machines virtuelles de passerelle RAS que nécessaire pour les pools de passerelle RAS, vous pouvez facilement mettre à l’échelle votre déploiement de passerelle RAS pour optimiser les performances et la capacité. Lorsque vous ajoutez des machines virtuelles à un pool, vous pouvez configurer ces passerelles RAS avec des connexions VPN de site à site de tout type (IKEv2, L3, GRE), éliminant ainsi les goulots d’étranglement de capacité sans temps d’indisponibilité.  
  
**Gestion simplifiée des passerelles de site d’entreprise**  
  
Lorsque votre locataire dispose de plusieurs sites d’entreprise, le locataire peut configurer tous les sites avec une adresse IP VPN de site à site distante et une seule adresse IP de voisin distant : votre fournisseur de services de chiffrement du centre de donnée du service d’accès au client de la passerelle RAS pour ce locataire. Cela simplifie la gestion de la passerelle pour vos locataires.  
  
**Correction rapide de l’échec de la passerelle**  
  
Pour garantir une réponse rapide au basculement, vous pouvez configurer le délai d’attente du paramètre KeepAlive BGP entre les itinéraires Edge et le routeur de contrôle sur un court intervalle de temps, par exemple, inférieur ou égal à dix secondes. Avec cet intervalle Keep Alive courts, si un routeur Edge BGP de passerelle RAS échoue, l’échec est détecté rapidement et le contrôleur de réseau suit les étapes décrites dans les sections précédentes. Cet avantage peut réduire la nécessité d’un protocole de détection de défaillance distinct, tel que le protocole de détection de transfert bidirectionnel (BFD).  
  
 
  


