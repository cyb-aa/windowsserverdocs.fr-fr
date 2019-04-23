---
title: Virtualisation de fonction réseau
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la virtualisation de fonction réseau, ce qui vous permet de déployer des appliances de réseau virtuels comme pare-feu de centre de données mutualisées passerelle RAS et l’équilibrage de charge logiciel (SLB) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59474a13d1cbce6a607f025caf3f6c1b839c7eed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884550"
---
# <a name="network-function-virtualization"></a>Virtualisation de fonction réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la virtualisation de fonction réseau, ce qui vous permet de déployer des appliances de réseau virtuels, les pare-feu de centre de données mutualisées passerelle RAS et l’équilibrage de charge logiciel \(SLB\) multiplexeur \(MUX\).
  
>[!NOTE]  
>Outre cette rubrique, la documentation suivante de la virtualisation de fonction réseau est disponible.  
> - [Vue d’ensemble du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Passerelle RAS pour SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Logiciel équilibrage de charge (SLB) pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
Dans les logiciels d’aujourd'hui centres de données définis, les fonctions de réseau qui sont effectuées par les appliances matérielles (par exemple, les équilibreurs de charge, les pare-feux, routeurs, commutateurs, etc.) sont plus en plus virtualisées, de façon appliances virtuelles. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux. Appliances virtuelles sont rapidement émergentes et créer un tout nouveau marché. Ils continuent à susciter l’intérêt et de gagner du terrain dans les deux plateformes de virtualisation et de services cloud.  
  
Microsoft a inclus une passerelle autonome en tant qu’une appliance virtuelle, en commençant par Windows Server 2012 R2. Pour plus d’informations, voir [Passerelle Windows Server](https://technet.microsoft.com/library/dn313101.aspx). Avec Windows Server 2016 Microsoft continue à développer et d’investir dans le marché de la virtualisation de réseau (fonction).  
  
## <a name="virtual-appliance-benefits"></a>Avantages de l’appliance virtuelle  
Une appliance virtuelle est dynamique et faciles à modifier car il s’agit d’une machine virtuelle prédéfinie et personnalisée. Il peut être une ou plusieurs machines virtuelles empaqueté, mis à jour et géré en tant qu’unité. Avec le logiciel défini de mise en réseau (SDN), vous obtenez l’agilité et la flexibilité nécessaire dans une infrastructure de cloud d’aujourd'hui. Exemple :  
  
-   SDN présente le réseau sous la forme d’une ressource dynamique et mis en pool.  
  
-   SDN facilite l’isolation des locataires.  
  
-   SDN optimise les performances et évolutivité.  
  
-   Appliances virtuelles mobilité transparente de capacité d’extension et de la charge de travail.  
  
-   Appliances virtuelles réduire la complexité opérationnelle.  
  
-   Appliances virtuelles permettent aux clients facilement acquérir, déployer et gérer des solutions pré-intégrées.  
  
    -   Les clients peuvent facilement transférer l’appliance virtuelle n’importe où dans le cloud.  
  
    -   Les clients peuvent monter en puissance appliances virtuelles ou vers le bas dynamiquement selon la demande.  
  
Pour plus d’informations sur Microsoft SDN consultez [Sdn](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Les fonctions de réseau sont en cours virtualisées ?  
La place de marché pour les fonctions de réseau virtualisé est croît rapidement. Les fonctions de réseau suivantes sont en cours de virtualisation :  
  
-   **Sécurité**  
  
    -   Pare-feu  
  
    -   Antivirus  
  
    -   DDoS (déni de Service distribué)  
  
    -   IPS/IDS (Intrusion Prevention System/Intrusion Detection System)  
  
-   **Application/WAN optimiseurs**  
  
-   **Edge**  
  
    -   Passerelle de site à site  
  
    -   Passerelles L3  
  
    -   Routeurs  
  
    -   Commutateurs  
  
    -   NAT  
  
    -   Équilibreurs de charge (mais pas nécessairement à la périphérie)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Pourquoi Microsoft est une plateforme idéale pour les appliances virtuelles  
![Pile de réseau virtuel](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La plate-forme Microsoft a été conçue pour être une excellente plateforme pour générer et déployer des appliances virtuelles. Voici pourquoi :  
  
-   Microsoft fournit des fonctions clés de réseau virtualisé avec Windows Server 2016.  
  
-   Vous pouvez déployer une appliance virtuelle du fournisseur de votre choix.  
  
-   Vous pouvez déployer, configurer et gérer vos appliances virtuelles avec le contrôleur de réseau de Microsoft qui est fourni avec Windows Server 2016. Pour plus d’informations sur le contrôleur de réseau, consultez [contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V peut héberger les systèmes d’exploitation invité supérieur dont vous avez besoin.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualisation de fonction réseau dans Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Fonctions d’appliances virtuelles fournies par Microsoft  
Les appliances virtuelles suivantes sont fournies avec Windows Server 2016 :  
  
**Équilibreur de charge logiciel**  
  
Un équilibreur de charge de couche 4 d’exploitation à l’échelle du centre de données. Il s’agit d’une version identique de l’équilibreur de charge d’Azure qui a été déployé à l’échelle dans l’environnement Azure. Pour plus d’informations sur l’équilibrage de charge du logiciel Microsoft, consultez [d’équilibrage de charge logiciel (SLB) pour SDN](https://technet.microsoft.com/library/mt632286.aspx). Pour plus d’informations sur les Services d’équilibrage de charge Microsoft Azure, consultez [Microsoft Azure Load Balancing Services](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Passerelle**. Passerelle RAS fournit toutes les combinaisons des fonctions de passerelle suivantes.  
  
-   **Passerelle de site à Site**  
  
    Passerelle RAS fournit un protocole BGP (Border Gateway)-partagé, capable de passerelle qui permet aux clients accéder et gérer leurs ressources sur les connexions VPN de site à site depuis des sites distants, et qui autorise le flux de trafic réseau entre les ressources virtuelles dans les réseaux physiques cloud et le client. Pour plus d’informations sur la passerelle RAS, consultez [passerelle RAS haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Passerelle de transfert**  
  
    Passerelle RAS achemine le trafic entre réseaux virtuels et le réseau physique fournisseur d’hébergement. Par exemple, si les locataires de créer un ou plusieurs réseaux virtuels et ont besoin d’accéder aux ressources partagées sur le réseau physique au fournisseur d’hébergement, la passerelle de transfert peut acheminer le trafic entre le réseau virtuel et le réseau physique pour fournir aux utilisateurs qui travaillent sur le réseau virtuel avec les services dont ils ont besoin. Pour plus d’informations, consultez [passerelle RAS haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Passerelles de tunnel GRE**  
  
    Les tunnels GRE activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Étant donné que le protocole GRE est léger et prise en charge est disponible sur la plupart des périphériques réseau, il devient un choix idéal pour le tunneling, où le chiffrement de données n’est pas nécessaire. La prise en charge GRE dans les tunnels de site à site (S2S) résout le problème de transfert entre les réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée. Pour plus d’informations sur les tunnels GRE, consultez [Tunneling GRE dans Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Plan de contrôle de routage avec le protocole BGP**  
  
Contrôle de routage de virtualisation de réseau Hyper-V (HNV) est l’entité logique et centralisée dans le plan de contrôle, qui comporte tous les itinéraires de plan d’adressage du client et apprend dynamiquement, puis met à jour les routeurs de passerelle RAS distribués dans le réseau virtuel. Pour plus d’informations, consultez [passerelle RAS haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle RAS](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Pare-feu d’architecture mutualisée distribué**  
  
Le pare-feu protège la couche réseau des réseaux virtuels. Les stratégies sont appliquées au niveau du port de commutateur virtuel vSwitch SDN de chaque machine virtuelle de locataire. Il protège tous les flux de trafic : est-ouest et nord-sud. Les stratégies sont envoyées via le portail du locataire et le contrôleur de réseau les distribue à tous les hôtes applicables. Pour plus d’informations sur le pare-feu distribué architecture mutualisé, consultez [vue d’ensemble du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


