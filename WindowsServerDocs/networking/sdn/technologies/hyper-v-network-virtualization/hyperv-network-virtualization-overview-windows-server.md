---
title: Vue d’ensemble de la virtualisation de réseau Hyper-V dans Windows Server 2016
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
ms.openlocfilehash: c7c20f9b2d81ac2d49ed0bbea5f3aca48dea0c50
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-overview-in-windows-server-2016"></a>Vue d’ensemble de la virtualisation de réseau Hyper-V dans Windows Server 2016

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Dans Windows Server 2016 et Virtual Machine Manager, Microsoft fournit une solution de virtualisation de réseau de bout en bout.  Il existe cinq principaux composants constituent la solution de virtualisation de réseau de Microsoft:  
  
-   **Windows Azure Pack pour Windows Server** fournit un accès au portail pour créer des réseaux virtuels et un portail d’administration pour gérer les réseaux virtuels client.  
  
-   **Virtual Machine Manager** (VMM) offre une gestion centralisée de l’infrastructure réseau.  
  
-   **Contrôleur de réseau Microsoft** fournit un point programmable et centralisé de l’automatisation pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données.  
  
-   **La virtualisation de réseau Hyper-V** fournit l’infrastructure nécessaire pour virtualiser le trafic réseau.  
  
-   **Les passerelles de virtualisation de réseau Hyper-V** fournir des connexions entre les réseaux virtuels et physiques.  
  
Cette rubrique présente les concepts et explique les principaux avantages et fonctionnalités de virtualisation de réseau Hyper-V (une partie de la solution de virtualisation de réseau globale) dans Windows Server 2016. Elle explique comment la virtualisation de réseau profite à la fois les clouds privés recherche de consolidation de charges de travail d’entreprise et les fournisseurs de services de cloud public de l’Infrastructure en tant que Service (IaaS).  
  
Pour plus de détails techniques sur la virtualisation de réseau dans Windows Server 2016, voir [détails techniques de virtualisation de réseau Hyper-V dans Windows Server 2016](../../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md).  
  
**Vouliez-vous dire**  
  
-   [Vue d’ensemble de la virtualisation de réseau Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)  
  
