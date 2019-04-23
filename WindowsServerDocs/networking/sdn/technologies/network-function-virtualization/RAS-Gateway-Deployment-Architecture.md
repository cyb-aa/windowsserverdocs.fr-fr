---
title: Architecture de déploiement de la passerelle du serveur d’accès à distance
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur le déploiement du fournisseur de services Cloud (CSP) de passerelle RAS dans Windows Server 2016, notamment les pools de passerelle RAS, réflecteurs d’itinéraire et le déploiement de plusieurs passerelles pour les locataires individuels.
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
ms.openlocfilehash: a3895e25a2af0437deb9eebe4ad89b110cfc9f2b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870270"
---
# <a name="ras-gateway-deployment-architecture"></a>Architecture de déploiement de la passerelle du serveur d’accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur le déploiement du fournisseur de services Cloud (CSP) de passerelle RAS, notamment les pools de passerelle RAS, réflecteurs d’itinéraire et le déploiement de plusieurs passerelles pour les locataires individuels.  
  
Les sections suivantes fournissent de brèves vues d’ensemble de certaines des nouvelles fonctionnalités de passerelle RAS afin que vous puissiez comprendre comment utiliser ces fonctionnalités dans la conception de votre déploiement de passerelle.  
  
En outre, un exemple de déploiement est fourni, y compris des informations sur le processus d’ajout de nouveaux locataires, synchronisation de l’itinéraire et routage de plan de données, passerelle et basculement de réflecteur d’itinéraire et bien plus encore.  
  
Cette rubrique contient les sections suivantes.  
  
