---
title: Vue d’ensemble de la virtualisation réseau Hyper-V dans Windows Server 2016
description: Cette rubrique fournit une vue d’ensemble de la virtualisation de réseau Hyper-V dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0115b7ad-d229-4c69-9d7e-a3f5fbaa3b2f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aecc66630d8fbe994b6619424fa26b5c0c8d0921
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862190"
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Vue d’ensemble de la virtualisation réseau Hyper-V dans Windows Server 2016

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans Windows Server 2016 et Virtual Machine Manager, Microsoft fournit une solution de virtualisation de réseau de bout en bout.  Il existe cinq principaux composants qui constituent la solution de virtualisation de réseau de Microsoft :  
  
-   **Windows Azure Pack pour Windows Server** fournit un accès au portail pour créer des réseaux virtuels et un portail d’administration pour gérer les réseaux virtuels client.  
  
-   **Virtual Machine Manager** (VMM) offre une gestion centralisée de l’infrastructure réseau.  
  
-   **Contrôleur de réseau Microsoft** fournit un point centralisé et programmable d’automation pour gérer, configurer, surveiller et dépanner l’infrastructure de réseau virtuel et physique dans votre centre de données.  
  
-   La**virtualisation de réseau Hyper-V** fournit l’infrastructure nécessaire pour virtualiser le trafic réseau.  
  
-   **Passerelles de virtualisation de réseau Hyper-V** fournir des connexions entre réseaux virtuels et physiques.  
  
Cette rubrique présente les concepts et explique les principaux avantages et fonctionnalités de virtualisation de réseau Hyper-V (une partie de la solution de virtualisation de réseau globale) dans Windows Server 2016. Elle explique comment la virtualisation de réseau profite à la fois aux nuages privés qui recherchent une consolidation des charges de travail des entreprises et aux fournisseurs de services de nuages publics IaaS (Infrastructure as a Service).  
  
