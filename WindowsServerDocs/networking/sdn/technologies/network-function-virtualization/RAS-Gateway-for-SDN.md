---
title: Passerelle du serveur d’accès à distance pour SDN
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la passerelle RAS, qui est un routeur de type logiciel, multilocataire, Border Gateway Protocol (BGP) qui prend en charge Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 415a5f4728b1b08e59630935e47f22d74b0267e8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312931"
---
# <a name="ras-gateway-for-sdn"></a>Passerelle du serveur d’accès à distance pour SDN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016 # # passerelle RAS pour SDN  


La passerelle RAS est un routeur de type logiciel, multilocataire, Border Gateway Protocol (BGP) conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels de locataire à l’aide de la virtualisation de réseau Hyper-V. Les passerelles RAS acheminent le trafic réseau entre le réseau physique et les ressources du réseau de machines virtuelles, quel que soit l’emplacement. Vous pouvez acheminer le trafic réseau au même emplacement physique ou à de nombreux emplacements différents.   

L’architecture mutualisée est la capacité d’une infrastructure cloud à prendre en charge les charges de travail des machines virtuelles de plusieurs locataires, tout en les isolant les unes des autres, tandis que toutes les charges de travail s’exécutent sur la même infrastructure. Les multiples charges de travail d’un client seul peuvent être interconnectées et être gérées à distance mais ces systèmes n’offrent aucune interconnexion avec les charges de travail d’autres clients et ces derniers ne peuvent pas eux-mêmes les gérer à distance.

  
> [!NOTE]  
> En plus de cette rubrique, les rubriques suivantes relatives à la passerelle RAS sont disponibles.  
>   
> -   [Nouveautés de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Architecture de déploiement de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Haute disponibilité de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Référence à la commande Windows PowerShell BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Conditions préalables à l’installation de la passerelle RAS pour SDN  
Vous ne pouvez pas utiliser l’interface Windows pour installer l’accès à distance lorsque vous souhaitez déployer une passerelle RAS en mode multi-locataire pour une utilisation avec SDN. Au lieu de cela, vous devez utiliser Windows PowerShell.  
  
Mais avant de pouvoir installer la passerelle RAS à l’aide de Windows PowerShell, vous devez utiliser Windows PowerShell pour ajouter la fonctionnalité Windows **RemoteAccess** . Pour ce faire, exécutez la commande suivante à l’invite de commandes Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Cette commande ajoute la fonctionnalité **RemoteAccess** et les commandes Windows PowerShell pour la fonctionnalité.  
  
Une fois que vous avez ajouté **RemoteAccess** à votre serveur, vous pouvez installer l’accès à distance en tant que passerelle ras avec le mode multilocataire et le Border Gateway Protocol (BGP).  
  
Pour plus d’informations, consultez la rubrique de référence Windows PowerShell [install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Fonctionnalités de la passerelle RAS  
Voici les fonctionnalités de la passerelle RAS dans Windows Server 2016. Vous pouvez déployer une passerelle RAS dans des pools à haute disponibilité qui utilisent toutes ces fonctionnalités à la fois.  
  
-   **VPN de site à site**. Cette fonctionnalité de passerelle RAS vous permet de connecter deux réseaux à des emplacements physiques différents sur Internet à l’aide d’une connexion VPN de site à site. Pour les fournisseurs de services de chiffrement qui hébergent de nombreux locataires dans leur centre de donnes, la passerelle RAS fournit une solution de passerelle mutualisée qui permet à vos locataires d’accéder à leurs ressources et de les gérer sur des connexions VPN de site à site à partir de sites distants, et qui autorise le flux de trafic réseau entre les ressources virtuelles de votre centre de ressources et leur réseau physique.  
  
-   **VPN de point à site**. Cette fonctionnalité de passerelle RAS permet aux employés ou aux administrateurs de l’organisation de se connecter au réseau de votre organisation à partir d’emplacements distants.  Pour les déploiements multi-locataires, les administrateurs réseau de locataires peuvent utiliser des connexions VPN de point à site pour accéder aux ressources de réseau virtuel dans le centre de ressources CSP.  
  
-   **Tunneling GRE**. Les tunnels basés sur l’encapsulation générique de routage (GRE) activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Dans la mesure où le protocole GRE est léger et que sa prise en charge est disponible sur la plupart des périphériques réseau, il constitue un choix idéal pour le tunneling, dans lequel le chiffrement de données n’est pas nécessaire. La prise en charge de GRE dans les tunnels de site à site (S2S) résout le problème de transfert entre les réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée, comme décrit plus loin dans cette rubrique.  
  
-   **Routage dynamique avec Border Gateway Protocol (BGP)** . Le protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il s’agit d’un protocole de routage dynamique qui, de plus, découvre automatiquement les itinéraires entre les sites qui sont connectés via des connexions VPN de site à site. Si votre organisation possède plusieurs sites qui sont connectés à l’aide de routeurs compatibles BGP, tels que la passerelle RAS, le protocole BGP permet aux routeurs de calculer automatiquement et d’utiliser des itinéraires valides entre eux en cas d’interruption ou de défaillance du réseau. Pour plus d’informations, consultez la [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


