---
title: Passerelle du serveur d’accès à distance
description: Cette rubrique, qui est destinée aux professionnels IT (Information Technology), fournit des informations générales sur la passerelle RAS, y compris les modes de déploiement de passerelle RAS et fonctionnalités dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: pashort
author: shortpatti
ms.date: 05/23/2018
ms.openlocfilehash: d61dcdbb61449bd2af57b8e2c99ced6235c4deca
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281264"
---
# <a name="ras-gateway"></a>Passerelle du serveur d’accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Passerelle RAS est un routeur logiciel et une passerelle que vous pouvez utiliser en mode dédié ou en mode mutualisé.  
  
- **Locataire unique** mode permet aux organisations de toute taille pour déployer la passerelle en tant qu’extérieur, ou réseau privé virtuel de périphérie d’accessible sur Internet (VPN) et serveur DirectAccess. En mode client unique, vous pouvez déployer la passerelle RAS sur un serveur physique ou une machine virtuelle (VM) exécutant Windows Server 2016.  
  
- **Mode mutualisé** permet des fournisseurs de services Cloud (CSP) et aux entreprises d’utiliser la passerelle RAS pour activer le centre de données et de cloud de routage du trafic réseau entre les réseaux virtuels et physiques, y compris Internet. Pour le mode partagé, il est recommandé de déployer la passerelle RAS sur des machines virtuelles qui exécutent Windows Server 2016.  
  
> [!NOTE]  
> Passerelle RAS prend en charge IPv4 et IPv6, notamment le transfert IPv4 et IPv6. Lorsque vous configurez passerelle RAS avec la traduction d’adresses réseau (NAT), seul NAT44 est pris en charge.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>Qui sera intéressé par passerelle RAS ?
  
Si vous êtes un administrateur système, architecte réseau ou tout autre professionnel de l’informatique, passerelle RAS peut vous intéresser sous un ou plusieurs des cas suivants :  
  
-   Vous concevez ou prenez en charge une infrastructure informatique qui utilise ou utilisera Hyper-V pour le déploiement de machines virtuelles sur des réseaux virtuels.  
  
-   Vous concevez ou prenez en charge une infrastructure informatique pour une organisation qui a déployé ou prévoit de déployer des technologies cloud.  
  
-   Vous voulez fournir une connexion entre les réseaux physiques et les réseaux virtuels.  
  
-   Vous souhaitez fournir aux clients de votre organisation d’accéder à leurs réseaux virtuels via Internet.  
  
-   Vous souhaitez fournir aux employés de votre organisation un accès à distance au réseau de votre organisation.  
  
-   Vous souhaitez connecter des emplacements physiques différents bureaux distants via Internet.  
 
Cette rubrique, qui est destinée aux professionnels IT (Information Technology), fournit des informations générales sur la passerelle RAS, notamment les fonctionnalités et modes de déploiement de passerelle RAS. 
  
