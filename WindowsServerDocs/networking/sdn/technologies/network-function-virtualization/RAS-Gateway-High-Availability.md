---
title: Haute disponibilité de la passerelle du serveur d’accès à distance
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les configurations de haute disponibilité pour la passerelle mutualisée RAS pour la mise en réseau SDN (Software Defined) dans Windows Server 2016.
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
ms.openlocfilehash: 8ce515ceeb7ab6989ef18055f312983a6518a1a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847760"
---
# <a name="ras-gateway-high-availability"></a>Haute disponibilité de la passerelle du serveur d’accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les configurations de haute disponibilité pour la passerelle mutualisée RAS pour la mise en réseau SDN (Software Defined).  
  
Cette rubrique contient les sections suivantes.  
  
-   [Vue d’ensemble de la passerelle RAS](#bkmk_overview)  
  
-   [Vue d’ensemble des Pools de passerelle](#bkmk_pools)  
  
-   [Vue d’ensemble du déploiement de passerelle RAS](#bkmk_deployment)  
  
-   [Intégration de la passerelle RAS avec le contrôleur de réseau](#bkmk_integration)  
  
## <a name="bkmk_overview"></a>Vue d’ensemble de la passerelle RAS  
Si votre organisation est un fournisseur de services Cloud (CSP) ou une entreprise avec plusieurs locataires, vous pouvez déployer la passerelle RAS en mode partagé pour fournir le routage du trafic réseau vers et depuis des réseaux virtuels et physiques, y compris Internet.  
  
Vous pouvez déployer la passerelle RAS en mode partagé au comme une passerelle pour acheminer le trafic réseau de client locataire au locataire de ressources et les réseaux virtuels.  
  
Lorsque vous déployez plusieurs instances de machines virtuelles de passerelle RAS qui fournissent le basculement et la haute disponibilité, vous déployez un pool de passerelle. Dans Windows Server 2012 R2, la passerelle de toutes les machines virtuelles formé un pool unique, ce qui fait une séparation logique entre le déploiement de passerelle un peu difficile.  Passerelle Windows Server 2012 R2 proposé un déploiement de redondance de 1:1 pour le machines virtuelles, ce qui a entraîné la capacité disponible pour le site à site (S2S) sous-utilisation du connexions VPN de passerelle.  
  
Ce problème est résolu dans Windows Server 2016, qui fournit plusieurs Pools de passerelle - qui peuvent être de différents types pour la séparation logique. Le nouveau mode d’une redondance M + N permet une configuration de basculement plus efficace.  
  
Pour plus d’informations vue d’ensemble sur la passerelle RAS, consultez [passerelle RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="bkmk_pools"></a>Vue d’ensemble des Pools de passerelle  
Dans Windows Server 2016, vous pouvez déployer des passerelles dans un ou plusieurs pools.  
  
L’illustration suivante montre les différents types de pools de passerelle qui fournissent le routage du trafic entre des réseaux virtuels.  
  
![Pools de passerelle RAS](../../../media/RAS-Gateway-High-Availability/ras_pools.png)  
  
Chaque pool a les propriétés suivantes.  
  
-   Chaque pool est redondant M + N. Cela signifie qu’un suis ' nombre de machines virtuelles de passerelle active est sauvegardé par un nombre « n » des machines virtuelles de passerelle de secours. La valeur de N (passerelles de secours) est toujours inférieure ou égale à M (passerelles actives).  
  
-   Un pool peut effectuer toutes les fonctions de passerelle individuelles : Internet Key Exchange version 2 (IKEv2) S2S, couche 3 (L3) et l’Encapsulation GRE (Generic Routing) - ou le pool peut effectuer toutes ces fonctions.  
  
-   Vous pouvez affecter une adresse IP publique unique pour tous les pools ou à un sous-ensemble de pools. Fait donc considérablement permet de réduire le nombre d’adresses IP publiques que vous devez utiliser, car il est possible d’avoir tous les locataires à connecter au cloud sur une seule adresse IP. La section ci-dessous sur la haute disponibilité et l’équilibrage de charge décrit comment cela fonctionne.  
  
-   Vous pouvez facilement augmenter un pool de passerelle vers le haut ou vers le bas en ajoutant ou supprimant des machines virtuelles de passerelle dans le pool. Suppression ou ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer des pools d’ensemble des passerelles.  
  
-   Connexions d’un client unique peuvent s’arrêter sur plusieurs pools et les passerelles dans un pool. Toutefois, si un client possède des connexions se terminant par un **tous les** type de pool de passerelle, il ne peut pas s’abonner à d’autres **tous les** type ou des pools de passerelle de type individuel.  
  
Pools de passerelles fournissent également la flexibilité nécessaire pour activer des scénarios supplémentaires :  
  
-   Pools de client unique - vous pouvez créer un pool pour une utilisation par un seul client.  
  
-   Si vous vendez des services cloud via les canaux de partenaire (revendeur), vous pouvez créer des jeux de pools distincts pour chaque revendeur.  
  
-   Plusieurs pools peuvent fournir la même fonction de passerelle, mais différentes capacités. Par exemple, vous pouvez créer un pool de passerelle qui prend en charge un débit élevé et les connexions IKEv2 S2S un débit faible.  
  
## <a name="bkmk_deployment"></a>Vue d’ensemble du déploiement de passerelle RAS  
L’illustration suivante montre un déploiement type de fournisseur de services Cloud (CSP) de passerelle RAS.  
  
![Vue d’ensemble du déploiement de passerelle RAS](../../../media/RAS-Gateway-High-Availability/ras_csp_deploy.png)  
  
Avec ce type de déploiement, les pools de passerelle sont déployés derrière un logiciel équilibreur de charge (SLB), ce qui permet le CSP affecter une adresse IP publique unique pour le déploiement complet. Plusieurs connexions de passerelle d’un locataire peuvent se terminer sur plusieurs pools de passerelle - et sur plusieurs passerelles au sein d’un pool. Ceci est illustré via des connexions S2S d’IKEv2 dans le diagramme ci-dessus, mais les mêmes ne sont applicable à d’autres fonctions de passerelle, telles que des passerelles L3 et GRE.  
  
Dans l’illustration, l’appareil MT BGP est la passerelle mutualisée RAS avec le protocole BGP. BGP mutualisé est utilisé pour le routage dynamique. Le routage d’un locataire est centralisé - un point unique, appelé le réflecteur d’itinéraire (RR), gère l’homologation BGP pour tous les sites locataires. L’enregistrement de ressource est répartie sur toutes les passerelles dans un pool. Cela entraîne une configuration où les connexions d’un locataire (chemin d’accès de données) terminées sur plusieurs passerelles, mais l’enregistrement de ressource pour le locataire (point homologation BGP - chemin d’accès de contrôle) est sur un seul des passerelles.  
  
Le routeur BGP est réparti dans le diagramme pour représenter ce concept de routage centralisé. L’implémentation du protocole BGP passerelle fournit également le routage de transit, ce qui permet le cloud pour agir en tant que point de transit pour le routage entre les sites locataires deux. Ces fonctionnalités BGP sont applicables à toutes les fonctions de passerelle.  
  
## <a name="bkmk_integration"></a>Intégration de la passerelle RAS avec le contrôleur de réseau  
Passerelle RAS est entièrement intégrée au contrôleur de réseau dans Windows Server 2016. Lors de la passerelle RAS et contrôleur de réseau sont déployées, contrôleur de réseau exécute les fonctions suivantes.  
  
-   Déploiement des pools de passerelle  
  
-   Configuration des connexions client sur chaque passerelle  
  
-   Le trafic réseau de commutation sont envoyées à une passerelle de secours en cas de défaillance de la passerelle  
  
Les sections suivantes fournissent des informations détaillées sur la passerelle RAS et contrôleur de réseau.  
  
-   [Approvisionnement et l’équilibrage de charge de connexions de passerelle (IKEv2 L3 et GRE)](#bkmk_provisioning)  
  
-   [Haute disponibilité pour IKEv2 S2S](#bkmk_ike)  
  
-   [Haute disponibilité pour GRE](#bkmk_gre)  
  
-   [Haute disponibilité pour L3 transfert passerelles](#bkmk_l3)  
  
### <a name="bkmk_provisioning"></a>Approvisionnement et l’équilibrage de charge de connexions de passerelle (IKEv2 L3 et GRE)  
Lorsqu’un client demande une connexion de passerelle, la demande est envoyée au contrôleur de réseau. Contrôleur de réseau est configuré avec les informations sur tous les pools de passerelle, y compris la capacité de chaque passerelle et chaque pool dans chaque pool. Contrôleur de réseau sélectionne l’appropriés du pool et la passerelle pour la connexion. Cette sélection est basée sur les besoins en bande passante pour la connexion. Contrôleur de réseau utilise un algorithme « best fit » pour récupérer efficacement des connexions dans un pool. Le point d’homologation BGP pour la connexion est également désigné pour l’instant si c’est la première connexion du client.  
  
Une fois que le contrôleur de réseau sélectionne une passerelle RAS pour la connexion, le contrôleur de réseau configure la configuration nécessaire pour la connexion sur la passerelle. Si la connexion est une connexion S2S de IKEv2, contrôleur de réseau configure également une règle de traduction d’adresses réseau (NAT) sur le pool SLB ; Cette règle NAT sur le pool SLB dirige les demandes de connexion du client vers la passerelle. Locataires sont différenciées par l’adresse IP source, qui doit être unique.  
  
> [!NOTE]  
> Connexions L3 et GRE contournent le SLB et échangez directement avec la passerelle RAS.  Ces connexions requièrent que le routeur de point de terminaison distant (ou tout autre périphérique tiers) doit être correctement configuré pour vous connecter avec la passerelle RAS.  
  
Si le routage BGP est activé pour la connexion, puis l’homologation BGP est initié par la passerelle RAS - et itinéraires sont échangées entre les locaux et cloud passerelles. Les itinéraires qui sont apprises par BGP (ou qui sont des itinéraires configurés de manière statique si BGP n’est pas utilisé) sont envoyées au contrôleur de réseau. Contrôleur de réseau il puis ajoute les itinéraires vers les hôtes Hyper-V sur lequel sont installés les machines virtuelles clientes. À ce stade, le trafic de client peut être routé vers le site local correct. Contrôleur de réseau crée des stratégies de virtualisation de réseau Hyper-V associées qui spécifient des emplacements de la passerelle et il les ajoute aux hôtes Hyper-V.  
  
### <a name="bkmk_ike"></a>Haute disponibilité pour IKEv2 S2S  
Une passerelle RAS dans un pool se compose de connexions et de l’homologation BGP de différents clients. Chaque pool comporte suis ' passerelles actives et « n » en attente.  
  
Contrôleur de réseau gère l’échec des passerelles de la manière suivante.  
  
-   Contrôleur de réseau en permanence pings les passerelles dans tous les pools et peut détectent une passerelle qui a échoué ou ayant échoué. Contrôleur de réseau peut détecter les types suivants d’échecs de la passerelle RAS.  
  
    -   Échec de la machine virtuelle de passerelle RAS  
  
    -   Échec de l’hôte Hyper-V sur lequel la passerelle RAS est en cours d’exécution  
  
    -   Échec du service de passerelle RAS  
  
    Contrôleur de réseau stocke la configuration de toutes les passerelles actives déployées. Configuration se compose de paramètres de connexion et les paramètres de routage.  
  
-   En cas d’échec d’une passerelle, son impact sur les connexions client sur la passerelle, ainsi que des connexions de client qui se trouvent sur d’autres passerelles mais dont RR réside sur la passerelle ayant échouée. Temps d’arrêt des connexions de ce dernier est inférieure à la première. Lorsque le contrôleur de réseau détecte une passerelle ayant échouée, il effectue les tâches suivantes.  
  
    -   Supprime les itinéraires des connexions concernées les hôtes de calcul.  
  
    -   Supprime les stratégies de virtualisation de réseau Hyper-V sur ces ordinateurs hôtes.  
  
    -   Sélectionne une passerelle de secours, il convertit en une passerelle active et configure la passerelle.  
  
    -   Modifie les mappages NAT sur le pool SLB pour pointer les connexions à la nouvelle passerelle.  
  
-   Simultanément, comme la configuration s’affiche sur la nouvelle passerelle active, les connexions IKEv2 S2S et l’homologation BGP sont rétablis. Les connexions et l’homologation BGP peuvent être initiés par la passerelle de cloud ou la passerelle locale. Les passerelles actualiser leurs routes et les envoient au contrôleur de réseau. Une fois que le contrôleur de réseau apprend les itinéraires de nouvelles découvertes par les passerelles, contrôleur de réseau envoie les itinéraires et les stratégies associées de la virtualisation de réseau Hyper-V pour les hôtes Hyper-V où se trouvent les machines virtuelles des locataires affectés à la défaillance. Cette activité de contrôleur de réseau est similaire à la situation d’une nouvelle installation de connexion, il se produit uniquement sur une plus grande échelle.  
  
### <a name="bkmk_gre"></a>Haute disponibilité pour GRE  
Le processus de réponse de basculement de passerelle RAS par le contrôleur de réseau - y compris la détection de défaillance, la copie de connexion et la configuration de routage vers la passerelle de secours, le basculement de BGP/statique routage des connexions concernées (y compris le retrait et nouveau de l’entretien des itinéraires sur calcul hôtes et ré-l'homologation BGP), et la reconfiguration des stratégies de virtualisation de réseau Hyper-V sur des hôtes de calcul - est le même pour les connexions et passerelles GRE. Le rétablissement des connexions GRE se produit différemment, toutefois, et la solution de haute disponibilité pour GRE a certaines exigences supplémentaires.  
  
![Haute disponibilité pour GRE](../../../media/RAS-Gateway-High-Availability/ras_ha.png)  
  
Au moment du déploiement de passerelle, chaque ordinateur virtuel de passerelle RAS est affecté une adresse IP dynamique (DIP). En outre, chaque machine virtuelle de passerelle est également assignée à une adresse IP virtuelle (VIP) pour la haute disponibilité GRE. Adresses IP virtuelles sont affectées uniquement à des passerelles dans des pools qui peuvent accepter des connexions GRE et non à des pools non GRE. Les adresses IP virtuelles affectées sont publiés vers le haut de commutateurs de rack (TOR) à l’aide du protocole BGP, qui annonce davantage les adresses IP virtuelles dans le réseau physique du cloud. Cela rend les passerelles accessibles à partir de routeurs à distance ou de périphériques tiers où se trouve l’autre extrémité de la connexion GRE. Cette homologation BGP est différente de celle au niveau du locataire l’appairage BGP pour l’échange d’itinéraires de locataire.  
  
Au moment de la configuration de la connexion GRE, contrôleur de réseau sélectionne une passerelle, configure un point de terminaison GRE sur la passerelle sélectionnée et renvoie l’adresse IP virtuelle de la passerelle attribuée. Cette adresse IP virtuelle est ensuite configurée comme l’adresse de tunnel GRE sur le routeur à distance de destination.  
  
En cas d’échec d’une passerelle, contrôleur de réseau copie l’adresse IP virtuelle de la passerelle ayant échouée et les autres données de configuration à la passerelle de secours. Lorsque la passerelle de secours devient active, il publie l’adresse IP virtuelle à son commutateur TOR et de façon plus approfondie du réseau physique. Routeurs distants continuent à connecter les tunnels GRE à la même adresse IP virtuelle et l’infrastructure de routage permet de s’assurer que les paquets sont acheminés vers la nouvelle passerelle active.  
  
### <a name="bkmk_l3"></a>Haute disponibilité pour L3 transfert passerelles  
Une passerelle de transfert de L3 de la virtualisation de réseau Hyper-V est un pont entre l’infrastructure physique du centre de données et l’infrastructure virtualisée dans le cloud de virtualisation de réseau Hyper-V. Sur une passerelle de transfert de L3 mutualisée, chaque client utilise son propre réseau logique avec balises VLAN pour la connectivité avec le réseau physique du locataire.  
  
Lorsqu’un nouveau client crée une nouvelle passerelle L3, Gestionnaire de Service de passerelle de contrôleur de réseau sélectionne une machine virtuelle de passerelle disponible et configure une nouvelle interface client avec une adresse IP espace adresse client (CA) hautement disponible (à partir de réseau logique avec balises d’un réseau local virtuel du locataire ). L’adresse IP est utilisée comme adresse IP homologue sur la passerelle à distance (réseau physique) et est le tronçon suivant pour atteindre le réseau de virtualisation de réseau Hyper-V du locataire.  
  
Contrairement aux connexions réseau IPsec ou GRE, le commutateur TOR ne sauront pas dynamiquement de réseau avec balises du VLAN du locataire. Le routage pour le réseau avec balises du VLAN du locataire doit être configuré sur le commutateur TOR et tous les commutateurs intermédiaires et les routeurs entre l’infrastructure physique et de la passerelle pour garantir la connectivité de bout en bout.  Voici un exemple de configuration de réseau virtuel CSP comme illustré dans l’illustration ci-dessous.  
  
|Network (Réseau)|Sous-réseau|ID du réseau local virtuel|Passerelle par défaut|  
|-----------|----------|-----------|-------------------|  
|Réseau logique L3 de Contoso|10.127.134.0/24|1001|10.127.134.1|  
|Réseau logique L3 de Woodgrove|10.127.134.0/24|1002|10.127.134.1|  
  
Voici des exemples de configuration de passerelle client comme illustré dans l’illustration ci-dessous.  
  
|Nom du locataire|Adresse IP de passerelle L3|ID du réseau local virtuel|Adresse IP d’homologue|  
|---------------|-------------------------|-----------|-------------------|  
|Contoso|10.127.134.50|1001|10.127.134.55|  
|Woodgrove|10.127.134.60|1002|10.127.134.65|  
  
Voici l’illustration de ces configurations dans un centre de données CSP.  
  
![Haute disponibilité pour L3 transfert passerelles](../../../media/RAS-Gateway-High-Availability/ras_fwdgw.png)  
  
Les échecs de passerelle, la détection de défaillance et le processus de basculement de passerelle dans le contexte d’un L3 transfert de passerelle est similaire aux processus pour IKEv2 et les passerelles de RAS GRE. Les différences résident dans le traitement des adresses IP externes.  
  
Lorsque l’état de la machine virtuelle de passerelle devient non intègre, le contrôleur de réseau sélectionne une des passerelles de secours dans le pool et redéploie les connexions réseau et le routage sur la passerelle de secours. Tandis que déplacer les connexions, la passerelle de transfert L3 hautement disponible autorité de certification espace adresse IP est également déplacée à la nouvelle machine virtuelle de passerelle, ainsi que l’espace de l’autorité de certification adresse IP BGP du locataire.  
  
Étant donné que l’adresse IP de l’homologation L3 est déplacé vers la nouvelle machine virtuelle de passerelle pendant le basculement, l’infrastructure physique distant est à nouveau en mesure de se connecter à cette adresse IP et, par la suite, atteindre la charge de travail de virtualisation de réseau Hyper-V. Pour le routage dynamique BGP, car l’espace de l’autorité de certification adresse IP BGP est déplacé vers la nouvelle passerelle de machine virtuelle, le routeur BGP à distance peut rétablir l’homologation et Découvrez tous les itinéraires de virtualisation de réseau Hyper-V à nouveau.  
  
> [!NOTE]  
> Vous devez configurer séparément les commutateurs TOR et tous les routeurs intermédiaires pour pouvoir utiliser le réseau logique avec balises VLAN pour la communication du client. En outre, L3 basculement est limitée aux racks uniquement qui sont configurés de cette façon. Pour cette raison, le pool de passerelle L3 doit être configuré avec soin et une configuration manuelle doit être accomplie séparément.  
  


