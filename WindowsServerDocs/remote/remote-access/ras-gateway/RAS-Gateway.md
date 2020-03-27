---
title: Passerelle du serveur d’accès à distance
description: Cette rubrique, qui est destinée aux professionnels de l’informatique, fournit des informations générales sur la passerelle RAS, y compris les modes de déploiement et les fonctionnalités de la passerelle RAS dans Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: lizross
author: eross-msft
ms.date: 05/23/2018
ms.openlocfilehash: 762ba98a57db1411098c6ae6a8394e9a9b063181
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308524"
---
# <a name="ras-gateway"></a>Passerelle du serveur d’accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

La passerelle RAS est un routeur logiciel et une passerelle que vous pouvez utiliser en mode à un seul locataire ou en mode multi-locataire.  
  
- Le mode de **locataire unique** permet aux organisations de toutes tailles de déployer la passerelle comme un réseau privé virtuel (VPN) et un serveur DirectAccess côté Internet. En mode mono-client, vous pouvez déployer la passerelle RAS sur un serveur physique ou une machine virtuelle exécutant Windows Server 2016.  
  
- Le mode multi- **locataire** permet aux fournisseurs de services Cloud (CSP) et aux entreprises d’utiliser la passerelle RAS pour activer le routage du trafic réseau du centre de donnes et du Cloud entre les réseaux virtuels et physiques, y compris Internet. Pour le mode multi-locataire, il est recommandé de déployer la passerelle RAS sur les machines virtuelles qui exécutent Windows Server 2016.  
  
> [!NOTE]  
> La passerelle RAS prend en charge IPv4 et IPv6, notamment le transfert IPv4 et IPv6. Quand vous configurez une passerelle RAS avec la traduction d’adresses réseau (NAT), seul NAT44 est pris en charge.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>À qui s’adresse la passerelle RAS ?
  
Si vous êtes administrateur système, architecte réseau ou tout autre professionnel de l’informatique, la passerelle RAS peut vous intéresser dans une ou plusieurs des circonstances suivantes :  
  
-   Vous concevez ou prenez en charge une infrastructure informatique qui utilise ou utilisera Hyper-V pour le déploiement de machines virtuelles sur des réseaux virtuels.  
  
-   Vous concevez ou prenez en charge une infrastructure informatique pour une organisation qui a déployé ou prévoit de déployer des technologies cloud.  
  
-   Vous voulez fournir une connexion entre les réseaux physiques et les réseaux virtuels.  
  
-   Vous voulez fournir aux clients de votre organisation un accès à leurs réseaux virtuels sur Internet.  
  
-   Vous souhaitez fournir aux employés de votre organisation un accès à distance au réseau de votre organisation.  
  
-   Vous souhaitez connecter des bureaux à différents emplacements physiques sur Internet.  
 
Cette rubrique, qui est destinée aux professionnels de l’informatique, fournit des informations générales sur la passerelle RAS, y compris les modes de déploiement et les fonctionnalités de la passerelle RAS. 
  
