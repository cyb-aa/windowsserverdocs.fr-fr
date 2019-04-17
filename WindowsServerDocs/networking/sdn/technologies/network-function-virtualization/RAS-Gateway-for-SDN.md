---
title: Passerelle pour SDN
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur cette passerelle, qui est basé sur un logiciel, mutualisée, routeur capable de protocole BGP (Border Gateway) dans Windows Server2016.
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
ms.openlocfilehash: 052911dcd52df82ef4e259de0c64078c54f00195
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-for-sdn"></a>Passerelle pour SDN

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la passerelle, qui est un logiciel, mutualisé, protocole BGP (Border Gateway) compatible avec routeur dans Windows Server2016 est conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels clients à l’aide de la virtualisation de réseau Hyper-V.  
  
> [!NOTE]  
> Outre cette rubrique, les rubriques de cette passerelle suivantes sont disponibles.  
>   
> -   [Nouveautés de la passerelle](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Architecture de déploiement de passerelle RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [RAS passerelle haute disponibilité](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Border Gateway Protocol et #40; protocole BGP et #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Référence de commande PowerShell BGP Windows](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
Dans Windows Server2016, cette passerelle achemine le trafic réseau entre le réseau physique et les ressources réseau d’ordinateurs virtuels, quel que soit l’emplacement où se trouvent les ressources. Vous pouvez utiliser cette passerelle pour acheminer le trafic réseau entre les réseaux physiques et virtuels au même emplacement physique ou à plusieurs emplacements physiques via Internet.  
  
Architecture mutualisée est la possibilité d’une infrastructure cloud pour prendre en charge les charges de travail de machine virtuelle de plusieurs clients, encore les isoler les unes des autres, alors que toutes les charges de travail exécutées sur la même infrastructure. Plusieurs charges de travail d’un client seul peuvent être interconnectées et être gérés à distance, mais ces systèmes pas d’interconnexion avec les charges de travail d’autres clients, ni peuvent autres locataires les gérer à distance.  
  
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Configuration requise pour l’installation de la passerelle pour SDN  
Vous ne pouvez pas utiliser l’interface Windows pour installer l’accès à distance lorsque vous souhaitez déployer cette passerelle en mode mutualisé pour une utilisation avec SDN. Au lieu de cela, vous devez utiliser Windows PowerShell.  
  
Mais avant d’installer cette passerelle à l’aide de Windows PowerShell, vous devez utiliser Windows PowerShell pour ajouter le **RemoteAccess** fonctionnalité de Windows. Pour ce faire, exécutez la commande suivante à l’invite Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Cette commande ajoute le **RemoteAccess** fonctionnalité et les commandes Windows PowerShell pour la fonctionnalité.  
  
Après avoir ajouté **RemoteAccess** à votre serveur, vous pouvez installer l’accès à distance en tant que passerelle RAS avec le mode mutualisé et le protocole BGP (Border Gateway).  
  
Pour plus d’informations, voir la rubrique de référence Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Fonctionnalités de passerelle RAS  
Voici les fonctionnalités de passerelle dans Windows Server2016. Vous pouvez déployer la passerelle dans les pools de haute disponibilité qui utilisent toutes ces fonctionnalités à la fois.  
  
-   **VPN de site à site**. Cette fonctionnalité cette passerelle vous permet de vous connecter deux réseaux emplacements physiques sur Internet à l’aide d’une connexion VPN de site à site. Pour les CSP qui hébergent de nombreux clients dans leur centre de données, cette passerelle fournit une solution de passerelle mutualisée qui permet aux clients accéder et gérer leurs ressources sur les connexions VPN de site à site à partir de sites distants, et qui autorise le flux de trafic réseau entre les ressources virtuelles dans votre centre de données et leur réseau physique.  
  
-   **VPN de site à point**. Cette fonctionnalité de passerelle permet des employés de l’organisation ou administrateurs de se connecter au réseau de votre organisation à partir d’emplacements distants.  Pour des déploiements mutualisés, client permet aux administrateurs réseau les connexions VPN point à site pour accéder aux ressources du réseau virtuel du centre de données du fournisseur de services cryptographiques.  
  
-   **Tunneling GRE**. Encapsulation GRE (Generic Routing) en fonction des tunnels activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Dans la mesure où le protocole GRE est léger et que la prise en charge est disponible sur la plupart des périphériques réseau, il devient un choix idéal pour le tunneling, où le chiffrement des données n’est pas nécessaire. Prise en charge GRE dans les tunnels de Site à Site (S2S) résout le problème de transfert entre les réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée, comme décrit plus loin dans cette rubrique.  
  
-   **Routage dynamique avec protocole BGP (Border Gateway)**. Protocole BGP réduit le besoin de configuration manuelle de routage sur les routeurs, car il est un protocole de routage dynamique et découvre automatiquement les itinéraires entre les sites qui sont connectés à l’aide de connexions VPN site à site. Si votre organisation possède plusieurs sites qui sont connectés à l’aide de routeurs BGP comme passerelle, BGP permet de routeurs calculer automatiquement pour utiliser des itinéraires valides entre eux en cas d’interruption du réseau ou l’échec. Pour plus d’informations, voir [RFC4271](https://tools.ietf.org/html/rfc4271).  
  

  