Cette rubrique contient les sections suivantes.  
  
  
-   [Modes de déploiement de passerelle RAS](#bkmk_modes)  
  
-   [Clustering passerelle RAS pour la haute disponibilité](#bkmk_clustering)  
  
-   [Fonctionnalités de passerelle RAS](#bkmk_features)  
  
-   [Scénarios de déploiement de passerelle RAS](#bkmk_deploy)  
  
-   [Outils de gestion de passerelle RAS](#bkmk_manage)  
  


  
## <a name="bkmk_modes"></a>Modes de déploiement de passerelle RAS  
Passerelle RAS inclut les modes de déploiement suivants.  
  
### <a name="single-tenant-mode"></a>Mode de client unique  
Pour la plupart des organisations, à l’aide de la passerelle RAS en mode dédié est la configuration classique. En mode client unique, vous pouvez déployer passerelle RAS en tant que serveur VPN edge, un serveur DirectAccess d’edge ou les deux simultanément. Dans cette configuration, passerelle RAS fournit les employés distants avec une connectivité à votre réseau en utilisant des connexions VPN ou DirectAccess. En outre, mode dédié vous permet de se connecter des emplacements physiques différents bureaux distants via Internet.  
  
### <a name="multitenant-mode"></a>Mode mutualisé  
Si votre organisation est un CSP ou une entreprise avec plusieurs locataires, vous pouvez déployer la passerelle RAS en mode partagé pour fournir le routage du trafic réseau vers et à partir de réseaux virtuels et physiques.  
  
L’architecture mutualisée est la possibilité d’une infrastructure de cloud pour prendre en charge les charges de travail de machine virtuelle de plusieurs locataires, encore les isoler les unes aux autres, tandis que toutes les charges de travail exécutées sur la même infrastructure. Les multiples charges de travail d’un client seul peuvent être interconnectées et être gérées à distance mais ces systèmes n’offrent aucune interconnexion avec les charges de travail d’autres clients et ces derniers ne peuvent pas eux-mêmes les gérer à distance.  
  
Par exemple, une entreprise peut disposer de plusieurs sous-réseaux virtuels, chacun étant dédié à un service spécifique, par exemple Recherche et développement ou Comptabilité. Autre exemple : un CSP avec plusieurs clients qui utilisent des sous-réseaux virtuels isolés au sein du même centre de données physique. Dans les deux cas, passerelle RAS peut acheminer le trafic vers et à partir de chaque client tout en conservant l’isolation de chaque client. Cette fonctionnalité rend la passerelle RAS mutualisation.  
  
Réseaux virtuels sont créés à l’aide de la virtualisation réseau Hyper-V, qui est une technologie qui a été introduite dans Windows Server 2012 et est améliorée dans Windows Server 2016. Passerelle RAS est intégrée avec la virtualisation de réseau Hyper-V et est en mesure d’acheminer le trafic de réseau efficacement dans des scénarios où il sont a plusieurs différents clients - ou locataires - qui ont isolés virtuel réseaux dans le même centre de données.  
  
Virtualisation de réseau Hyper-V vous offre la possibilité de déployer un réseau de machine virtuelle (VM) qui est indépendant du réseau physique sous-jacent. Avec les réseaux de machines virtuelles, qui sont composées d’un ou plusieurs sous-réseaux virtuels, l’emplacement physique exact d’un sous-réseau IP est dissocié de la topologie de réseau virtuel. Par conséquent, vous pouvez facilement déplacer vos sous-réseaux locaux sur vers le cloud - tout en conservant votre adresse IP existante adresses et de la topologie dans le cloud. Cette capacité à conserver l’infrastructure permet aux services existants de continuer à fonctionner, sans se préoccuper de l’emplacement physique des sous-réseaux. Autrement dit, la virtualisation de réseau Hyper-V permet de créer un cloud hybride transparent.  
  
> [!NOTE]  
> Virtualisation de réseau Hyper-V est une technologie de réseau overlay à l’aide de Network Virtualization Generic Routing Encapsulation ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)), ce qui permet aux clients d’utiliser leur propre espace d’adressage ainsi que de fournisseurs de services cloud une meilleure extensibilité possible à l’aide de réseaux locaux virtuels pour l’isolation des locataires.  
  
Dans Windows Server 2016, passerelle RAS achemine le trafic réseau entre le réseau physique et les ressources réseau de machine virtuelle, quel que soit l’emplacement où se trouvent les ressources. Vous pouvez utiliser la passerelle RAS pour acheminer le trafic réseau entre les réseaux physiques et virtuels sur le même emplacement physique ou sur plusieurs emplacements physiques.  
  
Par exemple, si vous avez un réseau physique et un réseau virtuel au même emplacement physique, vous pouvez déployer un ordinateur exécutant Hyper-V qui est configuré avec une machine virtuelle à la passerelle RAS à agir en tant que passerelle de transfert et acheminer le trafic entre le physiques et virtuels réseaux.  
  
Dans un autre exemple, si vos réseaux virtuels existe dans le cloud, votre CSP peut déployer une passerelle RAS afin que vous pouvez créer une connexion de site à site de réseau privé virtuel (VPN) entre votre serveur VPN et passerelle RAS du CSP ; Lorsque cette liaison est établie vous pouvez vous connecter à vos ressources virtuelles dans le cloud via la connexion VPN.  
  
Pour plus d’informations, consultez [passerelle RAS haute disponibilité](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_clustering"></a>Clustering passerelle RAS pour la haute disponibilité  
Passerelle RAS est déployée sur un ordinateur dédié qui exécute Hyper-V et qui est configuré avec une machine virtuelle. La machine virtuelle est ensuite configurée comme une passerelle RAS.  
  
Pour la haute disponibilité des ressources réseau, vous pouvez déployer la passerelle RAS avec basculement à l’aide de deux serveurs hôtes physiques exécutant Hyper-V qui exécutent chaque également une machine virtuelle (VM) qui est configurée en tant que passerelle. Les ordinateurs virtuels passerelle sont ensuite configurés en cluster pour fournir la protection de basculement contre les pannes réseau et matérielles.  
  
Par exemple, si votre organisation est une entreprise avec un déploiement de cloud privé, vous devrez peut-être uniquement deux ordinateurs virtuels passerelle RAS, chacun d’eux est installé sur un autre ordinateur exécutant Hyper-V. Dans ce scénario, les ordinateurs virtuels de passerelle RAS sont ajoutés à un cluster pour fournir une haute disponibilité.  
  
Dans un autre exemple, si votre organisation est un fournisseur de services Cloud (CSP) avec deux cents de clients dans votre centre de données, vous pouvez utiliser huit machines virtuelles de passerelle RAS, avec chaque paire de machines virtuelles de passerelle RAS en cluster fournissant des services de routage pour cinquante clients. Dans ce scénario, deux ordinateurs qui exécutent Hyper-V chaque ont quatre ordinateurs virtuels qui sont configurés en tant que passerelles de RAS. Vous configurez ensuite quatre clusters de machines virtuelles de passerelle RAS, chaque cluster contenant une seule machine virtuelle à partir de chaque ordinateur exécutant Hyper-V.  
  
Lorsque vous déployez cette passerelle, les serveurs hôtes exécutant Hyper-V et les machines virtuelles que vous configurez comme passerelle doivent exécuter Windows Server 2012 R2 ou Windows Server 2016.  
  
## <a name="bkmk_features"></a>Fonctionnalités de passerelle RAS  
Passerelle RAS inclut les fonctionnalités suivantes.  
  
-   **VPN de site à site**. Cette fonctionnalité de passerelle RAS permet de connecter deux réseaux situés à des emplacements physiques différents via Internet à l’aide d’une connexion VPN de site à site. Si vous avez un siège social et plusieurs filiales, vous pouvez déployer une passerelle RAS à chaque emplacement de périmètre et créer des connexions de site à site pour fournir des flux de trafic réseau entre les emplacements. Pour les CSP qui héberge de nombreux clients dans leur centre de données, passerelle RAS fournit une solution de passerelle mutualisée qui permet aux clients accéder et gérer leurs ressources sur les connexions VPN de site à site depuis des sites distants, et qui autorise le flux de trafic réseau entre ressources virtuelles dans votre centre de données et leur réseau physique.  
  
-   **Point-to-site VPN**. Cette fonctionnalité de passerelle RAS permet aux employés d’organisation ou aux administrateurs de se connecter au réseau de votre organisation à partir d’emplacements distants. Pour les déploiements à locataire unique de la passerelle RAS, employés distants peuvent se connecter au réseau de votre organisation à l’aide d’une connexion VPN. Cette connexion vous permet d’utiliser des ressources réseau internes, telles que les sites web intranet et les serveurs de fichiers. Pour des déploiements mutualisés, client permet aux administrateurs réseau connexions de point-to-site VPN pour accéder aux ressources de réseau virtuel dans le centre de données CSP.  
  
-   **Routage dynamique avec protocole BGP (Border Gateway)** . Le protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il s’agit d’un protocole de routage dynamique qui, de plus, découvre automatiquement les itinéraires entre les sites qui sont connectés via des connexions VPN de site à site. Si votre organisation possède plusieurs sites qui sont connectés à l’aide de routeurs BGP comme passerelle RAS, BGP permet les routeurs calculer automatiquement et d’utiliser des itinéraires valides entre eux en cas d’interruption du réseau ou l’échec. Pour plus d’informations, consultez [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  
-   **Traduction d’adresses réseau (NAT)** . Traduction d’adresses réseau (NAT) vous permet de partager une connexion à l’Internet public via une interface unique avec une seule adresse IP publique. Les ordinateurs sur le réseau privé utilisent des adresses privées, non routable. NAT mappe les adresses privées à l’adresse publique. Cette fonctionnalité de passerelle RAS permet des employés de l’organisation avec les déploiements à locataire unique accéder aux ressources Internet derrière la passerelle. Pour les fournisseurs de services cloud, cette fonctionnalité permet aux applications qui sont exécutent sur des machines virtuelles d’accéder à Internet du locataire. Par exemple, un machine virtuelle qui est configuré comme un serveur Web de locataire peut contacter externes ressources financières pour traiter les transactions par carte de crédit.  

  
## <a name="bkmk_deploy"></a>Scénarios de déploiement de passerelle RAS  
Voici les scénarios de déploiement recommandé pour la passerelle RAS.  
  
-   **Périphérie d’entreprise - déploiement de client unique**. Avec le déploiement d’entreprise de client unique, vous pouvez vous connecter un physique à plusieurs autres emplacements physiques sur Internet à l’aide de la connexion VPN de site à site des fonctionnalités - et le protocole BGP (Border Gateway) vous permet d’utiliser le routage dynamique. Vous pouvez également fournir aux employés distants d’accéder au réseau de votre organisation avec des connexions de point-to-site VPN et connexions DirectAccess. (Les connexions DirectAccess sont toujours disponibles et fournissent également l’avantage que vous pouvez gérer facilement les ordinateurs qui sont connectés à l’aide de DirectAccess, car ils sont connectés chaque fois qu’ils sont sur et connecté à Internet). Vous pouvez également configurer des passerelles de client unique entreprise RAS avec NAT, afin que les ordinateurs sur votre Intranet peuvent facilement communiquer avec Internet.  
  
-   **Cloud Service fournisseur Edge - déploiement mutualisé**. Déploiement mutualisé de passerelle RAS pour les fournisseurs de services cloud vous permet d’offrir à vos clients toutes les fonctionnalités qui sont disponibles avec le déploiement de client unique de périphérie d’entreprise. Connexions VPN de site à site entre réseaux virtuels clients dans votre centre de données et les emplacements réseau de locataire sur la moyenne Internet que locataires ont un accès transparent aux ressources de cloud de tout le temps. L’accès VPN point à site pour les locataires signifie que les administrateurs clients peuvent toujours se connecter à leurs réseaux virtuels dans votre centre de données pour gérer leurs ressources. BGP assure le routage dynamique et conserve les clients connectés à leurs ressources, même si des problèmes réseau se produisent sur Internet ou ailleurs. Ainsi que de NAT client pour se connecter aux ressources sur Internet, tels que des ressources de traitement de carte de crédit, les machines virtuelles.  
  
## <a name="bkmk_manage"></a>Outils de gestion de passerelle RAS  
Voici les outils de gestion pour la passerelle RAS.  
  
-   Dans Windows Server 2016, pour déployer un routeur passerelle RAS, vous devez utiliser les commandes Windows PowerShell. Pour plus d’informations, consultez [applets de commande de l’accès à distance](https://docs.microsoft.com/powershell/module/remoteaccess) pour Windows Server 2016 et Windows 10.  
  
-   Dans System Center 2012 R2 Virtual Machine Manager (VMM), la passerelle RAS est nommée passerelle Windows Server. Un ensemble limité d’options de configuration de protocole BGP (Border Gateway) sont disponibles dans l’interface logicielle VMM, y compris **adresse IP BGP locale** et **numéros de système autonome (ASN)** ,  **Liste des adresses IP homologues BGP**, et **les valeurs ASN**. Cependant, vous pouvez utiliser les commandes BGP Windows PowerShell d’accès à distance pour configurer toutes les autres fonctionnalités de la passerelle Windows Server. Pour plus d’informations, consultez [de Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) et [applets de commande de l’accès à distance](https://technet.microsoft.com/library/hh918399.aspx) pour Windows Server 2016 et Windows 10.  
  
## <a name="related-topics"></a>Rubriques connexes
- [Passerelle RAS haute disponibilité](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Tunneling GRE dans Windows Server](gre-tunneling-windows-server.md)
- [Débit et performances du Tunnel GRE de la passerelle RAS](RAS-Gateway-GRE-Perf.md)