-   [À l’aide des nouvelles fonctionnalités de passerelle RAS pour concevoir votre déploiement](#bkmk_new)  
  
-   [Exemple de déploiement](#bkmk_example)  
  
-   [Ajout de nouveaux locataires et l’espace d’adressage (CA) de client EBGP homologation](#bkmk_tenant)  
  
-   [Synchronisation de l’itinéraire et les données de plan de routage](#bkmk_route)  
  
-   [Comment le contrôleur de réseau répond à la passerelle RAS et de basculement de réflecteur d’itinéraire](#bkmk_failover)  
  
-   [Avantages de l’utilisation des nouvelles fonctionnalités de passerelle RAS](#bkmk_advantages)  
  
## <a name="bkmk_new"></a>À l’aide des nouvelles fonctionnalités de passerelle RAS pour concevoir votre déploiement  
Passerelle RAS inclut plusieurs nouvelles fonctionnalités qui changent et améliorent la façon dont dans lesquels vous déployez votre infrastructure de passerelle dans votre centre de données.  
  
### <a name="bgp-route-reflector"></a>Réflecteur d’itinéraire BGP  
La fonctionnalité de réflecteur d’itinéraire de protocole BGP (Border Gateway) est désormais inclus avec la passerelle RAS et fournit une alternative à la topologie de maille pleine BGP est normalement requis pour la synchronisation de routage entre des routeurs. Avec la synchronisation de maillage complet, tous les routeurs BGP doivent se connecter tous les autres routeurs dans la topologie de routage. Lorsque vous utilisez réflecteur d’itinéraire, toutefois, le réflecteur d’itinéraire est le seul routeur qui se connecte avec tous les autres routeurs, appelés réflecteur d’itinéraire BGP clients, simplifiant ainsi la synchronisation de l’itinéraire et de réduire le trafic réseau. Le réflecteur d’itinéraire apprend les itinéraires de tous les calcule les itinéraires meilleures et redistribue les itinéraires meilleures à ses clients BGP.  
  
Pour plus d’informations, consultez [What ' s New in passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md).  
  
### <a name="bkmk_pools"></a>Pools de passerelles  
Dans Windows Server 2016, vous pouvez créer plusieurs pools de passerelle de types différents. Pools de passerelles contiennent de nombreuses instances de passerelle RAS et acheminent le trafic réseau entre les réseaux physiques et virtuels.  
  
Pour plus d’informations, consultez [What ' s New in passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [passerelle RAS haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_gps"></a>Évolutivité de Pool de passerelle  
Vous pouvez facilement augmenter un pool de passerelle vers le haut ou vers le bas en ajoutant ou supprimant des machines virtuelles de passerelle dans le pool. Suppression ou ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer des pools d’ensemble des passerelles.  
  
Pour plus d’informations, consultez [What ' s New in passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [passerelle RAS haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
### <a name="bkmk_m"></a>Redondance de Pool de passerelle M + N  
Chaque pool de passerelle est redondant M + N. Cela signifie qu’un suis ' nombre de machines virtuelles de passerelle active est sauvegardé par un nombre « n » des machines virtuelles de passerelle de secours. M + N redondance vous offre davantage de flexibilité dans la détermination du niveau de fiabilité dont vous avez besoin lorsque vous déployez la passerelle RAS.  
  
Pour plus d’informations, consultez [What ' s New in passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md) et [passerelle RAS haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_example"></a>Exemple de déploiement  
L’illustration suivante fournit un exemple avec homologation sur les connexions VPN de site à site configurées entre deux clients, Contoso et Woodgrove et le centre de données Fabrikam CSP eBGP.  
  
![eBGP homologation via un VPN de site à site](../../../media/RAS-Gateway-Deployment-Architecture/ras_gateway_architecture.png)  
  
Dans cet exemple, Contoso nécessite la bande passante de la passerelle supplémentaire, ce qui conduit à la décision de conception d’infrastructure passerelle pour arrêter le site Contoso Los Angeles sur GW3 au lieu de GW2. Pour cette raison, les connexions VPN Contoso à partir de différents sites se terminer dans le centre de données CSP sur deux différentes passerelles.  
  
Les deux de ces passerelles, GW2 et GW3, ont été la première passerelles RAS configuré par le contrôleur de réseau lorsque le CSP ajouté les clients de Contoso et Woodgrove à leur infrastructure. Pour cette raison, ces deux passerelles sont configurés en tant qu’itinéraire réflecteurs pour ces clients correspondants (ou locataires). GW2 est le réflecteur d’itinéraire de Contoso et GW3 est le réflecteur d’itinéraire Woodgrove - en plus d’être le point de terminaison de passerelle RAS du CSP pour la connexion VPN avec le site du siège social de Contoso Los Angeles.  
  
> [!NOTE]  
> Une passerelle RAS peut acheminer le trafic de réseau virtuel et physique pour cent jusqu'à des locataires différents, selon les besoins de bande passante de chaque client.  
  
En tant qu’itinéraire réflecteurs, GW2 envoie les itinéraires de l’espace d’autorité de certification de Contoso au contrôleur de réseau et GW3 envoie les itinéraires de l’espace d’autorité de certification Woodgrove au contrôleur de réseau.  
  
Contrôleur de réseau exécute un push des stratégies de virtualisation de réseau Hyper-V pour Contoso et Woodgrove réseaux virtuels, ainsi que les stratégies RAS au RAS passerelles et des stratégies pour les multiplexeurs (multiplexeurs) qui sont configurés comme un équilibrage de charge logicielle d’équilibrage de charge pool.  
  
## <a name="bkmk_tenant"></a>Ajout de nouveaux locataires et l’espace d’adresse client (CA) eBGP l’homologation  
Lorsque vous vous connecter à un nouveau client et ajoutez le client en tant que nouveau locataire dans votre centre de données, vous pouvez utiliser le processus suivant, une grande partie de ce qui est effectuée automatiquement par le contrôleur de réseau et passerelle RAS routeurs eBGP.  
  
1.  Configurez un nouveau réseau virtuel et les charges de travail selon les besoins de votre client.  
  
2.  Si nécessaire, configurez la connectivité à distance entre le site d’entreprise de clients distants et de leur réseau virtuel dans votre centre de données. Lorsque vous déployez une connexion VPN de site à site pour le locataire, contrôleur de réseau sélectionne un ordinateur virtuel de passerelle RAS disponible dans le pool de passerelle disponible automatiquement et configure la connexion.  
  
3.  Lors de la configuration de l’ordinateur virtuel de passerelle RAS pour le nouveau locataire, contrôleur de réseau configure la passerelle RAS comme un routeur BGP et désigne en tant que le réflecteur d’itinéraire pour le locataire. Cela est vrai même dans les circonstances où la passerelle RAS qui sert en tant que passerelle ou en tant que passerelle et réflecteur d’itinéraire, pour les autres clients.  
  
4.  Selon que l’autorité de certification espace routage est configuré pour les réseaux d’utilisation configurée de manière statique ou dynamique BGP, le contrôleur de réseau configure le correspondantes, voisins BGP ou des itinéraires statiques dans la machine virtuelle de passerelle RAS et le réflecteur d’itinéraire.  
  
    > [!NOTE]  
    > -   Une fois que le contrôleur de réseau a configuré une passerelle RAS et le réflecteur d’itinéraire pour le client, chaque fois que le même client nécessite une nouvelle connexion VPN de site à site, contrôleur de réseau vérifie la capacité disponible sur cet ordinateur virtuel de passerelle RAS. Si la passerelle d’origine peut traiter la capacité requise, la nouvelle connexion de réseau est également configurée sur la même machine virtuelle de passerelle de RAS. Si l’ordinateur virtuel de passerelle RAS ne peut pas gérer une capacité supplémentaire, le contrôleur de réseau sélectionne une machine virtuelle de passerelle RAS disponibles et configure la nouvelle connexion sur celui-ci. Cette nouvelle VM de passerelle RAS associé au locataire devient le client de réflecteur d’itinéraire du client d’origine réflecteur d’itinéraire passerelle RAS.  
    > -   Étant donné que les pools de passerelle RAS sont derrière des équilibreurs de charge de logiciels (SLBs), adresses VPN de site à site locataires chacun utilise une adresse IP publique unique, appelée une adresse IP virtuelle (VIP), qui est traduite par les SLBs une adresse IP interne de centre de données, appelé un adresse IP dynamique (DIP), pour une passerelle RAS qui achemine le trafic pour le locataire de l’entreprise. Ce mappage d’adresse IP publique-privée par SLB garantit que les tunnels VPN de site à site sont correctement établies entre les sites d’entreprise et les passerelles de RAS de CSP et les réflecteurs d’itinéraire.  
    >   
    >     Pour plus d’informations sur SLB, les adresses IP virtuelles et les adresses IP dynamiques, consultez [l’équilibrage de charge logiciel &#40;SLB&#41; pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
5.  Une fois le tunnel VPN de site à site entre le site d’entreprise et le centre de données CSP que passerelle RAS est établie pour le nouveau locataire, les itinéraires statiques qui sont associés les tunnels sont automatiquement configurés sur l’entreprise et le fournisseur de services cryptographiques extrémités du tunnel .  
  
6.  Avec l’espace de l’autorité de certification BGP du routage, l’homologation entre les sites d’entreprise et le réflecteur de routage de passerelle RAS CSP d’eBGP est également établi.  
  
## <a name="bkmk_route"></a>Synchronisation de l’itinéraire et les données de plan de routage  
Une fois que l’homologation eBGP est établie entre les sites d’entreprise et le réflecteur de routage de passerelle RAS CSP, le réflecteur d’itinéraire tous les itinéraires d’entreprise apprend en utilisant le routage dynamique BGP. Le réflecteur d’itinéraire synchronise ces itinéraires entre tous les clients de réflecteur d’itinéraire afin qu’ils sont configurés avec le même ensemble d’itinéraires.  
  
Réflecteur d’itinéraire met également à jour ces itinéraires consolidées, à l’aide de la synchronisation de l’itinéraire, au contrôleur de réseau. Contrôleur de réseau les itinéraires traduit par les stratégies de virtualisation de réseau Hyper-V, puis configure le réseau de l’infrastructure pour vous assurer que le chemin d’accès des données de bout en bout pour le routage est configuré. Ce processus fait du client réseau virtuel accessible à partir du client Enterprise sites.  
  
Pour le routage de plan de données, les paquets qui atteignent les ordinateurs virtuels de passerelle RAS sont directement routées vers réseau virtuel du locataire, étant donné que les itinéraires requis sont désormais disponibles avec tous les ordinateurs virtuels de passerelle RAS participantes.  
  
De même, avec les stratégies de virtualisation de réseau Hyper-V en place, le réseau virtuel locataire achemine les paquets directement vers les machines virtuelles à la passerelle RAS (sans nécessiter de connaître le réflecteur d’itinéraire), puis vers les sites d’entreprise sur les tunnels VPN de site à site .  
  
De plus,. le trafic de retour à partir du réseau virtuel du client vers le site d’entreprise à distance client ignore les SLBs, un processus appelé retour serveur Direct (DSR).  
  
## <a name="bkmk_failover"></a>Comment le contrôleur de réseau répond à la passerelle RAS et de basculement de réflecteur d’itinéraire  
Voici deux scénarios possibles de basculement, l’un pour les clients de Reflector de routage de passerelle RAS - et l’autre pour réflecteurs de routage de passerelle RAS, y compris des informations sur comment contrôleur de réseau gère le basculement des machines virtuelles dans une configuration.  
  
### <a name="vm-failure-of-a-ras-gateway-bgp-route-reflector-client"></a>Échec de la machine virtuelle d’un Client de réflecteur d’itinéraire BGP passerelle RAS  
Contrôleur de réseau effectue les actions suivantes en cas d’échec d’un client de réflecteur de routage de passerelle RAS.  
  
> [!NOTE]  
> Quand une passerelle RAS n’est pas un réflecteur d’itinéraire pour l’infrastructure BGP d’un client, il est un client de réflecteur d’itinéraire dans l’infrastructure BGP du locataire.  
  
-   Contrôleur de réseau sélectionne un disponible RAS passerelle machine virtuelle de secours et configure la nouvelle machine virtuelle des passerelle RAS avec la configuration de l’ordinateur virtuel de passerelle RAS ayant échoué.  
  
-   Contrôleur de réseau met à jour la configuration du SLB correspondant pour vous assurer que les tunnels VPN de site à site à partir de sites du client à la passerelle RAS ayant échoué sont correctement établies avec la nouvelle passerelle RAS.  
  
-   Contrôleur de réseau configure le client de réflecteur d’itinéraire BGP sur la nouvelle passerelle.  
  
-   Contrôleur de réseau configure le nouveau client de réflecteur d’itinéraire RAS passerelle BGP comme active. La passerelle RAS démarre immédiatement l’homologation avec réflecteur d’itinéraire du locataire pour partager des informations de routage et d’activer eBGP l’homologation pour le site d’entreprise correspondant.  
  
### <a name="vm-failure-for-a-ras-gateway-bgp-route-reflector"></a>Échec de la machine virtuelle pour un réflecteur d’itinéraire BGP passerelle RAS  
Contrôleur de réseau effectue les actions suivantes lorsqu’un réflecteur d’itinéraire BGP RAS passerelle échoue.  
  
-   Contrôleur de réseau sélectionne un disponible RAS passerelle machine virtuelle de secours et configure la nouvelle machine virtuelle des passerelle RAS avec la configuration de l’ordinateur virtuel de passerelle RAS ayant échoué.  
  
-   Contrôleur de réseau configure le réflecteur d’itinéraire sur la nouvelle machine virtuelle de passerelle de RAS et assigne la nouvelle machine virtuelle de la même adresse IP qui a été utilisée par la machine virtuelle ayant échouée, offrant ainsi l’intégrité d’itinéraire malgré le dysfonctionnement de la machine virtuelle.  
  
-   Contrôleur de réseau met à jour la configuration du SLB correspondant pour vous assurer que les tunnels VPN de site à site à partir de sites du client à la passerelle RAS ayant échoué sont correctement établies avec la nouvelle passerelle RAS.  
  
-   Contrôleur de réseau configure la nouvelle RAS passerelle BGP itinéraire Reflector machine virtuelle comme active.  
  
-   Le réflecteur d’itinéraire devient immédiatement actif. Le tunnel VPN de site à site à l’entreprise est établi, et que le réflecteur d’itinéraire utilise l’homologation eBGP et échange des itinéraires avec les routeurs de site d’entreprise.  
  
-   Après sélection d’itinéraire BGP, les mises à jour de réflecteur d’itinéraire RAS passerelle BGP clients réflecteur d’itinéraire dans le centre de données de locataire et synchronise les itinéraires avec le contrôleur de réseau, la mise à disposition le chemin d’accès des données de bout en bout pour le trafic de client.  
  
## <a name="bkmk_advantages"></a>Avantages de l’utilisation des nouvelles fonctionnalités de passerelle RAS  
Voici quelques-uns des avantages de l’utilisation de ces nouvelles fonctionnalités de la passerelle RAS lors de la conception de votre déploiement de passerelle RAS.  
  
**Évolutivité de la passerelle RAS**  
  
Étant donné que vous pouvez ajouter autant RAS passerelle de machines virtuelles que vous avez besoin pour les pools de passerelle RAS, vous pouvez facilement augmenter votre déploiement de passerelle RAS pour optimiser les performances et la capacité. Lorsque vous ajoutez des machines virtuelles à un pool, vous pouvez configurer ces passerelles RAS avec les connexions VPN site à site quelconque (IKEv2, L3, GRE), en éliminant les goulots d’étranglement capacité sans temps d’arrêt.  
  
**Gestion simplifiée de la passerelle de Site**  
  
Lorsque votre client possède plusieurs sites d’entreprise, le client peut configurer tous les sites avec une adresse IP du VPN de site à site à distance et une adresse IP de voisin à distance unique - votre centre de données CSP VIP de réflecteur d’itinéraire RAS passerelle BGP pour ce client. Cela simplifie la gestion de la passerelle pour vos clients.  
  
**Mise à jour rapide de l’échec de la passerelle**  
  
Pour garantir une réponse d’un basculement rapide, vous pouvez configurer le temps de paramètre BGP Keepalive entre les itinéraires edge et le routeur de contrôle à un court délai, inférieur ou égal à dix secondes. Avec ce court intervalle pendant lequel maintenir, si un routeur de périphérie BGP de la passerelle RAS échoue, la défaillance est détectée rapidement et contrôleur de réseau suit les étapes fournies dans les sections précédentes. Cet avantage peut réduire le besoin d’un protocole de détection de défaillance distincts, comme protocole de détection de transfert de bidirectionnel (BFD).  
  
 
  


