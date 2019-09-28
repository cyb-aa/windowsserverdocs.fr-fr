---
title: Virtualisation de fonction réseau
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la virtualisation de fonction réseau, qui vous permet de déployer des appliances de réseau virtuel telles que le pare-feu de centre de informations, la passerelle RAS mutualisée et l’équilibrage de charge logiciel (SLB) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79df3bbe-48fd-4eff-8df6-35f6317566f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 338d5a285f2524932a91a66db186554cd0f50e2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355661"
---
# <a name="network-function-virtualization"></a>Virtualisation de fonction réseau

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la virtualisation de fonction réseau, qui vous permet de déployer des appliances de réseau virtuel, telles que le pare-feu de centre de informations, la passerelle RAS mutualisée et l’équilibrage de charge logiciel \(SLB @ no__t-1 multiplexer \(MUX @ NO_ _ t-3.
  
>[!NOTE]  
>En plus de cette rubrique, la documentation sur la virtualisation de fonction réseau suivante est disponible.  
> - [Présentation du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Passerelle du serveur d’accès à distance pour SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Équilibrage de la charge logicielle pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
Dans les centres de développement de logiciels actuels, les fonctions réseau qui sont effectuées par les appliances matérielles (par exemple, les équilibreurs de charge, les pare-feu, les routeurs, les commutateurs, etc.) sont de plus en plus virtualisées en tant qu’Appliances virtuelles. Cette « virtualisation des fonctions réseau » est une évolution naturelle de la virtualisation des serveurs et des réseaux. Les appliances virtuelles sont rapidement émergentes et créent un nouveau marché. Ils continuent à générer un intérêt et à gagner de l’énergie dans les plateformes de virtualisation et les services Cloud.  
  
Microsoft a inclus une passerelle autonome en tant qu’appliance virtuelle à partir de Windows Server 2012 R2. Pour plus d’informations, voir [Passerelle Windows Server](https://technet.microsoft.com/library/dn313101.aspx). Désormais, avec Windows Server 2016, Microsoft continue de développer et d’investir dans le marché de la virtualisation des fonctions réseau.  
  
## <a name="virtual-appliance-benefits"></a>Avantages des appliances virtuelles  
Une appliance virtuelle est dynamique et facile à modifier, car il s’agit d’une machine virtuelle prédéfinie et personnalisée. Il peut s’agir d’une ou plusieurs machines virtuelles empaquetées, mises à jour et gérées en tant qu’unité. Avec la mise en réseau SDN (Software Defined Networking), vous bénéficiez de l’agilité et de la flexibilité nécessaires dans l’infrastructure cloud d’aujourd’hui. Exemple :  
  
-   SDN présente le réseau sous la forme d’une ressource regroupée et dynamique.  
  
-   SDN facilite l’isolation des locataires.  
  
-   SDN optimise la mise à l’échelle et les performances.  
  
-   Les appliances virtuelles permettent l’extension de la capacité transparente et la mobilité des charges de travail.  
  
-   Les appliances virtuelles réduisent la complexité opérationnelle.  
  
-   Les appliances virtuelles permettent aux clients d’acquérir, de déployer et de gérer facilement des solutions pré-intégrées.  
  
    -   Les clients peuvent facilement déplacer l’appliance virtuelle n’importe où dans le Cloud.  
  
    -   Les clients peuvent mettre à l’échelle dynamiquement les appliances virtuelles en fonction de la demande.  
  
Pour plus d’informations sur Microsoft SDN, consultez [mise en réseau à définition logicielle](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Quelles sont les fonctions de réseau virtualisées ?  
La place de marché pour les fonctions de réseau virtualisé augmente rapidement. Les fonctions réseau suivantes sont virtualisées :  
  
-   **Sécurité**  
  
    -   Pare-feu  
  
    -   Antivirus  
  
    -   DDoS (déni de service distribué)  
  
    -   Adresses IP/IDS (système de prévention des intrusions/système de détection des intrusions)  
  
-   **Optimiseurs d’applications/WAN**  
  
-   **Edge**  
  
    -   Passerelle de site à site  
  
    -   Passerelles L3  
  
    -   Routeurs  
  
    -   Active  
  
    -   NAT  
  
    -   Équilibreurs de charge (pas nécessairement à la périphérie)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Pourquoi Microsoft est une plate-forme idéale pour les appliances virtuelles  
![Pile de réseau virtuel](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La plate-forme Microsoft a été conçue pour être une plate-forme idéale pour la création et le déploiement d’appliances virtuelles. Voici pourquoi:  
  
-   Microsoft fournit des fonctions clés de réseau virtualisé avec Windows Server 2016.  
  
-   Vous pouvez déployer une appliance virtuelle à partir du fournisseur de votre choix.  
  
-   Vous pouvez déployer, configurer et gérer vos appliances virtuelles avec le contrôleur de réseau Microsoft fourni avec Windows Server 2016. Pour plus d’informations sur le contrôleur de réseau, consultez [contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V peut héberger les principaux systèmes d’exploitation invités dont vous avez besoin.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualisation de fonction réseau dans Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Fonctions d’appliances virtuelles fournies par Microsoft  
Les appliances virtuelles suivantes sont fournies avec Windows Server 2016 :  
  
**Équilibreur de charge logiciel**  
  
Un équilibreur de charge de couche 4 fonctionnant au niveau du centre de l’échelle. Il s’agit d’une version similaire de l’équilibrage de charge d’Azure qui a été déployée à l’échelle dans l’environnement Azure. Pour plus d’informations sur les Load Balancer logicielles Microsoft, voir la rubrique [équilibrage de charge logicielle (SLB) pour SDN](https://technet.microsoft.com/library/mt632286.aspx). Pour plus d’informations sur les services d’équilibrage de charge Microsoft Azure, consultez [Microsoft Azure services d’équilibrage de charge](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Passerelle**. La passerelle RAS fournit toutes les combinaisons des fonctions de passerelle suivantes.  
  
-   **Passerelle de site à site**  
  
    La passerelle RAS fournit une passerelle mutualisée prenant en charge les Border Gateway Protocol (BGP) qui permet à vos locataires d’accéder à leurs ressources et de les gérer sur des connexions VPN de site à site à partir de sites distants, et qui autorise le flux de trafic réseau entre les ressources virtuelles dans les réseaux physiques du Cloud et du locataire. Pour plus d’informations sur la passerelle RAS, consultez [haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle RAS](https://technet.microsoft.com/library/mt626650.aspx)de la passerelle RAS.  
  
-   **Passerelle de transfert**  
  
    La passerelle RAS achemine le trafic entre les réseaux virtuels et le réseau physique du fournisseur d’hébergement. Par exemple, si les locataires créent un ou plusieurs réseaux virtuels et ont besoin d’accéder à des ressources partagées sur le réseau physique du fournisseur d’hébergement, la passerelle de transfert peut acheminer le trafic entre le réseau virtuel et le réseau physique pour permettre aux utilisateurs de travailler sur le réseau virtuel avec les services dont il a besoin. Pour plus d’informations, consultez haute disponibilité et [passerelle RAS](https://technet.microsoft.com/library/mt626650.aspx)de la [passerelle RAS](https://technet.microsoft.com/library/mt631692.aspx) .  
  
-   **Passerelles de tunnel GRE**  
  
    Les tunnels GRE activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Étant donné que le protocole GRE est léger et que la prise en charge de GRE est disponible sur la plupart des périphériques réseau, il constitue un choix idéal pour le tunneling où le chiffrement des données n’est pas nécessaire. La prise en charge GRE dans les tunnels de site à site (S2S) résout le problème de transfert entre les réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée. Pour plus d’informations sur les tunnels GRE, voir [tunneling GRE dans Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Plan de contrôle du routage avec BGP**  
  
Le contrôle de routage de la virtualisation de réseau Hyper-V (HNV) est l’entité logique et centralisée dans le plan de contrôle, qui contient tous les itinéraires des plans d’adresses des clients et qui apprend et met à jour les routeurs de la passerelle RAS distribuée dans le réseau virtuel. Pour plus d’informations, consultez haute disponibilité et [passerelle RAS](https://technet.microsoft.com/library/mt626650.aspx)de la [passerelle RAS](https://technet.microsoft.com/library/mt631692.aspx) .  
  
**Pare-feu multi-locataire distribué**  
  
Le pare-feu protège la couche réseau des réseaux virtuels. Les stratégies sont appliquées au port SDN-vSwitch de chaque machine virtuelle cliente. Il protège tous les flux de trafic : est-ouest et Nord-Sud. Les stratégies sont envoyées via le portail du locataire et le contrôleur de réseau les distribue à tous les ordinateurs hôtes concernés. Pour plus d’informations sur le pare-feu multi-locataire distribué, consultez [vue d’ensemble du pare-feu de centre](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)de données.  
  