Cette rubrique contient les sections suivantes.  
  
  
-   [Modes de déploiement de la passerelle RAS](#bkmk_modes)  
  
-   [Mise en cluster de la passerelle RAS pour la haute disponibilité](#bkmk_clustering)  
  
-   [Fonctionnalités de la passerelle RAS](#bkmk_features)  
  
-   [Scénarios de déploiement de la passerelle RAS](#bkmk_deploy)  
  
-   [Outils de gestion de passerelle RAS](#bkmk_manage)  
  


  
## <a name="ras-gateway-deployment-modes"></a><a name="bkmk_modes"></a>Modes de déploiement de la passerelle RAS  
La passerelle RAS comprend les modes de déploiement suivants.  
  
### <a name="single-tenant-mode"></a>Mode de locataire unique  
Pour la plupart des organisations, l’utilisation de la passerelle RAS en mode mono-locataire est la configuration standard. En mode mono-client, vous pouvez déployer la passerelle RAS comme un serveur VPN Edge, un serveur DirectAccess Edge, ou les deux simultanément. Dans cette configuration, la passerelle RAS fournit aux employés distants une connectivité à votre réseau à l’aide de connexions VPN ou DirectAccess. En outre, le mode mono-locataire vous permet de connecter des bureaux à différents emplacements physiques sur Internet.  
  
### <a name="multitenant-mode"></a>Mode multi-locataire  
Si votre organisation est un fournisseur de services de chiffrement ou une entreprise avec plusieurs locataires, vous pouvez déployer la passerelle RAS en mode multi-locataire pour assurer le routage du trafic réseau vers et depuis des réseaux virtuels et physiques.  
  
L’architecture mutualisée est la capacité d’une infrastructure cloud à prendre en charge les charges de travail des machines virtuelles de plusieurs locataires, tout en les isolant les unes des autres, tandis que toutes les charges de travail s’exécutent sur la même infrastructure. Les multiples charges de travail d’un client seul peuvent être interconnectées et être gérées à distance mais ces systèmes n’offrent aucune interconnexion avec les charges de travail d’autres clients et ces derniers ne peuvent pas eux-mêmes les gérer à distance.  
  
Par exemple, une entreprise peut disposer de plusieurs sous-réseaux virtuels, chacun étant dédié à un service spécifique, par exemple Recherche et développement ou Comptabilité. Autre exemple : un CSP avec plusieurs clients qui utilisent des sous-réseaux virtuels isolés au sein du même centre de données physique. Dans les deux cas, la passerelle RAS peut acheminer le trafic vers et depuis chaque locataire tout en conservant l’isolation conçue de chaque locataire. Cette fonctionnalité permet à la passerelle RAS de prendre en charge le multi-locataire.  
  
Les réseaux virtuels sont créés à l’aide de la virtualisation de réseau Hyper-V, qui est une technologie introduite dans Windows Server 2012 et qui a été améliorée dans Windows Server 2016. La passerelle RAS est intégrée à la virtualisation de réseau Hyper-V et peut acheminer le trafic réseau efficacement dans les cas où il existe de nombreux clients ou locataires, qui ont des réseaux virtuels isolés dans le même centre de centres.  
  
La virtualisation de réseau Hyper-V vous permet de déployer un réseau d’ordinateurs virtuels indépendant du réseau physique sous-jacent. Avec les réseaux de machines virtuelles, qui sont composés d’un ou plusieurs sous-réseaux virtuels, l’emplacement physique exact d’un sous-réseau IP est dissocié de la topologie du réseau virtuel. Par conséquent, vous pouvez facilement déplacer vos sous-réseaux locaux vers le Cloud, tout en conservant vos adresses IP et votre topologie existantes dans le Cloud. Cette capacité à conserver l’infrastructure permet aux services existants de continuer à fonctionner, sans se préoccuper de l’emplacement physique des sous-réseaux. Autrement dit, la virtualisation de réseau Hyper-V permet de créer un cloud hybride transparent.  
  
> [!NOTE]  
> La virtualisation de réseau Hyper-V est une technologie de superposition de réseau utilisant l’encapsulation générique de routage de réseau ([NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)), qui permet aux locataires d’apporter leur propre espace d’adressage et permet aux fournisseurs de services de chiffrement une meilleure évolutivité qu’avec les réseaux locaux virtuels pour l’isolation des locataires.  
  
Dans Windows Server 2016, la passerelle RAS achemine le trafic réseau entre le réseau physique et les ressources du réseau de machines virtuelles, quel que soit l’emplacement des ressources. Vous pouvez utiliser la passerelle RAS pour acheminer le trafic réseau entre les réseaux physiques et virtuels au même emplacement physique ou à différents emplacements physiques.  
  
Par exemple, si vous avez un réseau physique et un réseau virtuel au même emplacement physique, vous pouvez déployer un ordinateur exécutant Hyper-V configuré avec une machine virtuelle de passerelle RAS pour agir en tant que passerelle de transfert et acheminer le trafic entre les serveurs virtuels et physiques. réseaux.  
  
Dans un autre exemple, si vos réseaux virtuels existent dans le Cloud, votre CSP peut déployer une passerelle RAS pour vous permettre de créer une connexion site à site de réseau privé virtuel (VPN) entre votre serveur VPN et la passerelle RAS du CSP. Lorsque ce lien est établi, vous pouvez vous connecter à vos ressources virtuelles dans le Cloud via la connexion VPN.  
  
Pour plus d’informations, consultez [haute disponibilité de la passerelle RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="clustering-ras-gateway-for-high-availability"></a><a name="bkmk_clustering"></a>Mise en cluster de la passerelle RAS pour la haute disponibilité  
La passerelle RAS est déployée sur un ordinateur dédié qui exécute Hyper-V et qui est configuré avec une machine virtuelle. La machine virtuelle est ensuite configurée en tant que passerelle RAS.  
  
Pour une haute disponibilité des ressources réseau, vous pouvez déployer une passerelle RAS avec basculement en utilisant deux serveurs hôtes physiques exécutant Hyper-V qui exécutent chacun une machine virtuelle configurée en tant que passerelle. Les ordinateurs virtuels passerelle sont ensuite configurés en cluster pour fournir la protection de basculement contre les pannes réseau et matérielles.  
  
Par exemple, si votre organisation est une entreprise avec un déploiement de cloud privé, vous pouvez avoir besoin de deux machines virtuelles de passerelle RAS, chacune d’elles étant installée sur un autre ordinateur exécutant Hyper-V. Dans ce scénario, les machines virtuelles de la passerelle RAS sont ajoutées à un cluster pour fournir une haute disponibilité.  
  
Dans un autre exemple, si votre organisation est un fournisseur de services Cloud (CSP) avec 200 locataires dans votre centre de donnes, vous pouvez utiliser huit machines virtuelles de passerelle RAS, chaque paire de machines virtuelles de passerelle RAS en cluster fournissant des services de routage pour les locataires 50. Dans ce scénario, deux ordinateurs exécutant Hyper-V disposent chacun de quatre machines virtuelles configurées en tant que passerelles RAS. Vous configurez ensuite quatre clusters de machines virtuelles de passerelle RAS, chaque cluster contenant une machine virtuelle à partir de chaque ordinateur exécutant Hyper-V.  
  
Lorsque vous déployez une passerelle RAS, les serveurs hôtes qui exécutent Hyper-V et les machines virtuelles que vous configurez en tant que passerelles doivent exécuter Windows Server 2012 R2 ou Windows Server 2016.  
  
## <a name="ras-gateway-features"></a><a name="bkmk_features"></a>Fonctionnalités de la passerelle RAS  
La passerelle RAS comprend les fonctionnalités suivantes.  
  
-   **VPN de site à site**. Cette fonctionnalité de passerelle RAS vous permet de connecter deux réseaux à des emplacements physiques différents sur Internet à l’aide d’une connexion VPN de site à site. Si vous disposez d’un siège social et de plusieurs succursales, vous pouvez déployer une passerelle RAS Edge à chaque emplacement et créer des connexions de site à site pour fournir un flux de trafic réseau entre les emplacements. Pour les fournisseurs de services de chiffrement qui hébergent de nombreux locataires dans leur centre de donnes, la passerelle RAS fournit une solution de passerelle mutualisée qui permet à vos locataires d’accéder à leurs ressources et de les gérer sur des connexions VPN de site à site à partir de sites distants, et qui autorise le flux de trafic réseau entre les ressources virtuelles de votre centre de ressources et leur réseau physique.  
  
-   **VPN de point à site**. Cette fonctionnalité de passerelle RAS permet aux employés ou aux administrateurs de l’organisation de se connecter au réseau de votre organisation à partir d’emplacements distants. Pour les déploiements à un seul client de la passerelle RAS, les employés distants peuvent se connecter au réseau de votre organisation à l’aide d’une connexion VPN. Cette connexion leur permet d’utiliser des ressources réseau internes, telles que des sites Web intranet et des serveurs de fichiers. Pour les déploiements multi-locataires, les administrateurs réseau de locataires peuvent utiliser des connexions VPN de point à site pour accéder aux ressources de réseau virtuel dans le centre de ressources CSP.  
  
-   **Routage dynamique avec Border Gateway Protocol (BGP)** . Le protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il s’agit d’un protocole de routage dynamique qui, de plus, découvre automatiquement les itinéraires entre les sites qui sont connectés via des connexions VPN de site à site. Si votre organisation possède plusieurs sites qui sont connectés à l’aide de routeurs compatibles BGP, tels que la passerelle RAS, le protocole BGP permet aux routeurs de calculer automatiquement et d’utiliser des itinéraires valides entre eux en cas d’interruption ou de défaillance du réseau. Pour plus d’informations, consultez la [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  
-   **Traduction d’adresses réseau (NAT)** . La traduction d’adresses réseau (NAT) vous permet de partager une connexion à l’Internet public par le biais d’une interface unique avec une adresse IP publique unique. Les ordinateurs sur le réseau privé utilisent des adresses privées non routables. NAT mappe les adresses privées à l’adresse publique. Cette fonctionnalité de passerelle RAS permet aux employés de l’organisation avec des déploiements à locataire unique d’accéder aux ressources Internet à partir de la passerelle. Pour les fournisseurs de services de chiffrement, cette fonctionnalité permet aux applications qui s’exécutent sur des machines virtuelles clientes d’accéder à Internet. Par exemple, une machine virtuelle cliente qui est configurée en tant que serveur Web peut contacter des ressources financières externes pour traiter les transactions de carte de crédit.  

  
## <a name="ras-gateway-deployment-scenarios"></a><a name="bkmk_deploy"></a>Scénarios de déploiement de la passerelle RAS  
Voici les scénarios de déploiement recommandés pour la passerelle RAS.  
  
-   **Enterprise Edge-déploiement à un seul client**. Avec le déploiement d’entreprise à client unique, vous pouvez connecter un physique à plusieurs autres emplacements physiques sur Internet à l’aide de la fonctionnalité VPN de site à site, et Border Gateway Protocol (BGP) vous permet d’utiliser le routage dynamique. Vous pouvez également fournir aux employés distants l’accès au réseau de votre organisation avec des connexions VPN point à site et des connexions DirectAccess. (Les connexions DirectAccess sont toujours activées et offrent également l’avantage de pouvoir gérer facilement les ordinateurs qui sont connectés à l’aide de DirectAccess, car ils sont connectés chaque fois qu’ils sont connectés et connectés à Internet.) Vous pouvez également configurer des passerelles RAS d’entreprise à client unique avec NAT, afin que les ordinateurs de votre intranet puissent communiquer facilement avec Internet.  
  
-   **Périphérie du fournisseur de services Cloud-déploiement multi-locataire**. La passerelle RAS déploiement multi-locataire pour les fournisseurs de services de chiffrement vous permet de proposer à vos locataires toutes les fonctionnalités disponibles avec le déploiement à client unique Enterprise Edge. Les connexions VPN de site à site entre les réseaux virtuels locataires dans votre centre de ressources et les emplacements réseau de locataire sur Internet signifient que les locataires ont tout le temps d’accéder en toute transparence à leurs ressources de Cloud. L’accès VPN de point à site pour les locataires signifie que les administrateurs clients peuvent toujours se connecter à leurs réseaux virtuels dans votre centre de ressources pour gérer leurs ressources. Le protocole BGP assure le routage dynamique et assure la connexion des locataires à leurs ressources même lorsque des problèmes réseau se produisent sur Internet ou ailleurs. NAT permet aux machines virtuelles clientes de se connecter à des ressources sur Internet, telles que des ressources de traitement de carte de crédit.  
  
## <a name="ras-gateway-management-tools"></a><a name="bkmk_manage"></a>Outils de gestion de passerelle RAS  
Voici les outils de gestion pour la passerelle RAS.  
  
-   Dans Windows Server 2016, vous devez utiliser les commandes Windows PowerShell pour déployer un routeur de passerelle RAS. Pour plus d’informations, consultez [applets de commande d’accès à distance](https://docs.microsoft.com/powershell/module/remoteaccess) pour windows server 2016 et Windows 10.  
  
-   Dans System Center 2012 R2 Virtual Machine Manager (VMM), la passerelle RAS est nommée passerelle Windows Server. Un ensemble limité d’options de configuration d’Border Gateway Protocol (BGP) sont disponibles dans l’interface logicielle de VMM, notamment l' **adresse IP BGP locale** et les **numéros de système autonomes (ASN)** , la **liste des adresses IP homologues BGP**et les **valeurs ASN**. Cependant, vous pouvez utiliser les commandes BGP Windows PowerShell d’accès à distance pour configurer toutes les autres fonctionnalités de la passerelle Windows Server. Pour plus d’informations, consultez [Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) et [applets de commande d’accès à distance](https://technet.microsoft.com/library/hh918399.aspx) pour Windows Server 2016 et Windows 10.  
  
## <a name="related-topics"></a>Rubriques connexes
- [Haute disponibilité de la passerelle RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Tunneling GRE dans Windows Server](gre-tunneling-windows-server.md)
- [Débit et performances du Tunnel GRE de la passerelle RAS](RAS-Gateway-GRE-Perf.md)
