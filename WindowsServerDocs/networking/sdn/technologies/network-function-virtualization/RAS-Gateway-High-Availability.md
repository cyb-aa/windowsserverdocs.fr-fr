---
title: Haute disponibilité de la passerelle du serveur d’accès à distance
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les configurations à haute disponibilité pour la passerelle mutualisée RAS pour la mise en réseau SDN (Software Defined Networking) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7d9c37629c0e0d9964554ba90887aa45f74a330a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355609"
---
# <a name="ras-gateway-high-availability"></a>Haute disponibilité de la passerelle du serveur d’accès à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les configurations à haute disponibilité pour la passerelle mutualisée RAS pour la mise en réseau SDN (Software Defined Networking).  
  
Cette rubrique contient les sections suivantes.  
  
-   [Vue d’ensemble de la passerelle RAS](#bkmk_overview)  
  
-   [Vue d’ensemble des pools de passerelle](#bkmk_pools)  
  
-   [Présentation du déploiement de la passerelle RAS](#bkmk_deployment)  
  
-   [Intégration de la passerelle RAS avec le contrôleur de réseau](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Vue d’ensemble de la passerelle RAS  
Si votre organisation est un fournisseur de services Cloud (CSP) ou une entreprise avec plusieurs locataires, vous pouvez déployer la passerelle RAS en mode multi-locataire pour assurer le routage du trafic réseau vers et depuis des réseaux virtuels et physiques, y compris Internet.  
  
Vous pouvez déployer la passerelle RAS en mode multi-locataire en tant que passerelle de périphérie pour acheminer le trafic réseau client des clients vers les ressources et les réseaux virtuels locataires.  
  
Lorsque vous déployez plusieurs instances de machines virtuelles de passerelle RAS qui fournissent une haute disponibilité et un basculement, vous déployez un pool de passerelles. Dans Windows Server 2012 R2, toutes les machines virtuelles de la passerelle ont formé un seul pool, ce qui rend un peu difficile la séparation logique du déploiement de la passerelle.  La passerelle Windows Server 2012 R2 offrait un déploiement de redondance 1:1 pour les machines virtuelles de passerelle, ce qui a entraîné une sous-utilisation de la capacité disponible pour les connexions VPN de site à site (S2S).  
  
Ce problème est résolu dans Windows Server 2016, qui fournit plusieurs pools de passerelles, qui peuvent être de différents types pour la séparation logique. Le nouveau mode de redondance M + N permet une configuration de basculement plus efficace.  
  
Pour plus d’informations sur la passerelle RAS, consultez [passerelle RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Vue d’ensemble des pools de passerelle  
Dans Windows Server 2016, vous pouvez déployer des passerelles dans un ou plusieurs pools.  
  
L’illustration suivante montre les différents types de pools de passerelle qui fournissent le routage du trafic entre les réseaux virtuels.  
  
![Pools de passerelle RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Chaque pool a les propriétés suivantes.  
  
-   Chaque pool est M + N redondant. Cela signifie qu’un nombre « m » de machines virtuelles de passerelle actives est sauvegardé par un nombre « N » de machines virtuelles de passerelle de secours. La valeur de N (passerelles de secours) est toujours inférieure ou égale à M (passerelles actives).  
  
-   Un pool peut effectuer l’une des fonctions de passerelle individuelles, protocole IKE (Internet Key Exchange) version 2 (IKEv2) S2S, couche 3 (L3) et l’encapsulation générique de routage (GRE), ou le pool peut effectuer toutes ces fonctions.  
  
-   Vous pouvez affecter une adresse IP publique unique à tous les pools ou à un sous-ensemble de pools. Cela réduit considérablement le nombre d’adresses IP publiques que vous devez utiliser, car il est possible que tous les locataires se connectent au Cloud sur une seule adresse IP. La section ci-dessous relative à la haute disponibilité et à l’équilibrage de charge décrit la façon dont cela fonctionne.  
  
-   Vous pouvez facilement mettre à l’échelle un pool de passerelle en ajoutant ou en supprimant des machines virtuelles de passerelle dans le pool. La suppression ou l’ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer des pools complets de passerelles.  
  
-   Les connexions d’un seul locataire peuvent se terminer sur plusieurs pools et plusieurs passerelles dans un pool. Toutefois, si un client a des connexions qui se terminent dans un pool de passerelle de type **tous** , il ne peut pas s’abonner à d’autres pools de passerelle de type **tout** ou type.  
  
Les pools de passerelle offrent également la flexibilité nécessaire pour activer des scénarios supplémentaires :  
  
-   Pools à locataire unique : vous pouvez créer un pool à utiliser par un locataire.  
  
-   Si vous vendez des services Cloud par le biais de canaux partenaires (revendeurs), vous pouvez créer des ensembles distincts de pools pour chaque revendeur.  
  
-   Plusieurs pools peuvent fournir la même fonction de passerelle, mais des capacités différentes. Par exemple, vous pouvez créer un pool de passerelles qui prend en charge les connexions IKEv2 S2S à débit élevé et à faible débit.  
  
## <a name="bkmk_deployment"></a>Présentation du déploiement de la passerelle RAS  
L’illustration suivante montre un déploiement du fournisseur de services Cloud (CSP) classique de la passerelle RAS.  
  
![Présentation du déploiement de la passerelle RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Avec ce type de déploiement, les pools de passerelle sont déployés derrière un logiciel Load Balancer (SLB), ce qui permet au CSP d’attribuer une adresse IP publique unique pour l’ensemble du déploiement. Plusieurs connexions de passerelle d’un locataire peuvent se terminer sur plusieurs pools de passerelles, et également sur plusieurs passerelles au sein d’un pool. Cela est illustré par les connexions IKEv2 S2S dans le diagramme ci-dessus, mais elles s’appliquent également aux autres fonctions de la passerelle, telles que les passerelles L3 et GRE.  
  
Dans l’illustration, l’appareil BGP MT est une passerelle mutualisée RAS avec BGP. Le protocole BGP multi-locataire est utilisé pour le routage dynamique. Le routage d’un locataire est centralisé-un point unique, appelé Reflector de route, gère l’homologation BGP pour tous les sites locataires. Le RR est distribué sur toutes les passerelles d’un pool. Cela aboutit à une configuration dans laquelle les connexions d’un locataire (chemin de données) se terminent sur plusieurs passerelles, mais le RR du locataire (chemin de contrôle du point d’homologation BGP) n’est sur qu’une seule des passerelles.  
  
Le routeur BGP est séparé dans le diagramme pour représenter ce concept de routage centralisé. L’implémentation BGP de la passerelle fournit également le routage de transit, qui permet au Cloud d’agir en tant que point de transit pour le routage entre deux sites locataires. Ces fonctionnalités BGP s’appliquent à toutes les fonctions de passerelle.  
  
## <a name="bkmk_integration"></a>Intégration de la passerelle RAS avec le contrôleur de réseau  
La passerelle RAS est entièrement intégrée au contrôleur de réseau dans Windows Server 2016. Lorsque la passerelle RAS et le contrôleur de réseau sont déployés, le contrôleur de réseau effectue les fonctions suivantes.  
  
-   Déploiement des pools de passerelle  
  
-   Configuration des connexions client sur chaque passerelle  
  
-   Basculement du trafic réseau vers une passerelle de secours en cas de défaillance de la passerelle  
  
Les sections suivantes fournissent des informations détaillées sur la passerelle RAS et le contrôleur de réseau.  
  
-   [Approvisionnement et équilibrage de la charge des connexions de passerelle (IKEv2, L3 et GRE)](#bkmk_provisioning)  
  
-   [Haute disponibilité pour IKEv2 S2S](#bkmk_ike)  
  
-   [Haute disponibilité pour GRE](#bkmk_gre)  
  
-   [Haute disponibilité pour les passerelles de transfert L3](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Approvisionnement et équilibrage de la charge des connexions de passerelle (IKEv2, L3 et GRE)  
Lorsqu’un client demande une connexion de passerelle, la demande est envoyée au contrôleur de réseau. Le contrôleur de réseau est configuré avec des informations sur tous les pools de passerelle, y compris la capacité de chaque pool et de chaque passerelle dans chaque pool. Le contrôleur de réseau sélectionne le pool et la passerelle appropriés pour la connexion. Cette sélection est basée sur la bande passante requise pour la connexion. Le contrôleur de réseau utilise un algorithme « meilleur ajustement » pour choisir efficacement les connexions dans un pool. Le point d’appairage BGP pour la connexion est également désigné à ce moment-là s’il s’agit de la première connexion du locataire.  
  
Une fois que le contrôleur de réseau a sélectionné une passerelle RAS pour la connexion, le contrôleur de réseau configure la configuration nécessaire pour la connexion sur la passerelle. Si la connexion est une connexion IKEv2 S2S, le contrôleur de réseau provisionne également une règle de traduction d’adresses réseau (NAT) sur le pool SLB ; Cette règle NAT sur le pool SLB dirige les demandes de connexion du client vers la passerelle désignée. Les locataires sont différenciés par l’adresse IP source, qui est censée être unique.  
  
> [!NOTE]  
> Les connexions L3 et GRE contournent SLB et se connectent directement à la passerelle RAS désignée.  Ces connexions requièrent que le routeur de point de terminaison distant (ou un autre périphérique tiers) soit correctement configuré pour se connecter à la passerelle RAS.  
  
Si le routage BGP est activé pour la connexion, l’homologation BGP est lancée par la passerelle RAS et les itinéraires sont échangés entre les passerelles locales et Cloud. Les itinéraires appris par BGP (ou qui sont des itinéraires configurés statiquement si le protocole BGP n’est pas utilisé) sont envoyés au contrôleur de réseau. Le contrôleur de réseau raccorde ensuite les itinéraires aux hôtes Hyper-V sur lesquels les machines virtuelles clientes sont installées. À ce stade, le trafic du client peut être acheminé vers le site local approprié. Le contrôleur de réseau crée également des stratégies de virtualisation de réseau Hyper-V associées qui spécifient des emplacements de passerelle et les monte sur les hôtes Hyper-V.  
  
### <a name="bkmk_ike"></a>Haute disponibilité pour IKEv2 S2S  
Une passerelle RAS dans un pool se compose des connexions et de l’homologation BGP de différents locataires. Chaque pool a des passerelles actives et des passerelles de secours « N ».  
  
Le contrôleur de réseau gère l’échec des passerelles de la manière suivante.  
  
-   Le contrôleur de réseau effectue un test ping permanent des passerelles dans tous les pools et peut détecter une passerelle qui a échoué ou qui échoue. Le contrôleur de réseau peut détecter les types suivants d’échecs de passerelle RAS.  
  
    -   Échec de la machine virtuelle de la passerelle RAS  
  
    -   Échec de l’hôte Hyper-V sur lequel la passerelle RAS est en cours d’exécution  
  
    -   Échec du service de passerelle RAS  
  
    Le contrôleur de réseau stocke la configuration de toutes les passerelles actives déployées. La configuration se compose des paramètres de connexion et des paramètres de routage.  
  
-   En cas de défaillance d’une passerelle, cela a un impact sur les connexions client sur la passerelle, ainsi que sur les connexions client qui se trouvent sur d’autres passerelles, mais dont les RR résident sur la passerelle défaillante. Le temps d’indisponibilité des dernières connexions est inférieur au premier. Lorsque le contrôleur de réseau détecte une passerelle défaillante, il effectue les tâches suivantes.  
  
    -   Supprime les itinéraires des connexions impactées à partir des hôtes de calcul.  
  
    -   Supprime les stratégies de virtualisation de réseau Hyper-V sur ces ordinateurs hôtes.  
  
    -   Sélectionne une passerelle de secours, la convertit en passerelle active et configure la passerelle.  
  
    -   Modifie les mappages NAT sur le pool SLB pour pointer les connexions à la nouvelle passerelle.  
  
-   En même temps, à mesure que la configuration s’affiche sur la nouvelle passerelle active, les connexions IKEv2 et l’homologation BGP sont rétablies. Les connexions et l’homologation BGP peuvent être initiées par la passerelle Cloud ou la passerelle locale. Les passerelles actualisent leurs itinéraires et les envoient au contrôleur de réseau. Une fois que le contrôleur de réseau a appris les nouveaux itinéraires découverts par les passerelles, le contrôleur de réseau envoie les itinéraires et les stratégies de virtualisation de réseau Hyper-V associées aux hôtes Hyper-V où résident les machines virtuelles des locataires ayant subi une défaillance. Cette activité du contrôleur de réseau est semblable à celle d’une nouvelle configuration de connexion, mais elle a lieu à une plus grande échelle.  
  
### <a name="bkmk_gre"></a>Haute disponibilité pour GRE  
Processus de réponse du basculement de la passerelle RAS par le contrôleur de réseau, y compris la détection des défaillances, la copie de la connexion et de la configuration de routage vers la passerelle de secours, le basculement du routage BGP/statique des connexions concernées (y compris le retrait et réactivation des itinéraires sur les hôtes de calcul et la réhomologation BGP) et reconfiguration des stratégies de virtualisation de réseau Hyper-V sur les hôtes de calcul-est le même pour les passerelles et les connexions GRE. La réinitialisation des connexions GRE se produit différemment, mais la solution de haute disponibilité pour GRE a des exigences supplémentaires.  
  
![Haute disponibilité pour GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
Au moment du déploiement de la passerelle, chaque machine virtuelle de passerelle RAS reçoit une adresse IP dynamique (DIP). En outre, chaque machine virtuelle de passerelle reçoit également une adresse IP virtuelle (VIP) pour la haute disponibilité GRE. Les adresses IP virtuelles sont affectées uniquement aux passerelles dans les pools qui peuvent accepter des connexions GRE, et non à des pools non-GRE. Les adresses IP virtuelles attribuées sont publiées dans les commutateurs de rack (TDR) à l’aide du protocole BGP, qui publie ensuite les adresses IP virtuelles dans le réseau physique du Cloud. Cela rend les passerelles accessibles à partir des routeurs distants ou des appareils tiers où réside l’autre extrémité de la connexion GRE. Cette homologation BGP est différente de l’homologation BGP au niveau du locataire pour l’échange d’itinéraires de locataire.  
  
Au moment de l’approvisionnement de la connexion GRE, le contrôleur de réseau sélectionne une passerelle, configure un point de terminaison GRE sur la passerelle sélectionnée et retourne l’adresse IP virtuelle de la passerelle attribuée. Cette adresse IP virtuelle est ensuite configurée comme adresse de tunnel GRE de destination sur le routeur distant.  
  
En cas de défaillance d’une passerelle, le contrôleur de réseau copie l’adresse IP virtuelle de la passerelle défaillante et d’autres données de configuration sur la passerelle de secours. Lorsque la passerelle de secours devient active, elle publie l’adresse IP virtuelle sur son commutateur de présence, puis sur le réseau physique. Les routeurs distants continuent de connecter les tunnels GRE à la même adresse IP virtuelle et l’infrastructure de routage garantit que les paquets sont acheminés vers la nouvelle passerelle active.  
  
### <a name="bkmk_l3"></a>Haute disponibilité pour les passerelles de transfert L3  
Une passerelle de transfert L3 de virtualisation de réseau Hyper-V est un pont entre l’infrastructure physique du centre de datacenter et l’infrastructure virtualisée dans le Cloud de virtualisation de réseau Hyper-V. Sur une passerelle de transfert L3 mutualisée, chaque locataire utilise son propre réseau logique avec balisage VLAN pour la connectivité avec le réseau physique du locataire.  
  
Lorsqu’un nouveau locataire crée une nouvelle passerelle L3, la passerelle du contrôleur de réseau Service Manager sélectionne une machine virtuelle de passerelle disponible et configure une nouvelle interface de locataire avec une adresse IP d’espace d’adresse client (CA) à haut niveau de disponibilité (à partir du réseau logique de réseau local virtuel marqué du locataire ). L’adresse IP est utilisée comme adresse IP de l’homologue sur la passerelle distante (réseau physique) et est le tronçon suivant pour atteindre le réseau de virtualisation de réseau Hyper-V du locataire.  
  
Contrairement aux connexions réseau IPsec ou GRE, le commutateur TDR n’apprend pas le réseau VLAN marqué de manière dynamique du locataire. Le routage du réseau avec marquage VLAN du locataire doit être configuré sur le commutateur TDR et tous les commutateurs et routeurs intermédiaires entre l’infrastructure physique et la passerelle pour garantir une connectivité de bout en bout.  Voici un exemple de configuration de réseau virtuel CSP, comme illustré dans l’illustration ci-dessous.  
  
|Réseau|Sous-réseau|ID du réseau local virtuel|Passerelle par défaut|  
|-----------|----------|-----------|-------------------|  
|Réseau logique contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Réseau logique de Woodgrove L3|10.127.134.0/24|1002|10.127.134.1|  
  
Voici quelques exemples de configurations de passerelle de locataire, comme illustré dans l’illustration ci-dessous.  
  
|Nom du locataire|Adresse IP de la passerelle L3|ID du réseau local virtuel|Adresse IP de l’homologue|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Avait|10.127.134.60|1002|10.127.134.65|  
  
Voici l’illustration de ces configurations dans un centre de donnés CSP.  
  
![Haute disponibilité pour les passerelles de transfert L3](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Les échecs de passerelle, la détection de défaillance et le processus de basculement de passerelle dans le contexte d’une passerelle de transfert L3 sont similaires aux processus pour les passerelles RAS IKEv2 et GRE. Les différences résident dans la façon dont les adresses IP externes sont gérées.  
  
Lorsque l’état de la machine virtuelle de la passerelle devient non intègre, le contrôleur de réseau sélectionne l’une des passerelles de secours dans le pool et reconfigure les connexions réseau et le routage sur la passerelle de secours. Lors du déplacement des connexions, l’adresse IP de l’espace de certification haute disponibilité de la passerelle de transfert L3 est également déplacée vers la nouvelle machine virtuelle de passerelle, ainsi que l’adresse IP BGP de l’espace de l’autorité de certification du locataire.  
  
Étant donné que l’adresse IP d’homologation L3 est déplacée vers la nouvelle machine virtuelle de passerelle pendant le basculement, l’infrastructure physique distante peut à nouveau se connecter à cette adresse IP et atteindre ensuite la charge de travail de virtualisation de réseau Hyper-V. Pour le routage dynamique BGP, étant donné que l’adresse IP BGP de l’espace de l’autorité de certification est déplacée vers la nouvelle machine virtuelle de passerelle, le routeur BGP distant peut rétablir l’homologation et apprendre à nouveau tous les itinéraires de virtualisation de réseau Hyper-V.  
  
> [!NOTE]  
> Vous devez configurer séparément les commutateurs et tous les routeurs intermédiaires afin d’utiliser le réseau logique avec balisage VLAN pour la communication avec les locataires. En outre, le basculement L3 est limité uniquement aux racks configurés de cette façon. Pour cette raison, le pool de passerelles L3 doit être configuré avec soin et la configuration manuelle doit être effectuée séparément.  
  