Pour plus de détails techniques sur la virtualisation de réseau dans Windows Server 2016, consultez [détails techniques de virtualisation réseau Hyper-V dans Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  
  
**Voulez-vous utiliser**  
  
-   [Vue d’ensemble de la virtualisation de réseau Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  
  
-   [Vue d’ensemble d’Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Vue d'ensemble du commutateur virtuel Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>Description de la fonctionnalité  
Virtualisation de réseau Hyper-V fournit des « réseaux virtuels » (appelés un réseau d’ordinateurs virtuels) aux machines virtuelles similaires à la virtualisation de serveur (hyperviseur) fournit des « machines virtuelles » pour le système d’exploitation. La virtualisation de réseau dissocie les réseaux virtuels de l’infrastructure réseau physique et élimine les contraintes liées aux réseaux VLAN et à l’affectation d’adresses IP hiérarchiques lors de l’approvisionnement d’ordinateurs virtuels. Du fait de cette souplesse, les clients n’ont pas de difficultés à adopter les nuages IaaS et les hébergeurs et administrateurs de centre de données gèrent leur infrastructure avec efficacité, tout en préservant l’isolation mutualisée nécessaire, les exigences de sécurité et en prenant en charge le chevauchement des adresses IP d’ordinateurs virtuels.  
  
Les clients veulent étendre leurs centres de données dans le nuage en toute transparence. Aujourd’hui, bâtir des architectures de nuage hybride transparentes s’avère délicat du point de vue technique. Parmi les plus grandes obstacles les clients sont confrontés est la réutilisation de leurs topologies réseau existantes (sous-réseaux IP adresses, les services réseau et ainsi de suite.) dans le cloud et combler l’écart entre leurs ressources locales et leurs ressources de cloud.  La virtualisation de réseau Hyper-V repose sur le concept d’un réseau d’ordinateurs virtuels indépendant du réseau physique sous-jacent. Avec ce concept de réseau d’ordinateurs virtuels, qui est constitué d’un ou de plusieurs sous-réseaux virtuels, l’emplacement exact dans le réseau physique des ordinateurs virtuels associés à un réseau virtuel est dissocié de la topologie réseau virtuelle. De ce fait, les clients peuvent facilement transférer leurs sous-réseaux virtuels dans le nuage tout en conservant leurs adresses IP et topologie existantes dans le nuage, au point que les services existants continuent de fonctionner sans connaître l’emplacement physique des sous-réseaux. Autrement dit, la virtualisation de réseau Hyper-V permet de créer un cloud hybride transparent.  
  
Abstraction faite du nuage hybride, nombreuses sont les organisations qui consolident leurs centres de données et créent des nuages privés pour exploiter en interne les avantages offerts par l’architecture en nuage sur le plan de l’efficacité et de l’extensibilité. Virtualisation de réseau Hyper-V permet une meilleure flexibilité et l’efficacité pour la topologie du réseau des clouds privés en dissociant d’une unité commerciale (en le rendant virtuelle) à partir de la topologie réseau physique réelle. Ainsi, les unités commerciales peuvent facilement partager un nuage privé interne tout en étant isolées les unes des autres et conserver les topologies réseau existantes. L’équipe chargée des opérations d’un centre de données est libre de déployer et transférer dynamiquement des charges de travail n’importe où dans le centre de données sans aucune interruption de serveurs, ce qui procure une efficacité opérationnelle supérieure qui profite globalement au centre de données.  
  
Pour les propriétaires de la charge de travail, le principal avantage est qu’ils peuvent désormais transférer leur « topologies » de charge de travail vers le cloud sans modifier leurs adresses IP, ni réécrire leurs applications. Par exemple, l’application LOB tierce type est constituée d’un niveau frontal, d’un niveau logique métier et d’un niveau base de données. Par le biais d’une stratégie, la virtualisation de réseau Hyper-V permet au client d’intégrer tout ou partie des trois niveaux dans le nuage, tout en conservant la topologie de routage et les adresses IP des services (c.-à.-d., les adresses IP d’ordinateurs virtuels), sans qu’il soit nécessaire de modifier les applications.  
  
Pour les propriétaires d’infrastructure, la souplesse accrue sur le plan de la sélection élective des ordinateurs virtuels permet d’envisager le déplacement des charges de travail n’importe où dans les centres de données sans avoir à modifier les ordinateurs virtuels ou reconfigurer les réseaux. Par exemple, la virtualisation de réseau Hyper-V autorise une migration dynamique entre sous-réseaux, ce qui permet à un ordinateur virtuel de migrer dynamiquement n’importe où dans le centre de données sans aucune interruption de service. Auparavant, la migration dynamique se cantonnait au même sous-réseau, ce qui limitait les possibilités quant à l’emplacement des ordinateurs virtuels. La migration dynamique entre sous-réseaux permet aux administrateurs de consolider les charges de travail en fonction des besoins en ressources dynamiques, de l’efficacité énergétique, et elle peut également s’accommoder de la maintenance de l’infrastructure sans perturber le temps d’activité des charges de travail client.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Devant le succès des centres de données virtualisés, les organisations informatiques et les fournisseurs d’hébergement (fournisseurs offrant des services de colocalisation ou de location de serveurs physiques) ont commencé à offrir des infrastructures virtualisées plus souples qui facilitent l’offre d’instances de serveur à la demande à leurs clients. Ce nouveau type de service est appelé « infrastructure en tant que service » ou « Infrastructure as a Service » (IaaS) en anglais. Windows Server 2016 fournit toutes les fonctionnalités de plateforme nécessaire pour permettre aux clients d’entreprise créer des clouds privés et la transition vers un de l’informatique comme un modèle opérationnel du service. Windows Server 2016 2016 permet également aux hébergeurs de créer des nuages publics et offrir des solutions IaaS à leurs clients. Lorsqu’il est combiné avec Virtual Machine Manager et Windows Azure Pack pour gérer la stratégie de virtualisation de réseau Hyper-V, Microsoft fournit une solution de cloud puissantes.  
  
Virtualisation de réseau Hyper-V de Windows Server 2016 offre la virtualisation de réseau basée sur des stratégies et contrôlée par le logiciel qui réduit la surcharge auxquelles sont confrontée les entreprises qui développent des nuages IaaS dédiés de gestion, et elle offre une meilleure aux hébergeurs de cloud flexibilité et une extensibilité accrues pour gérer les ordinateurs virtuels pour atteindre une plus grande utilisation de ressources.  
  
Dans un scénario IaaS mettant en scène des ordinateurs virtuels issus de différentes divisions d’organisation (nuage dédié) ou de différents clients (nuage hébergé), une isolation sécurisée s’avère nécessaire. Solution d’aujourd'hui, les réseaux locaux virtuels (VLAN), peut présenter des inconvénients majeurs dans ce scénario.  
  
**Réseaux locaux virtuels**  
  
Actuellement, le VLAN est le mécanisme utilisé par la plupart des organisations pour prendre en charge la réutilisation d’espace d’adressage et l’isolation des locataires. Un VLAN utilise le balisage explicite (ID VLAN) dans les en-têtes de trame Ethernet et a recours à des commutateurs Ethernet pour mettre en œuvre l’isolation et limiter le trafic à destination des nœuds de réseau ayant le même ID VLAN. Les principaux inconvénients des VLAN sont les suivants :  
  
-   Risque accru d’interruptions involontaires en raison de la reconfiguration fastidieuse des commutateurs de production chaque fois que des ordinateurs virtuels ou des limites d’isolation sont déplacés dans le centre de données dynamique.  
  
-   Extensibilité limitée, car les commutateurs classiques prennent seulement en charge 1 000 ID VLAN sur un maximum de 4 094.  
  
-   Limitation à un seul sous-réseau IP, ce qui restreint le nombre de nœuds au sein d’un même VLAN et limite la sélection élective d’ordinateurs virtuels en fonction des emplacements physiques. Même si les VLAN peuvent être étendus à plusieurs sites, l’ensemble du VLAN doit se trouver sur le même sous-réseau.  
  
**Affectation d’adresses IP**  
  
En plus des inconvénients inhérents aux VLAN, attribution d’adresse IP ordinateurs virtuels pose certains problèmes, notamment :  
  
-   Les emplacements physiques dans l’infrastructure réseau de centre de données déterminent les adresses IP des ordinateurs virtuels. Par conséquent, une transition vers le nuage passe généralement par une modification des adresses IP des charges de travail de services.  
  
-   Les stratégies sont liées aux adresses IP, notamment les règles de pare-feu, les services d’annuaire et de découverte des ressources, etc. La modification des adresses IP nécessite la mise à jour de toutes les stratégies associées.  
  
-   Le déploiement d’ordinateurs virtuels et l’isolation du trafic sont dépendants de la topologie.  
  
Lorsque les administrateurs réseau d’un centre de données conçoivent le plan de la topologie physique du centre de données, ils doivent prendre des décisions quant à l’emplacement physique et au routage des sous-réseaux. Ces décisions dépendent de la technologie IP et Ethernet qui détermine les adresses IP possibles pour les ordinateurs virtuels s’exécutant sur un serveur donné ou sur une lame connectée à un rack spécifique du centre de données. Lorsqu’un ordinateur virtuel est approvisionné et placé dans un centre de données, il doit adhérer à ces choix et aux restrictions relatives aux adresses IP. Par conséquent, les administrateurs de centre de données attribuent généralement de nouvelles adresses IP aux ordinateurs virtuels.  
  
Le problème de cette exigence vient du fait que, en plus d’être une adresse, une adresse IP est associée à des informations sémantiques. Ainsi, un sous-réseau peut contenir certains services ou se trouver à un emplacement physique distinct. Les règles de pare-feu, les stratégies de contrôle d’accès et les associations de sécurité IPsec sont généralement associés à des adresses IP. La modification d’adresses IP contraint les propriétaires d’ordinateurs virtuels d’ajuster toutes leurs stratégies qui étaient basées sur les adresses IP d’origine. Cette renumérotation est si contraignante que bon nombre d’entreprises choisissent de ne déployer dans le nuage que les nouveaux services, sans toucher aux applications existantes.  
  
La virtualisation de réseau Hyper-V dissocie les réseaux virtuels des ordinateurs virtuels client de l’infrastructure réseau physique. Les ordinateurs virtuels client peuvent ainsi conserver leurs adresses IP d’origine et les administrateurs de centre de données peuvent approvisionner les ordinateurs virtuels client n’importe où dans le centre de données sans reconfigurer les adresses IP physiques ou les ID VLAN. La section suivante propose un récapitulatif des principales fonctionnalités.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités importantes  
Voici une liste de clés, avantages, les fonctionnalités de virtualisation de réseau Hyper-V dans Windows Server 2016 :  
  
-   **Permet de positionner la charge de travail flexible - l’isolation du réseau et IP adresse réutiliser sans VLAN**  
  
    Virtualisation de réseau Hyper-V dissocie les réseaux virtuels du client à partir de l’infrastructure réseau physique des hébergeurs, en fournissant la liberté pour le placement de la charge de travail dans les centres de données. La sélection élective des charges de travail des ordinateurs virtuels n’est plus limitée par l’attribution d’adresses IP ou les conditions d’isolation VLAN du réseau physique, car elle est mise en œuvre dans les hôtes Hyper-V selon les stratégies de virtualisation mutualisée définies par logiciel.  
  
    Les ordinateurs virtuels de différents clients dont les adresses IP se chevauchent peuvent désormais être déployés sur un même serveur hôte sans avoir à passer par une configuration VLAN fastidieuse ou violer la hiérarchie d’adresses IP. La migration des charges de travail client vers des fournisseurs d’hébergement IaaS partagé peut s’en trouver simplifiée, ce qui permet aux clients de déplacer ces charges de travail sans modification (les adresse IP des ordinateurs virtuels restent inchangées). Pour le fournisseur d’hébergement, la prise en charge de nombreux clients désireux d’étendre leur espace d’adressage réseau existant au centre de données IaaS partagé est un exercice complexe de configuration et de gestion de VLAN isolés pour chaque client pour assurer la coexistence d’espaces d’adressage susceptibles de de se chevaucher. Avec la virtualisation de réseau Hyper-V, la prise en charge d’adresses qui se chevauchent est facilitée et la reconfiguration réseau se trouve allégée pour le fournisseur d’hébergement.  
  
    De plus, la maintenance et les mises à niveau de l’infrastructure physique peuvent être effectuées sans entraîner d’arrêts au niveau des charges de travail client. Avec la virtualisation de réseau Hyper-V, les ordinateurs virtuels présents sur un hôte, un rack, un sous-réseau, un VLAN ou un cluster entier peuvent être migrés sans qu’il soit nécessaire de recourir à une modification des adresses IP physiques ni à une reconfiguration majeure.  
  
-   **Facilité de déplacement des charges de travail à un nuage IaaS partagé**  
  
    Avec la virtualisation de réseau Hyper-V, les adresses IP et les configurations d’ordinateur virtuel restent inchangées. Cela permet aux organisations informatiques de déplacer plus facilement les charges de travail de leurs centres de données vers un fournisseur d’hébergement IaaS partagé moyennant une reconfiguration minime de la charge de travail ou de leurs outils et stratégies d’infrastructure. S’il existe une connectivité entre deux centres de données, les administrateurs informatiques peuvent continuer d’utiliser leurs outils sans les reconfigurer.  
  
-   **Migration dynamique entre sous-réseaux**  
  
    Migration dynamique des charges de travail de machine virtuelle limite traditionnellement pour le même sous-réseau IP ou un réseau local virtuel, car l’interconnexion de sous-réseaux exigeait de système de d’exploitation invité de la machine virtuelle pour modifier son adresse IP. Ce changement d’adresse avait pour effet de couper la communication existante et d’interrompre les services exécutés sur l’ordinateur virtuel. Avec la virtualisation de réseau Hyper-V, les charges de travail peuvent être migrées dynamiques depuis des serveurs exécutant Windows Server 2016 dans un sous-réseau vers les serveurs exécutant Windows Server 2016 dans un sous-réseau différent sans modifier les adresses IP de la charge de travail. La virtualisation de réseau Hyper-V vérifie que le déplacement des ordinateurs virtuels lié à la migration dynamique a fait l’objet d’une mise à jour et d’une synchronisation entre les hôtes, qui ont une communication permanente avec les ordinateurs virtuels migrés.  
  
-   **Simplifie la gestion de l’administration de serveur et réseau découplée**  
  
    La sélection élective des charges de travail serveur est simplifiée, car la migration et la sélection élective des charges de travail sont indépendantes de la configuration des réseaux physiques sous-jacents. Les administrateurs de serveurs peuvent se consacrer pleinement à la gestion des services et des serveurs, tandis que les administrateurs réseau peuvent se concentrer sur la gestion globale du trafic et de l’infrastructure réseau. Cela permet aux administrateurs de serveurs de centre de données de déployer et migrer des ordinateurs virtuels sans modifier les adresses IP des ordinateurs virtuels. La virtualisation de réseau Hyper-V permettant une sélection élective des ordinateurs virtuels quelle que soit la topologie du réseau, les administrateurs réseau sont moins impliqués dans les sélections électives qui pourraient modifier les limites d’isolation, ce qui allège leur charge de travail.  
  
-   **Simplification du réseau et améliore l’utilisation des ressources serveur/réseau**  
  
    La rigidité des VLAN et le fait que la sélection élective des ordinateurs virtuels dépende d’une infrastructure réseau physique se traduisent par un surapprovisionnement et une sous-utilisation. En rompant cette dépendance, la sélection élective des charges de travail d’ordinateurs virtuels gagne en souplesse, ce qui peut simplifier la gestion du réseau et améliorer l’utilisation des ressources serveur et réseau. Notez que la virtualisation de réseau Hyper-V prend en charge les VLAN dans le contexte du centre de données physique. Par exemple, il peut être décidé que pour un centre de données, l’ensemble du trafic de la virtualisation de réseau Hyper-V soit affecté à un VLAN spécifique.  
  
-   **Est compatible avec l’infrastructure existante et les nouvelles technologies**  
  
    Virtualisation de réseau Hyper-V peut être déployée dans Centre de données votre, mais il est compatible avec nouvelles technologies de « réseau plat » de centre de données.  
  
    Par exemple, HNV dans Windows Server 2016 prend en charge le format d’encapsulation VXLAN et le protocole de gestion de base de données (OVSDB) de commutateur virtuel ouvert en tant qu’Interface SouthBound (essai)...  
  
-   **Fournit à l’état de préparation de l’interopérabilité et l’écosystème**  
  
    La virtualisation de réseau Hyper-V prend en charge diverses configurations pour permettre la communication avec les ressources existantes : connectivité intersite, réseau de zone stockage (SAN), accès non virtualisé aux ressources, etc. Microsoft s’est engagé à collaborer avec les partenaires de l’écosystème pour prendre en charge et améliorer l’expérience de la virtualisation de réseau Hyper-V en termes de performances, d’extensibilité et de gestion.  
  
-   **Configuration de stratégies**  
  
    Stratégies de virtualisation de réseau dans Windows Server 2016 sont configurés par le biais du contrôleur de réseau Microsoft. Le contrôleur de réseau a une API RESTful northbound et l’interface Windows PowerShell pour configurer la stratégie. Pour plus d’informations sur le contrôleur de réseau Microsoft, consultez [contrôleur de réseau](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Virtualisation de réseau Hyper-V à l’aide du contrôleur de réseau Microsoft nécessite Windows Server 2016 et le rôle Hyper-V.  
  
## <a name="BKMK_LINKS"></a>Voir aussi  
Pour en savoir plus sur la virtualisation de réseau Hyper-V dans Windows Server 2016, consultez les liens suivants :  
  
|Type de contenu|Références|  
|----------------|--------------|  
|**Ressources de la Communauté**|-   [Blog de l’Architecture de Cloud privé](https://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Poser des questions : [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN - [RFC 7348](https://www.rfc-editor.org/info/rfc7348)|  
|**Technologies associées**|-   [Contrôleur de réseau](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Vue d’ensemble de la virtualisation de réseau Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)|  
  


