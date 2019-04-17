---
title: Architecture de déploiement de passerelle RAS
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur le déploiement de fournisseur de services Cloud (CSP) de la passerelle d’accès distant dans Windows Server2016, y compris les pools de passerelle, itinéraire catadioptres et le déploiement de plusieurs passerelles pour des clients individuels.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d46e4e91-ece0-41da-a812-af8ab153edc4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21b101e10dba3d3b9578d6804b4fd92fbbcd2167
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-deployment-architecture"></a>Architecture de déploiement de passerelle RAS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur le déploiement de fournisseur de services Cloud (CSP) de la passerelle, y compris les pools de passerelle, itinéraire catadioptres et le déploiement de plusieurs passerelles pour des clients individuels.  
  
Les sections suivantes fournissent de brèves vues d’ensemble de certaines des nouvelles fonctionnalités de cette passerelle afin que vous puissiez comprendre comment utiliser ces fonctionnalités dans la conception de votre déploiement de passerelle.  
  
En outre, un exemple de déploiement est fourni, y compris les informations sur le processus d’ajout de nouveaux clients, synchronisation de l’itinéraire et données plan routage, passerelle et basculement réflecteur d’itinéraire et plus encore.  
  
Cette rubrique contient les sections suivantes.  
  
-   [À l’aide des nouvelles fonctionnalités de passerelle RAS pour concevoir votre déploiement](#bkmk_new)  
  
-   [Exemple de déploiement](#bkmk_example)  
  
-   [Ajout de nouveaux clients et l’espace d’adressage (CA) client EBGP homologation](#bkmk_tenant)  
  
-   [Synchronisation de l’itinéraire et les données plan de routage](#bkmk_route)  
  
-   [Comment le contrôleur de réseau répond à la passerelle et le basculement de réflecteur d’itinéraire](#bkmk_failover)  
  
-   [Avantages de l’utilisation des nouvelles fonctionnalités de passerelle d’accès distant](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>À l’aide des nouvelles fonctionnalités de passerelle RAS pour concevoir votre déploiement  
Cette passerelle inclut plusieurs nouvelles fonctionnalités qui changent et améliorent le mode dans lequel vous déployez votre infrastructure de passerelle dans votre centre de données.  
  
### <a name="bgp-route-reflector"></a>Réflecteur d’itinéraire BGP  
La fonctionnalité de réflecteur d’itinéraire de protocole BGP (Border Gateway) est désormais inclus avec la passerelle et offre une alternative à la topologie de maille pleine BGP qui est normalement requise pour la synchronisation d’itinéraire entre des routeurs. Avec la synchronisation de maille pleine, tous les routeurs BGP doivent se connecter avec tous les autres routeurs dans la topologie de routage. Lorsque vous utilisez réflecteur d’itinéraire, toutefois, le réflecteur d’itinéraire est le seul routeur qui se connecte avec tous les autres routeurs, appelés réflecteur d’itinéraire BGP clients, ce qui simplifie la synchronisation de l’itinéraire et réduire le trafic réseau. Le réflecteur d’itinéraire apprend les itinéraires de tous les calcule des itinéraires meilleures et redistribué par les itinéraires meilleures à ses clients BGP.  
  
Pour plus d’informations, voir [Nouveautés dans cette passerelle](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Pools de passerelle  
Dans Windows Server2016, vous pouvez créer plusieurs pools de passerelle de types différents. Passerelle pools contiennent le nombre d’instances de la passerelle et acheminent le trafic réseau entre les réseaux physiques et virtuels.  
  
Pour plus d’informations, voir [Nouveautés dans cette passerelle](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [RAS passerelle haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Extensibilité de Pool de passerelle  
Vous pouvez adapter facilement un pool de passerelle vers le haut ou vers le bas en ajoutant ou supprimant des ordinateurs virtuels de passerelle dans le pool. Retrait ou l’ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer les pools d’ensemble des passerelles.  
  
Pour plus d’informations, voir [Nouveautés dans cette passerelle](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [RAS passerelle haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Redondance de Pool de passerelle M + N  
Chaque pool de passerelle est redondant M + N. Cela signifie qu’un suis «nombre d’ordinateurs virtuels passerelle active est sauvegardé par un nombre «n» d’ordinateurs virtuels de passerelle en attente. M + N redondance vous offre davantage de flexibilité pour déterminer le niveau de fiabilité dont vous avez besoin lorsque vous déployez la passerelle.  
  
Pour plus d’informations, voir [Nouveautés dans cette passerelle](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [RAS passerelle haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Exemple de déploiement  
L’illustration suivante fournit un exemple avec eBGP homologation sur les connexions VPN de site à site configurées entre deux clients, Contoso et Woodgrove Bank et le centre de données du fournisseur CSP Fabrikam.  
  
![eBGP homologation sur les VPN de site à site](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
Dans cet exemple, Contoso requiert la bande passante supplémentaire de passerelle, ce qui conduit à la décision de conception d’infrastructure passerelle pour arrêter le site de Contoso Los Angeles sur GW3 au lieu de cassettes numériques GW2. Pour cette raison, les connexions VPN Contoso à partir de différents sites se terminent dans le centre de données de fournisseur de services cryptographiques sur deux passerelles différents.  
  
Les deux de ces passerelles, cassettes numériques GW2 et GW3, ont été la première passerelles RAS configuré par le contrôleur de réseau lorsque le fournisseur CSP ajouté les locataires Contoso et de la Woodgrove Bank à leur infrastructure. Pour cette raison, ces deux passerelles sont configurés en tant qu’itinéraire réflecteurs pour ces clients correspondants (ou locataires). Cassettes numériques GW2 est le réflecteur d’itinéraire Contoso et GW3 est le réflecteur d’itinéraire de Woodgrove Bank - en plus d’être le point de terminaison pour la connexion VPN avec le site du siège social de Contoso Los Angeles passerelle CSP.  
  
> [!NOTE]  
> Une passerelle peut acheminer le trafic réseau virtuel et physique pour jusqu'à 100clients différents, selon les besoins de la bande passante de chaque client.  
  
En tant qu’itinéraire catadioptres, cassettes numériques GW2 envoie les itinéraires de l’espace d’autorité de certification de Contoso au contrôleur de réseau et GW3 envoie les itinéraires de l’espace d’autorité de certification de Woodgrove Bank au contrôleur de réseau.  
  
Contrôleur de réseau place des stratégies de virtualisation de réseau Hyper-V pour les réseaux virtuels Contoso et de la Woodgrove Bank, ainsi que les stratégies RAS pour les passerelles RAS et les stratégies à le multiplexeur (MUXes) qui est configurés comme un pool d’équilibrage de charge logicielle d’équilibrage de charge.  
  
## <a name="bkmk_tenant"></a>Ajout de nouveaux clients et l’espace d’adresse client (CA) eBGP Peering  
Lorsque vous connectez un nouveau client et ajoutez le client en tant qu’un nouveau locataire dans votre centre de données, vous pouvez utiliser le processus suivant, dont la plupart est réalisée automatiquement par le contrôleur de réseau et passerelle routeurs eBGP.  
  
1.  Configurer un nouveau réseau virtuel et des charges de travail en fonction des besoins de votre client.  
  
2.  Si nécessaire, configurez la connectivité à distance entre le site d’entreprise clients distants et leur réseau virtuel dans votre centre de données. Lorsque vous déployez une connexion VPN de site à site pour le client, le contrôleur de réseau sélectionne un ordinateur virtuel de passerelle RAS disponibles à partir du pool disponible passerelle automatiquement et configure la connexion.  
  
3.  Lors de la configuration de l’ordinateur virtuel de passerelle RAS pour le nouveau locataire, contrôleur de réseau configure la passerelle RAS comme un routeur BGP et il désigne comme le réflecteur d’itinéraire pour le client. Cela vaut même dans les cas où la passerelle sert en tant que passerelle ou en tant que passerelle et réflecteur d’itinéraire, pour les autres clients.  
  
4.  Selon que l’autorité de certification espace routage est configuré pour les réseaux utilisation configurée de manière statique ou dynamique le routage BGP, contrôleur de réseau configure les correspondants, voisins BGP ou des itinéraires statiques à la fois sur l’ordinateur virtuel de passerelle RAS et réflecteur d’itinéraire.  
  
    > [!NOTE]  
    > -   Une fois que le contrôleur de réseau a configuré une passerelle et un réflecteur d’itinéraire pour le client, chaque fois que le même client nécessite une connexion VPN de site à nouveau, le contrôleur de réseau vérifie la capacité disponible sur cet ordinateur virtuel de passerelle RAS. Si la capacité requise peut répondre à la passerelle d’origine, la nouvelle connexion de réseau est également configurée sur l’ordinateur virtuel de passerelle RAS même. Si l’ordinateur virtuel de passerelle RAS ne peut pas gérer une capacité supplémentaire, le contrôleur de réseau sélectionne des ordinateurs virtuels passerelle RAS disponible un nouveau et configure la nouvelle connexion sur ce dernier. Cette nouvelle VM de passerelle RAS associés au client devient le client de réflecteur d’itinéraire du client d’origine réflecteur d’itinéraire passerelle RAS.  
    > -   Étant donné que les pools de passerelle sont situés derrière des équilibreurs de charge logicielle (SLBs), VPN de site à site des clients adresses chaque utilisation une seule adresse IP publique, appelée une adresse IP virtuelle (VIP), qui est traduite par les SLBs en une adresse IP interne de centre de données, appelée une adresse IP dynamique (DIP), pour une passerelle qui achemine le trafic pour le client d’entreprise. Ce mappage d’adresse IP publique pour privée par SLB garantit que les tunnels VPN de site à site sont correctement établies entre les sites d’entreprise et les passerelles CSP RAS et réflecteurs d’itinéraire.  
    >   
    >     Pour plus d’informations sur les différentes adresses IP et DIP, voir [l’équilibrage de charge logicielle et #40; sLB et #41; pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Une fois le tunnel VPN de site à site entre le site d’entreprise et le centre de données du fournisseur de services cryptographiques que cette passerelle est établie pour le nouveau locataire, les itinéraires statiques qui sont associés les tunnels sont automatiquement configurés sur l’entreprise et le fournisseur de services cryptographiques extrémités du tunnel.  
  
6.  Avec espace de l’autorité de certification BGP du routage, l’eBGP homologation entre les sites d’entreprise et le réflecteur d’itinéraire passerelle CSP RAS est également établi.  
  
## <a name="bkmk_route"></a>Synchronisation de l’itinéraire et les données plan de routage  
Une fois que l’homologation eBGP est établie entre les sites d’entreprise et le réflecteur d’itinéraire passerelle CSP RAS, le réflecteur d’itinéraire tous les itinéraires d’entreprise apprend à l’aide de routage BGP dynamique. Le réflecteur d’itinéraire synchronise ces itinéraires entre tous les clients réflecteur d’itinéraire afin qu’ils sont configurés avec le même jeu d’itinéraires.  
  
Réflecteur d’itinéraire met également à jour ces itinéraires consolidées, à l’aide de la synchronisation de l’itinéraire, au contrôleur de réseau. Contrôleur de réseau ensuite les itinéraires traduit par les stratégies de virtualisation de réseau Hyper-V et configure l’infrastructure réseau pour vous assurer que le routage de chemin d’accès des données End-to-End est mis en service. Ce processus permet de client réseau virtuel accessible à partir de l’entreprise client sites.  
  
Pour le routage du plan de données, les paquets d’atteindre les ordinateurs virtuels de passerelle RAS sont directement routés vers réseau virtuel du client, car les itinéraires requis sont désormais disponibles avec tous les ordinateurs virtuels de passerelle RAS participant au programme.  
  
De même, avec les stratégies de virtualisation de réseau Hyper-V en place, le réseau virtuel locataire achemine les paquets directement aux ordinateurs virtuels passerelle RAS (sans nécessiter de connaître le réflecteur d’itinéraire), puis pour les sites d’entreprise via les tunnels VPN de site à site.  
  
De plus,. Trafic de retour à partir du réseau virtuel du client vers le site d’entreprise de clients distants contourne les SLBs, un processus appelé Direct serveur renvoyer DSR ().  
  
## <a name="bkmk_failover"></a>Comment le contrôleur de réseau répond à la passerelle et le basculement de réflecteur d’itinéraire  
Voici deux scénarios possibles de basculement, l’un pour les clients de réflecteur d’itinéraire RAS passerelle - et l’autre pour RAS passerelle itinéraire réflecteurs notamment des informations sur comment le contrôleur de réseau gère le basculement pour les ordinateurs virtuels dans une configuration.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Échec de la machine virtuelle d’un Client de réflecteur d’itinéraire BGP passerelle RAS  
Contrôleur de réseau effectue les actions suivantes lorsqu’un client de réflecteur d’itinéraire RAS passerelle échoue.  
  
> [!NOTE]  
> Lorsqu’une passerelle n’est pas un réflecteur d’itinéraire pour l’infrastructure du protocole BGP d’un locataire, il est un client de réflecteur d’itinéraire dans l’infrastructure du protocole BGP du client.  
  
-   Contrôleur de réseau sélectionne des ordinateurs virtuels passerelle RAS mise en veille une disponibles et configure le nouvel ordinateur virtuel des passerelle RAS avec la configuration de l’ordinateur virtuel de passerelle de RAS a échoué.  
  
-   Contrôleur de réseau met à jour la configuration SLB correspondante pour vous assurer que les tunnels VPN de site à site à partir de sites client vers la passerelle RAS ayant échoué sont établis correctement avec la nouvelle passerelle RAS.  
  
-   Contrôleur de réseau configure le client de réflecteur d’itinéraire BGP sur la nouvelle passerelle.  
  
-   Contrôleur de réseau en tant que le nouveau client de réflecteur d’itinéraire RAS passerelle BGP active. La passerelle démarre immédiatement l’homologation avec réflecteur du client d’itinéraire pour partager des informations de routage et pour activer l’homologation eBGP pour le site d’entreprise correspondant.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Échec de l’ordinateur virtuel pour un réflecteur d’itinéraire BGP passerelle RAS  
Contrôleur de réseau effectue les actions suivantes lorsqu’un réflecteur d’itinéraire RAS passerelle BGP échoue.  
  
-   Contrôleur de réseau sélectionne des ordinateurs virtuels passerelle RAS mise en veille une disponibles et configure le nouvel ordinateur virtuel des passerelle RAS avec la configuration de l’ordinateur virtuel de passerelle de RAS a échoué.  
  
-   Contrôleur de réseau configure le réflecteur d’itinéraire sur le nouvel ordinateur virtuel de passerelle de RAS et affecte la nouvelle machine virtuelle de la même adresse IP qui était utilisée par l’ordinateur virtuel a échoué, offrant ainsi un itinéraire l’intégrité en dépit de l’échec de l’ordinateur virtuel.  
  
-   Contrôleur de réseau met à jour la configuration SLB correspondante pour vous assurer que les tunnels VPN de site à site à partir de sites client vers la passerelle RAS ayant échoué sont établis correctement avec la nouvelle passerelle RAS.  
  
-   Contrôleur de réseau en tant que nouvel RAS passerelle BGP itinéraire réflecteur d’ordinateur virtuel actif.  
  
-   Le réflecteur d’itinéraire immédiatement devient actif. Le tunnel VPN de site à l’entreprise est établi, et le réflecteur d’itinéraire utilise l’homologation eBGP et d’échanger des itinéraires avec les routeurs de site d’entreprise.  
  
-   Après la sélection de routage BGP, les mises à jour réflecteur d’itinéraire RAS passerelle BGP clients réflecteur d’itinéraire dans le centre de données pour les clients et synchronise les itinéraires avec le contrôleur de réseau, rendant le chemin d’accès des données End-to-End disponibles pour le trafic de client.  
  
## <a name="bkmk_advantages"></a>Avantages de l’utilisation des nouvelles fonctionnalités de passerelle d’accès distant  
Voici quelques-uns des avantages de l’utilisation de ces nouvelles fonctionnalités de la passerelle lors de la conception de votre déploiement de passerelle.  
  
**Évolutivité de la passerelle**  
  
Étant donné que vous pouvez ajouter des ordinateurs virtuels que nécessaire pour les pools de passerelle d’autant RAS passerelle, vous pouvez facilement adapter votre déploiement de passerelle pour optimiser les performances et la capacité. Lorsque vous ajoutez des ordinateurs virtuels à un pool, vous pouvez configurer ces passerelles RAS avec les connexions VPN de site de n’importe quel type (IKEv2, L3, GRE), en éliminant les goulots d’étranglement capacité avec aucun temps d’arrêt.  
  
**Gestion simplifiée d’entreprise de la passerelle de Site**  
  
Lorsque votre client possède plusieurs sites d’entreprise, le client peut configurer tous les sites avec une adresse IP VPN de site à site à distance et l’adresse IP unique voisin à distance - votre centre de données de fournisseur de services cryptographiques le VIP réflecteur d’itinéraire RAS passerelle BGP pour ce client. Cela simplifie la gestion de la passerelle pour vos clients.  
  
**Mise à jour rapide de défaillance de la passerelle**  
  
Pour garantir une réponse rapide de basculement, vous pouvez configurer le délai entre les itinéraires de bord et le routeur de contrôle pour un délai court, par exemple, inférieur ou égal à dix secondes Keepalive BGP. Avec cette courte intervalle garder actif, si un routeur de périphérie RAS passerelle BGP échoue, la défaillance est détectée rapidement et que le contrôleur de réseau suit les étapes fournies dans les sections précédentes. Cet avantage peut réduire la nécessité d’un protocole de détection échec distinct, comme protocole bidirectionnel transfert détection (BFD).  
  
 
  


