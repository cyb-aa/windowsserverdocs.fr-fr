---
title: Passerelle du serveur d’accès à distance pour SDN
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la passerelle RAS, qui est basé sur un logiciel, mutualisée, routeur capable de protocole BGP (Border Gateway) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4f1ad0b3f0b5921a53faa8a45baae9f0b8711873
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856270"
---
# <a name="ras-gateway-for-sdn"></a>Passerelle du serveur d’accès à distance pour SDN

>S’applique à : Windows Server (canal semi-annuel), la passerelle de Windows Server 2016 ## RAS pour SDN  


Passerelle RAS est un multilocataire basé sur le logiciel, le routeur capable de protocole BGP (Border Gateway) conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels client à l’aide de la virtualisation de réseau Hyper-V. Les passerelles RAS achemine le trafic réseau entre le réseau physique et les ressources du réseau de machine virtuelle, quel que soit l’emplacement. Vous pouvez acheminer le trafic réseau au même emplacement physique ou de plusieurs emplacements.   

L’architecture mutualisée est la possibilité d’une infrastructure de cloud pour prendre en charge les charges de travail de machine virtuelle de plusieurs locataires, encore les isoler les unes aux autres, tandis que toutes les charges de travail exécutées sur la même infrastructure. Les multiples charges de travail d’un client seul peuvent être interconnectées et être gérées à distance mais ces systèmes n’offrent aucune interconnexion avec les charges de travail d’autres clients et ces derniers ne peuvent pas eux-mêmes les gérer à distance.

  
> [!NOTE]  
> Outre cette rubrique, les rubriques suivantes de la passerelle RAS sont disponibles.  
>   
> -   [Nouveautés de la passerelle RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Architecture de déploiement de passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Passerelle RAS haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Protocole de passerelle frontière &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Référence des commandes PowerShell BGP Windows](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Conditions requises pour installer la passerelle RAS pour SDN  
Vous ne pouvez pas utiliser l’interface Windows pour installer l’accès à distance lorsque vous souhaitez déployer la passerelle RAS en mode partagé pour une utilisation avec SDN. Au lieu de cela, vous devez utiliser Windows PowerShell.  
  
Mais avant que vous pouvez installer la passerelle RAS à l’aide de Windows PowerShell, vous devez utiliser Windows PowerShell pour ajouter le **RemoteAccess** fonctionnalité de Windows. Pour ce faire, exécutez la commande suivante à l’invite Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Cette commande ajoute le **RemoteAccess** fonctionnalité et les commandes Windows PowerShell pour la fonctionnalité.  
  
Une fois que vous avez ajouté **RemoteAccess** à votre serveur, vous pouvez installer l’accès à distance comme une passerelle RAS avec le mode partagé et le protocole BGP (Border Gateway).  
  
Pour plus d’informations, consultez la rubrique de référence Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Fonctionnalités de passerelle RAS  
Voici les fonctionnalités de passerelle RAS dans Windows Server 2016. Vous pouvez déployer la passerelle RAS dans des pools de haute disponibilité qui utilisent toutes ces fonctionnalités en même temps.  
  
-   **VPN de site à site**. Cette fonctionnalité de passerelle RAS permet de connecter deux réseaux situés à des emplacements physiques différents via Internet à l’aide d’une connexion VPN de site à site. Pour les CSP qui héberge de nombreux clients dans leur centre de données, passerelle RAS fournit une solution de passerelle mutualisée qui permet aux clients accéder et gérer leurs ressources sur les connexions VPN de site à site depuis des sites distants, et qui autorise le flux de trafic réseau entre ressources virtuelles dans votre centre de données et leur réseau physique.  
  
-   **Point-to-site VPN**. Cette fonctionnalité de passerelle RAS permet aux employés d’organisation ou aux administrateurs de se connecter au réseau de votre organisation à partir d’emplacements distants.  Pour des déploiements mutualisés, client permet aux administrateurs réseau connexions de point-to-site VPN pour accéder aux ressources de réseau virtuel dans le centre de données CSP.  
  
-   **Tunneling GRE**. Encapsulation GRE (Generic Routing) en fonction des tunnels activent la connectivité entre réseaux virtuels clients et les réseaux externes. Dans la mesure où le protocole GRE est léger et que sa prise en charge est disponible sur la plupart des périphériques réseau, il constitue un choix idéal pour le tunneling, dans lequel le chiffrement de données n’est pas nécessaire. Prise en charge GRE dans les tunnels de Site à Site (S2S) résout le problème de transfert entre réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée, comme décrit plus loin dans cette rubrique.  
  
-   **Routage dynamique avec protocole BGP (Border Gateway)**. Le protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il s’agit d’un protocole de routage dynamique qui, de plus, découvre automatiquement les itinéraires entre les sites qui sont connectés via des connexions VPN de site à site. Si votre organisation possède plusieurs sites qui sont connectés à l’aide de routeurs BGP comme passerelle RAS, BGP permet les routeurs calculer automatiquement et d’utiliser des itinéraires valides entre eux en cas d’interruption du réseau ou l’échec. Pour plus d’informations, consultez [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


