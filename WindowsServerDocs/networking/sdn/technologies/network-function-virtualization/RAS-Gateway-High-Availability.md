---
title: RAS passerelle haute disponibilité
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les configurations de haute disponibilité pour la passerelle mutualisée pour logiciel défini de mise en réseau (SDN) dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 34d826c9-65bc-401f-889d-cf84e12f0144
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 48794ff8312ca00eda25f6d8bdca9929fc47084f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-high-availability"></a>RAS passerelle haute disponibilité

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les configurations de haute disponibilité pour la passerelle mutualisée pour logiciel défini de mise en réseau (SDN).  
  
Cette rubrique contient les sections suivantes.  
  
-   [Vue d’ensemble de la passerelle RAS](#bkmk_overview)  
  
-   [Vue d’ensemble de Pools de passerelle](#bkmk_pools)  
  
-   [Vue d’ensemble du déploiement de passerelle RAS](#bkmk_deployment)  
  
-   [Intégration de la passerelle RAS avec le contrôleur de réseau](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Vue d’ensemble de la passerelle RAS  
Si votre organisation est un fournisseur de services Cloud (CSP) ou une entreprise avec plusieurs clients, vous pouvez déployer la passerelle en mode mutualisé pour fournir le routage du trafic réseau vers et à partir de réseaux virtuels et physiques, notamment Internet.  
  
Vous pouvez déployer cette passerelle en mode mutualisé comme une passerelle pour acheminer le trafic réseau de client client aux ressources et les réseaux virtuels pour les clients.  
  
Lorsque vous déployez plusieurs instances d’ordinateurs virtuels passerelle RAS qui fournissent une haute disponibilité et basculement, vous déployez un pool de passerelle. Dans Windows Server2012R2, la passerelle de tous les ordinateurs virtuels formé un pool unique, qui fait une séparation logique du déploiement de passerelle un peu difficiles.  Passerelle Windows Server2012R2 proposé un déploiement de la redondance 1:1 pour l’ordinateurs virtuels, ce qui entraînait des connexions VPN incomplète l’utilisation de la capacité disponible pour le site à site (S2S) de la passerelle.  
  
Ce problème est résolu dans Windows Server2016, qui fournit plusieurs Pools de passerelle - qui peut être de types différents pour une séparation logique. Le nouveau mode de redondance M + N permet une configuration de basculement plus efficace.  
  
Pour plus d’informations de vue d’ensemble sur la passerelle, voir [passerelle](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Vue d’ensemble de Pools de passerelle  
Dans Windows Server2016, vous pouvez déployer des passerelles dans un ou plusieurs pools.  
  
L’illustration suivante montre les différents types de pools de passerelle qui fournissent le routage du trafic entre les réseaux virtuels.  
  
![Pools de passerelle](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Chaque pool a les propriétés suivantes.  
  
-   Chaque pool est redondant M + N. Cela signifie qu’un suis «nombre d’ordinateurs virtuels passerelle active est sauvegardé par un nombre «n» d’ordinateurs virtuels de passerelle en attente. La valeur de N (mise en veille passerelles) est toujours inférieur ou égal à M (passerelles actives).  
  
-   Un pool peut effectuer l’une des fonctions de passerelle individuelle - Internet Key Exchange version2 (IKEv2) S2S, niveau 3 (L3) et Encapsulation GRE (Generic Routing) - ou le pool peut effectuer toutes ces fonctions.  
  
-   Vous pouvez affecter une adresse IP publique unique à tous les pools ou à un sous-ensemble des pools. Effectuant considérablement permet de réduire le nombre d’adresses IP publiques que vous devez utiliser, car il est possible d’avoir à tous les clients de se connecter sur le cloud sur une seule adresse IP. La section ci-dessous sur la haute disponibilité et l’équilibrage de charge décrit comment procéder.  
  
-   Vous pouvez adapter facilement un pool de passerelle vers le haut ou vers le bas en ajoutant ou supprimant des ordinateurs virtuels de passerelle dans le pool. Retrait ou l’ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer les pools d’ensemble des passerelles.  
  
-   Connexions d’un client unique peuvent se terminer sur plusieurs pools et les passerelles dans un pool. Toutefois, si un client dispose de connexions se terminant par un **tous les** type du pool de passerelle, il ne peut pas s’abonner à d’autres **tous les** type ou des pools de passerelle de type individuels.  
  
Pools de passerelle fournissent également la flexibilité nécessaire pour activer des scénarios supplémentaires:  
  
-   Pools mono-utilisateur - vous pouvez créer un pool pour une utilisation par un locataire.  
  
-   Si vous vendez des services cloud via des canaux de partenaire (revendeur), vous pouvez créer des jeux de pools distincts pour chaque revendeur.  
  
-   Plusieurs pools peuvent fournir la même fonction de passerelle, mais différentes capacités. Par exemple, vous pouvez créer un pool de passerelle qui prend en charge un débit élevé et les connexions de faible débit S2S IKEv2.  
  
## <a name="bkmk_deployment"></a>Vue d’ensemble du déploiement de passerelle RAS  
L’illustration suivante montre un déploiement classique de fournisseur de services Cloud (CSP) de la passerelle.  
  
![Vue d’ensemble du déploiement de passerelle RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Avec ce type de déploiement, les pools de passerelle sont déployés derrière un logiciel charge Équilibrage (SLB), ce qui permet le fournisseur CSP affecter une seule adresse IP publique pour le déploiement complet. Plusieurs connexions de la passerelle d’un client peuvent se terminer sur plusieurs pools de passerelle - ainsi que sur les passerelles multiples au sein d’un pool. Ceci est illustré par le biais de connexions IKEv2 S2S dans le diagramme ci-dessus, mais le même est applicable à d’autres fonctions de passerelle, tels que des passerelles L3 et GRE.  
  
Dans l’illustration, l’appareil MT BGP est une passerelle mutualisée avec protocole BGP. Architecture mutualisée BGP est utilisé pour le routage dynamique. Le routage pour un client est centralisé - un point unique, appelé le réflecteur d’itinéraire (RR), gère l’homologation BGP pour tous les sites client. L’enregistrement de ressource est réparti entre tous les passerelles dans un pool. Cela entraîne une configuration où les connexions d’un client (chemin d’accès de données) se terminent sur plusieurs passerelles, mais l’enregistrement de ressource pour le client (point homologation BGP - chemin d’accès de contrôle) est sur un seul des passerelles.  
  
Le routeur BGP est séparé du diagramme pour illustrer ce concept de routage centralisé. L’implémentation du protocole BGP passerelle fournit également des routage de transit, ce qui permet le cloud à agir comme un point de transit pour le routage entre les sites client deux. Ces fonctionnalités BGP sont applicables à toutes les fonctions de passerelle.  
  
## <a name="bkmk_integration"></a>Intégration de la passerelle RAS avec le contrôleur de réseau  
Cette passerelle est entièrement intégrée avec le contrôleur de réseau dans Windows Server2016. Lors de la passerelle et le contrôleur de réseau sont déployés, le contrôleur de réseau effectue les fonctions suivantes.  
  
-   Déploiement des pools de passerelle  
  
-   Configuration des connexions client sur chaque passerelle  
  
-   Le trafic réseau de commutation des flux à une passerelle de mise en veille en cas d’échec de la passerelle  
  
Les sections suivantes fournissent des informations détaillées sur la passerelle et le contrôleur de réseau.  
  
-   [Configuration et l’équilibrage de charge de connexions passerelle (IKEv2, L3 et GRE)](#bkmk_provisioning)  
  
-   [Haute disponibilité pour IKEv2 S2S](#bkmk_ike)  
  
-   [Haute disponibilité pour GRE](#bkmk_gre)  
  
-   [Haute disponibilité pour L3 passerelles de transfert](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Configuration et l’équilibrage de charge de connexions passerelle (IKEv2, L3 et GRE)  
Lorsqu’un client demande une connexion à la passerelle, la demande est envoyée au contrôleur de réseau. Contrôleur de réseau est configuré avec des informations sur tous les pools de passerelle, y compris la capacité de chaque pool et chaque passerelle dans chaque pool. Contrôleur de réseau sélectionne le pool correct et la passerelle pour la connexion. Cette sélection est basée sur la bande passante requise pour la connexion. Contrôleur de réseau utilise un algorithme «ajuster» pour sélectionner les connexions efficacement dans un pool. Le point d’homologation BGP pour la connexion est également désigné à ce stade, s’il s’agit de la première connexion du client.  
  
Une fois que le contrôleur de réseau sélectionne une passerelle pour la connexion, le contrôleur de réseau configure la configuration requise pour la connexion sur la passerelle. Si la connexion est une connexion S2S IKEv2, contrôleur de réseau configure également une règle de traduction d’adresses réseau (NAT) sur le pool SLB; cette règle NAT sur le pool SLB dirige les demandes de connexion à partir du client pour la passerelle. Locataires se distinguent par l’adresse IP source, ce qui devrait être unique.  
  
> [!NOTE]  
> Connexions L3 et GRE contournent le SLB et connectent directement avec la passerelle RAS.  Ces connexions requièrent que le routeur du point de terminaison distant (ou tout autre périphérique tiers) doit être correctement configuré pour se connecter à la passerelle.  
  
Le routage BGP est activé pour la connexion, puis sur l’homologation BGP est initiée par cette passerelle - itinéraires sont échangées entre les locaux et cloud passerelles. Les itinéraires appris que par le protocole BGP (ou qui sont configurés de manière statique itinéraires si BGP n’est pas utilisé) sont envoyés au contrôleur de réseau. Contrôleur de réseau il puis ajoute les itinéraires vers le bas pour les ordinateurs hôtes Hyper-V sur lequel sont installés les ordinateurs virtuels clients. À ce stade, le trafic de client peut être acheminé vers le site local approprié. Contrôleur de réseau crée des stratégies de virtualisation de réseau Hyper-V associées qui spécifient les emplacements de la passerelle et il ajoute les vers le bas pour les ordinateurs hôtes Hyper-V.  
  
### <a name="bkmk_ike"></a>Haute disponibilité pour IKEv2 S2S  
Une passerelle dans un pool se compose des connexions et que l’homologation BGP des différents clients. Chaque pool a suis ' passerelles actives et mise en veille «n».  
  
Contrôleur de réseau gère l’échec des passerelles de la manière suivante.  
  
-   Contrôleur de réseau en permanence pings les passerelles dans tous les pools et peut détectent une passerelle qui a échoué ou échec. Contrôleur de réseau peut détecter les types suivants d’échecs de la passerelle.  
  
    -   Échec de l’ordinateur virtuel de passerelle RAS  
  
    -   Échec de l’ordinateur hôte Hyper-V sur lequel la passerelle d’accès distant est en cours d’exécution  
  
    -   Échec du service passerelle  
  
    Contrôleur de réseau stocke la configuration de tous les passerelles actives déployés. La configuration comprend les paramètres de connexion et les paramètres de routage.  
  
-   En cas d’échec d’une passerelle, il a une incidence sur les connexions client sur la passerelle, ainsi que les connexions client qui se trouvent sur d’autres passerelles mais dont RR se trouve sur la passerelle a échoué. Le temps d’arrêt des connexions ce dernier est inférieure à la première. Lorsque le contrôleur de réseau détecte une passerelle a échoué, il effectue les tâches suivantes.  
  
    -   Supprime les itinéraires des connexions concernées les ordinateurs hôtes de calcul.  
  
    -   Supprime les stratégies de virtualisation de réseau Hyper-V sur ces ordinateurs hôtes.  
  
    -   Sélectionne une passerelle de mise en veille, convertit en une passerelle active et configure la passerelle.  
  
    -   Modifie les mappages NAT sur le pool SLB pour pointer les connexions à la nouvelle passerelle.  
  
-   Simultanément, comme la configuration s’affiche sur la nouvelle passerelle active, les connexions IKEv2 S2S et l’homologation BGP sont rétablis. Les connexions et l’homologation BGP peuvent être lancées par la passerelle de cloud ou la passerelle locale. Les passerelles actualiser leurs itinéraires et les envoient dans le contrôleur de réseau. Une fois que le contrôleur de réseau apprend les itinéraires de nouvelles découvertes par les passerelles, contrôleur de réseau envoie les itinéraires et les stratégies associées de la virtualisation de réseau Hyper-V pour les ordinateurs hôtes Hyper-V sur lequel résident les ordinateurs virtuels de clients affectés à la défaillance. Cette activité de contrôleur de réseau est similaire pour le cas d’une nouvelle installation de connexion, il se produit uniquement sur une plus grande échelle.  
  
### <a name="bkmk_gre"></a>Haute disponibilité pour GRE  
Le processus de réponse de basculement de passerelle par le contrôleur de réseau - notamment la détection de défaillance, connexion copie et configuration du routage vers la passerelle de secours, basculement de BGP statique routage des connexions concernées (y compris le retrait et nouveau montage des itinéraires sur calculent hôtes et BGP homologation nouveau) et la reconfiguration des stratégies de virtualisation de réseau Hyper-V sur des ordinateurs hôtes de calcul est le même pour les connexions et les passerelles GRE. Le rétablissement de connexions GRE se produit différemment, toutefois, et la solution à haute disponibilité pour GRE a des exigences supplémentaires.  
  
![Haute disponibilité pour GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
Au moment du déploiement de passerelle, chaque ordinateur virtuel de passerelle RAS est attribué à une adresse IP dynamique (DIP). En outre, chaque ordinateur virtuel de passerelle est également affecté une adresse IP virtuelle (VIP) pour la haute disponibilité GRE. Adresses IP est affectés uniquement aux passerelles dans des pools qui peuvent accepter des connexions GRE et non aux pools non GRE. Les adresses IP affectés est publiés vers le haut de commutateurs TOR (rack) à l’aide du protocole BGP, qui annonce davantage le VIP dans le réseau physique du cloud. Cela rend les passerelles accessible à partir du routeurs à distance ou les appareils tiers dans lequel réside l’autre extrémité de la connexion GRE. Cette homologation BGP est différente de celle de l’homologation pour l’échange d’itinéraires locataire BGP d’au niveau du client.  
  
Au moment de la configuration de la connexion GRE, contrôleur de réseau sélectionne une passerelle, configure un point de terminaison GRE sur la passerelle sélectionnée et renvoie l’adresse IP virtuelle de la passerelle est attribuée. Cette adresse IP virtuelle est ensuite configuré en tant que l’adresse de tunnel GRE sur le routeur à distance de destination.  
  
En cas d’échec d’une passerelle, contrôleur de réseau copie l’adresse IP virtuelle de la passerelle a échoué et autres données de configuration pour la passerelle de secours. Lorsque la mise en veille passerelle devient active, il publie l’adresse IP virtuelle à son commutateur TOR et plus en détail dans le réseau physique. Routeurs à distance continuent à se connecter les tunnels GRE à la même adresse IP virtuelle et l’infrastructure de routage garantit que les paquets sont acheminés vers la nouvelle passerelle active.  
  
### <a name="bkmk_l3"></a>Haute disponibilité pour L3 passerelles de transfert  
Une passerelle de transfert L3 de la virtualisation de réseau Hyper-V est un pont entre l’infrastructure physique du centre de données et l’infrastructure virtualisée dans le cloud de la virtualisation de réseau Hyper-V. Sur une passerelle de transfert de L3 mutualisé, chaque client utilise son propre réseau logique avec balises VLAN pour la connectivité avec le réseau physique du client.  
  
Lorsqu’un nouveau locataire crée une nouvelle passerelle L3, Gestionnaire de Service de passerelle de contrôleur de réseau sélectionne une ordinateur virtuel de passerelle disponible et configure une nouvelle interface client avec une haut niveau d’adresse client (CA) espace adresseIP (à partir du réseau du client VLAN balisé logique). L’adresse IP est utilisée comme adresse IP homologue sur la passerelle à distance (réseau physique) et est tronçon suivant pour atteindre le réseau de virtualisation de réseau Hyper-V du client.  
  
Contrairement aux connexions réseau IPsec ou GRE, le commutateur TOR ne sauront pas dynamiquement réseau avec balises du VLAN du client. Le routage pour le réseau avec balises du VLAN du client doit être configuré sur le commutateur TOR et tous les commutateurs intermédiaires et les routeurs entre l’infrastructure physique et de la passerelle pour assurer la connectivité de bout en bout.  Voici un exemple de configuration de réseau virtuel du fournisseur de services cryptographiques comme illustré dans l’illustration ci-dessous.  
  
|Réseau|Sous-réseau|ID DE VLAN|Passerelle par défaut|  
|-----------|----------|-----------|-------------------|  
|Réseau logique Contoso L3|10.127.134.0/24|1001|10.127.134.1|  
|Réseau logique L3 Woodgrove Bank|10.127.134.0/24|1002|10.127.134.1|  
  
Voici des exemples de configurations de client passerelle comme illustré dans l’illustration ci-dessous.  
  
|Nom de client|Adresse IP de la passerelle L3|ID DE VLAN|Adresse IP de l’homologue|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove Bank|10.127.134.60|1002|10.127.134.65|  
  
Voici l’illustration de ces configurations dans un centre de données du fournisseur de services cryptographiques.  
  
![Haute disponibilité pour L3 passerelles de transfert](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Les échecs de passerelle, la détection d’échec et le processus de basculement de passerelle dans le contexte d’un L3 passerelle de transfert est similaire aux processus pour IKEv2 et GRE RAS passerelles. Les différences résident dans la manière dont les adresses IP externes sont gérées.  
  
Lorsque l’état des ordinateurs virtuels de passerelle devient n’est pas intègre, contrôleur de réseau sélectionne une des passerelles mise en veille dans le pool et dispositions de nouveau les connexions réseau et le routage sur la passerelle de secours. Lors de déplacement de hautement les connexions, la passerelle de transfert L3 l’adresse IP de d’espace disponible autorité de certification est également déplacée vers la nouvelle machine virtuelle de passerelle, ainsi que l’adresse IP BGP espace d’adressage du client.  
  
Étant donné que l’adresse IP d’homologation L3 est déplacé vers la passerelle de nouveaux ordinateurs virtuels pendant le basculement, l’infrastructure physique à distance est à nouveau en mesure de se connecter à cette adresse IP et, par la suite, atteindre la charge de travail de virtualisation de réseau Hyper-V. Pour le protocole BGP routage dynamique, en tant qu’adresse IP BGP est déplacé vers la passerelle de nouveaux ordinateurs virtuels, l’espace d’adressage, le routeur BGP à distance peut rétablir l’homologation et Découvrez tous les itinéraires de virtualisation de réseau Hyper-V à nouveau.  
  
> [!NOTE]  
> Vous devez configurer séparément les commutateurs TOR et tous les routeurs intermédiaires pour pouvoir utiliser le réseau logique avec balises VLAN pour la communication client. En outre, L3 basculement est limitée uniquement racks qui sont configurés de cette façon. Pour cette raison, le pool de passerelle L3 doive être configuré avec soin et la configuration manuelle doit être effectuée séparément.  
  