-   [Vue d’ensemble d’Hyper-V](assetId:///5aad349f-ef06-464a-b36f-366fbb040143)  
  
-   [Vue d’ensemble du commutateur virtuel Hyper-V](assetId:///e6ec46af-6ef4-49b3-b1f1-5268dc03f05b)  
  
## <a name="BKMK_OVER"></a>Description de la fonctionnalité  
La virtualisation de réseau Hyper-V fournit des «réseaux virtuels» (appelés un réseau d’ordinateurs virtuels) aux ordinateurs virtuels similaires à la virtualisation de serveur (hyperviseur) fournit des «ordinateurs virtuels» au système d’exploitation. Virtualisation de réseau dissocie les réseaux virtuels à partir de l’infrastructure réseau physique et supprime les contraintes de réseau local virtuel et l’attribution d’adresses IP hiérarchique à partir de l’approvisionnement d’ordinateur virtuel. Cette flexibilité simplifie pour les clients atteindre les nuages IaaS et efficace pour les hébergeurs et administrateurs de centre de données gèrent leur infrastructure, tout en maintenant l’isolation mutualisée nécessaire, les exigences de sécurité, et les adresses IP de Machine virtuelle qui se chevauchent prise en charge.  
  
Les clients veulent étendre de manière transparente leurs centres de données dans le cloud. Aujourd'hui, il existe défis techniques rendre ces architectures de nuage hybride transparentes. Parmi les plus grandes difficultés auxquelles clients sont confrontés est la réutilisation de leurs topologies réseau existantes (sous-réseaux, IP adresses, les services réseau et ainsi de suite) dans le cloud et pontage entre leurs ressources locales et leurs ressources de cloud.  La virtualisation de réseau Hyper-V fournit le concept d’un réseau d’ordinateurs virtuels est indépendant du réseau physique sous-jacent. Avec ce concept de réseau d’ordinateurs virtuels, composé d’un ou plusieurs sous-réseaux virtuels, l’emplacement exact dans le réseau physique d’ordinateurs virtuels connectés à un réseau virtuel est dissocié de la topologie réseau virtuelle. Par conséquent, les clients peuvent facilement déplacer leurs sous-réseaux virtuels vers le nuage tout en conservant leurs adresses IP et topologie existantes dans le cloud afin que les services existants continuent de fonctionner sans se préoccuper de l’emplacement physique des sous-réseaux. Autrement dit, la virtualisation de réseau Hyper-V permet à un cloud hybride transparent.  
  
En plus de cloud hybride, de nombreuses organisations sont consolider leurs centres de données et créer des nuages privés pour exploiter en interne les avantages de l’efficacité et l’extensibilité des architectures de nuage. La virtualisation de réseau Hyper-V permet une souplesse et une efficacité de la topologie de réseau de nuages privés en dissociant une unité commerciale (en la rendant virtuelle) à partir de la topologie réseau physique réelle. De cette façon, les unités commerciales peuvent facilement partager un nuage privé interne tout en étant isolées de l’autre et continuer à conserver les topologies réseau existantes. L’équipe des opérations centre de données a la possibilité de déployer et transférer dynamiquement des charges de travail n’importe où dans le centre de données sans interruption du serveur en fournissant une meilleure efficacité et un centre de données global plus efficace.  
  
Pour les propriétaires de la charge de travail, le principal avantage est qu’ils peuvent désormais transférer leur «topologies» de la charge de travail vers le nuage sans modifier leurs adresses IP, ni réécrire leurs applications. Par exemple, l’application cœur de métier à trois niveaux standard est constituée d’un niveau frontal, un niveau logique métier et un niveau de base de données. Via la stratégie, la virtualisation de réseau Hyper-V permet de client intégrer tout ou partie des trois niveaux dans le cloud, tout en conservant la topologie de routage et les adresses IP des services (autrement dit, adresses IP virtuels), sans avoir besoin d’applications d’être modifiés.  
  
Pour les propriétaires d’infrastructure, la souplesse accrue sur la sélection élective d’ordinateur virtuel rend possible le déplacement des charges de travail n’importe où dans les centres de données sans modifier les ordinateurs virtuels ou reconfigurer les réseaux. Par exemple la virtualisation de réseau Hyper-V permet la migration dynamique entre sous-réseaux afin qu’un ordinateur virtuel de migrer dynamiquement n’importe où dans le centre de données sans aucune interruption de service. La migration dynamique était limitée au même sous-réseau restriction où les ordinateurs virtuels a été trouvés. Entre sous-réseaux la migration dynamique permet aux administrateurs de consolider les charges de travail selon les besoins en ressources dynamiques, l’efficacité énergétique et peut également s’accommoder de maintenance de l’infrastructure sans perturber le temps, les charges de travail client.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Le succès des centres de données virtualisés, les services informatiques et les fournisseurs d’hébergement (fournisseurs qui proposent des services de colocalisation ou serveurs physiques) ont commencé à offrir des infrastructures virtualisées plus souples qui facilitent la proposer des instances de serveur à la demande à leurs clients. Cette nouvelle classe de service est appelée Infrastructure en tant que Service (IaaS). Windows Server 2016 fournit toutes les fonctionnalités de plateforme nécessaire pour activer les clients d’entreprise de créer des clouds privés et transition vers un de l’informatique comme un modèle opérationnel du service. Windows Server 2016 2016 permet également aux hébergeurs de créer des nuages publics et d’offrir des solutions IaaS à leurs clients. Lorsqu’elles sont combinées avec Virtual Machine Manager et Windows Azure Pack pour gérer la stratégie de virtualisation de réseau Hyper-V, Microsoft fournit une solution de cloud puissant.  
  
Virtualisation de réseau Hyper-V Windows Server 2016 offre la virtualisation de réseau basée sur la stratégie, et contrôlée par logiciel qui réduit la surcharge auxquels sont confrontée les entreprises lorsque développent des nuages IaaS dédiés, et elle offre aux hébergeurs de cloud une souplesse et une extensibilité accrues pour la gestion des ordinateurs virtuels pour obtenir la meilleure utilisation des ressources de gestion.  
  
Un scénario IaaS machines virtuelles à partir de différentes divisions d’organisation (nuage dédié) ou de différents clients (nuage hébergé), une isolation sécurisée. Solution d’aujourd'hui, les réseaux locaux virtuels (VLAN), peut présenter des inconvénients de taille dans ce scénario.  
  
**Réseaux locaux virtuels**  
  
Actuellement, le VLAN est le mécanisme utilisé par la plupart des organisations pour prendre en charge la réutilisation d’espace d’adressage et l’isolation des locataires. Un VLAN utilise le balisage explicite (ID VLAN) dans les en-têtes de trame Ethernet et elle s’appuie sur les commutateurs Ethernet pour mettre en œuvre d’isolation et limiter le trafic vers les nœuds de réseau ayant le même ID de VLAN. Les principaux inconvénients des VLAN sont les suivantes:  
  
-   Risque accru d’interruptions involontaires en raison de reconfiguration fastidieuse des commutateurs de production chaque fois que les ordinateurs virtuels ou des limites d’isolation déplacement dans le centre de données dynamique.  
  
-   Extensibilité limitée, car il existe un maximum de 4 094 et commutateurs classiques prennent en charge pas plus de 1 000 ID VLAN.  
  
-   Contrainte au sein d’un seul sous-réseau IP, ce qui limite le nombre de nœuds au sein d’un seul réseau local virtuel et restreint la sélection élective d’ordinateurs virtuels basés sur des emplacements physiques. Même si les réseaux locaux virtuels peuvent être étendus à plusieurs sites, l’ensemble du VLAN doit se trouver sur le même sous-réseau.  
  
**Attribution d’adresses IP**  
  
En plus des inconvénients inhérents aux VLAN, l’attribution d’adresses IP virtuels présente des problèmes, à savoir:  
  
-   Emplacements physiques dans l’infrastructure de réseau de centre de données déterminent les adresses IP d’ordinateurs virtuels. Par conséquent, une transition vers le cloud généralement une modification des adresses IP des charges de travail de service.  
  
-   Les stratégies sont liées aux adresses IP, tels que les règles de pare-feu, les services d’annuaire et de découverte de ressources et ainsi de suite. Modification des adresses IP nécessite la mise à jour de toutes les stratégies associées.  
  
-   Déploiement d’ordinateurs virtuels et l’isolation du trafic sont dépendants de la topologie.  
  
Lorsque les administrateurs réseau de centre de données planifiez la disposition physique du centre de données, ils doivent prendre des décisions sur où sous-réseaux seront physiquement placés et routés. Ces décisions sont basées sur la technologie IP et Ethernet qui influencent les adresses IP possibles qui sont autorisés pour les ordinateurs virtuels en cours d’exécution sur un serveur donné ou sur une lame connectée à un rack spécifique dans le centre de données. Lorsqu’un ordinateur virtuel est approvisionné et placé dans le centre de données, il doit adhérer à ces choix et les restrictions relatives à l’adresse IP. Par conséquent, le résultat standard est que les administrateurs de centre de données affecter de nouvelles adresses IP aux ordinateurs virtuels.  
  
Le problème avec cette exigence est qu’en plus d’être une adresse, est associées à une adresse IP à des informations sémantiques. Par exemple, un sous-réseau peut contenir certains services ou dans un emplacement physique distinct. Règles de pare-feu, les stratégies de contrôle d’accès et les associations de sécurité IPsec sont généralement associées à des adresses IP. Modification des adresses IP contraint les propriétaires d’ordinateurs virtuels d’ajuster toutes leurs stratégies qui étaient basées sur l’adresse IP d’origine. Cette renumérotation est très élevée que de nombreuses entreprises choisissent de déployer uniquement les nouveaux services sur le cloud, en conservant des applications héritées.  
  
La virtualisation de réseau Hyper-V dissocie les réseaux virtuels pour les machines virtuelles de client à partir de l’infrastructure réseau physique. Par conséquent, il permet de client des ordinateurs virtuels mettre à jour leurs adresses IP d’origine, tout en permettant aux administrateurs de centre de données approvisionner des ordinateurs virtuels client n’importe où dans le centre de données sans reconfigurer les adresses IP physiques ou des ID de VLAN. La section suivante récapitule les fonctionnalités clés.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités importantes  
Voici une liste des clés, avantages, les fonctionnalités de virtualisation de réseau Hyper-V dans Windows Server 2016:  
  
-   **Permet la sélection élective des charges de travail flexible - isolement réseau et IP réutilisation d’adresses sans VLAN**  
  
    La virtualisation de réseau Hyper-V dissocie les réseaux virtuels du client à partir de l’infrastructure réseau physique des hébergeurs, en fournissant de liberté pour les emplacements de la charge de travail à l’intérieur des centres de données. La sélection élective des charges de travail de machine virtuelle n’est plus limitée par l’attribution d’adresses IP ou les conditions d’isolation VLAN du réseau physique, car elle est appliquée au sein des ordinateurs hôtes Hyper-V selon les stratégies de virtualisation mutualisée définies par le logiciel.  
  
    Machines virtuelles à partir de différents clients avec des adresses IP qui se chevauchent peuvent désormais être déployés sur le même serveur hôte sans nécessiter une configuration VLAN fastidieuse ou violer la hiérarchie des adresses IP. Cela peut simplifier la migration des charges de travail client dans les fournisseurs, ce qui permet aux clients de déplacer ces charges de travail sans modification, ce qui implique notamment de laisser les adresses IP de machine virtuelle inchangées d’hébergement IaaS partagé. Pour le fournisseur d’hébergement, la prise en charge de nombreux clients désireux d’étendre leur espace d’adressage réseau existant au centre de données IaaS partagé est un exercice complexe de configuration et la maintenance des réseaux locaux virtuels isolés pour chaque client pour assurer la coexistence d’espaces d’adressage de se chevaucher. Avec la virtualisation de réseau Hyper-V, prise en charge les adresses qui se chevauchent est facilitée et nécessite moins reconfiguration réseau par le fournisseur d’hébergement.  
  
    En outre, mises à niveau et maintenance de l’infrastructure physique peuvent être effectuées sans entraîner d’arrêts de charges de travail client. Avec la virtualisation de réseau Hyper-V, les ordinateurs virtuels sur un ordinateur hôte spécifique, rack, sous-réseau, VLAN ou cluster entier peuvent être migrés sans demander de modification des adresses IP physique ou une reconfiguration majeure.  
  
-   **Facilité de déplacement des charges de travail à un nuage IaaS partagé**  
  
    Avec la virtualisation de réseau Hyper-V, les adresses IP et des configurations d’ordinateurs virtuels restent inchangées. Cela permet aux organisations informatiques de déplacer plus facilement les charges de travail à partir de leurs centres de données pour un fournisseur d’hébergement IaaS partagé moyennant une reconfiguration minime de la charge de travail ou leurs outils de l’infrastructure et les stratégies. Dans les cas où il existe une connectivité entre deux centres de données, les administrateurs informatiques peuvent continuer à utiliser leurs outils sans les reconfigurer.  
  
-   **Migration dynamique entre sous-réseaux**  
  
    La migration dynamique des charges de travail de machine virtuelle a traditionnellement limitée au même sous-réseau IP ou VLAN, car les sous-réseaux exigeait système de d’exploitation invité de l’ordinateur virtuel pour modifier son adresse IP. Ce changement d’adresse interrompt la communication existante et interrompre les services exécutés sur l’ordinateur virtuel. Avec la virtualisation de réseau Hyper-V, les charges de travail peuvent être migrées dynamiques à partir de serveurs exécutant Windows Server 2016 dans un sous-réseau vers des serveurs exécutant Windows Server 2016 dans un autre sous-réseau, sans modifier les adresses IP de la charge de travail. La virtualisation de réseau Hyper-V permet de s’assurer que les changements d’emplacement de machine virtuelle en raison de la migration dynamique sont mis à jour et synchronisés entre les ordinateurs hôtes qui ont une communication permanente avec l’ordinateur virtuel migré.  
  
-   **Facilite la gestion de l’administration de serveur et réseau découplée**  
  
    La sélection élective des charges de travail serveur est simplifiée, car la migration et la sélection élective des charges de travail sont indépendantes des configurations de réseau physique sous-jacent. Les administrateurs de serveur peuvent se concentrer sur la gestion des services et des serveurs et les administrateurs réseau peuvent se concentrer sur la gestion de trafic et d’infrastructure de réseau globale. Cela permet aux administrateurs de serveur de centre de données déployer et migrer des machines virtuelles sans modifier les adresses IP des ordinateurs virtuels. Il est réduit surcharge car la virtualisation de réseau Hyper-V permet la sélection élective d’ordinateur virtuel se produise topologie du réseau, ce qui réduit la nécessité pour les administrateurs réseau impliqués dans les emplacements qui pourraient modifier les limites d’isolation.  
  
-   **Simplification du réseau et améliore l’utilisation des ressources serveur/réseau**  
  
    La rigidité des VLAN et la dépendance de la sélection élective de l’ordinateur virtuel sur une infrastructure réseau physique se traduit par un surapprovisionnement et une sous-utilisation. En divisant la dépendance, la plus grande souplesse de la sélection élective des charges de travail de machine virtuelle peut simplifier la gestion de réseau et améliorer le serveur et l’utilisation des ressources réseau. Notez que la virtualisation de réseau Hyper-V prend en charge les réseaux locaux virtuels dans le contexte du centre de données physique. Par exemple, un centre de données que tout le trafic de la virtualisation de réseau Hyper-V sur un réseau local virtuel spécifique.  
  
-   **Est compatible avec l’infrastructure existante et les nouvelles technologies**  
  
    La virtualisation de réseau Hyper-V peut être déployée dans le centre de données d’aujourd'hui, mais il est compatible avec les nouvelles technologies de «réseau plat» de centre de données.  
  
    Par exemple, HNV dans Windows Server 2016 prend en charge le format d’encapsulation VXLAN et le commutateur virtuel ouvrir protocole de gestion de base de données (OVSDB) en tant qu’Interface SouthBound (essai) accédons  
  
-   **Fournit pour la mise en réseau et de l’interopérabilité**  
  
    La virtualisation de réseau Hyper-V prend en charge plusieurs configurations pour la communication avec les ressources existantes, telles que Cross-connectivité intersite, réseau de zone de stockage (SAN), l’accès aux ressources non virtualisé et ainsi de suite. Microsoft s’est engagé à collaborer avec les partenaires de l’écosystème pour prendre en charge et améliorer l’expérience de virtualisation de réseau Hyper-V en termes de performances, d’extensibilité et la facilité de gestion.  
  
-   **Configuration de stratégies**  
  
    Stratégies de virtualisation de réseau dans Windows Server 2016 sont configurés par le biais du contrôleur de réseau Microsoft. Le contrôleur de réseau a une API northbound RESTful et l’interface Windows PowerShell pour configurer la stratégie. Pour plus d’informations sur le contrôleur de réseau Microsoft, consultez [contrôleur de réseau](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md).  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
La virtualisation de réseau Hyper-V à l’aide du contrôleur de réseau Microsoft requiert Windows Server 2016 et le rôle Hyper-V.  
  
## <a name="BKMK_LINKS"></a>Voir aussi  
Pour en savoir plus sur la virtualisation de réseau Hyper-V dans Windows Server 2016, voir les liens suivants:  
  
|Type de contenu|Références|  
|----------------|--------------|  
|**Ressources de la Communauté**|-   [Blog de l’Architecture de Cloud privé](http://blogs.technet.com/b/privatecloud/archive/2012/03/19/cloud-datacenter-network-architecture-in-the-windows-server-8-era.aspx)<br />-Posez des questions:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-VXLAN - [RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**Technologies connexes**|-   [Contrôleur de réseau](../../../sdn/technologies/network-controller/../../../sdn/technologies/network-controller/Network-Controller.md)<br />-   [Vue d’ensemble de la virtualisation de réseau Hyper-V](assetId:///bf1dba9d-1960-4dd2-a5e2-99466a02044b) (Windows Server 2012 R2)|  
  


