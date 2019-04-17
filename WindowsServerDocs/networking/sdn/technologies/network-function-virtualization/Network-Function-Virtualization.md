---
title: Virtualisation de fonction réseau
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la virtualisation de fonction réseau, ce qui vous permet de déployer des appareils de mise en réseau virtuels comme pare-feu de centre de données, cette passerelle mutualisée et l’équilibrage de charge logiciel (SLB) dans Windows Server 2016.
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
ms.openlocfilehash: 7caa9eef42cb7ab95a13d64c1dcd3639b1132eb3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-function-virtualization"></a>Virtualisation de fonction réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la virtualisation de fonction réseau, ce qui vous permet de déployer des équipements réseau virtuels tels que les pare-feu de centre de données, mutualisée passerelle et \(SLB\) l’équilibrage de charge logicielle \(MUX\) multiplexeur.
  
>[!NOTE]  
>Outre cette rubrique, la documentation de la virtualisation de fonction réseau suivante est disponible.  
> - [Vue d’ensemble du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)  
> - [Passerelle pour SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)  
> - [Logiciel équilibrage de charge (SLB) pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)  
  
Dans le logiciel d’aujourd'hui centres de données définies, les fonctions réseau effectuées par les appliances matérielles (équilibreurs de charge, pare-feu, routeurs, commutateurs, etc.) sont plus en plus en cours virtualisés comme des équipements virtuels. Cette «virtualisation de fonction réseau» est une évolution naturelle de la virtualisation de serveur et de la virtualisation de réseau. Équipements virtuels sont rapidement émergents et création d’un tout nouveau marché. Ils continuent à susciter l’intérêt et de gagner du terrain dans les deux plateformes de virtualisation et de services cloud.  
  
Microsoft a inclus une passerelle autonome comme appliance virtuelle à partir de Windows Server 2012 R2. Pour plus d’informations, voir [passerelle Windows Server](https://technet.microsoft.com/library/dn313101.aspx). Désormais avec Windows Server 2016 Microsoft continue à développer et à investir dans le marché de la virtualisation de fonction réseau.  
  
## <a name="virtual-appliance-benefits"></a>Avantages d’appliance virtuelle  
Une appliance virtuelle est dynamique et facile à modifier, car il s’agit d’un ordinateur virtuel prédéfinie, personnalisé. Il peut être un ou plusieurs ordinateurs virtuels empaquetées, mis à jour et gérées en tant qu’unité. Avec le logiciel défini de mise en réseau (SDN), vous obtenez la flexibilité et la flexibilité nécessaire dans l’infrastructure de cloud d’aujourd'hui. Par exemple:  
  
-   SDN présente le réseau sous la forme d’une ressource dynamique et mis en pool.  
  
-   SDN facilite l’isolation des locataires.  
  
-   SDN optimise les performances et la montée en puissance parallèle.  
  
-   Équipements virtuels mobilité transparente de capacité d’extension et la charge de travail.  
  
-   Équipements virtuels réduire la complexité opérationnelle.  
  
-   Équipements virtuels permettent aux clients facilement acquérir, déployer et gérer des solutions pré-intégrées.  
  
    -   Les clients peuvent facilement transférer l’appliance virtuelle n’importe où dans le cloud.  
  
    -   Les clients capable d’évoluer équipements virtuels ou vers le bas dynamiquement basé sur la demande.  
  
Pour plus d’informations sur Microsoft SDN voir [logiciel défini de mise en réseau](https://technet.microsoft.com/windows-server-docs/networking/sdn/software-defined-networking--sdn-).  
  
### <a name="what-network-functions-are-being-virtualized"></a>Les fonctions réseau sont virtualisées?  
Le marché pour les fonctions de réseau virtualisé croît rapidement. Les fonctions réseau suivantes sont en cours virtualisées:  
  
-   **Sécurité**  
  
    -   Pare-feu  
  
    -   Antivirus  
  
    -   DDoS (déni de Service distribué)  
  
    -   Adresses IP/ID (système de détection des intrusions prévention système/Intrusion)  
  
-   **Optimiseurs de l’application/WAN**  
  
-   **Edge**  
  
    -   Passerelle de site à site  
  
    -   Passerelles L3  
  
    -   Routeurs  
  
    -   Commutateurs  
  
    -   NAT  
  
    -   Équilibreurs de charge (et pas nécessairement à la périphérie)  
  
    -   Proxy HTTP  
  
## <a name="why-microsoft-is-a-great-platform-for-virtual-appliances"></a>Pourquoi Microsoft est une excellente plateforme pour des équipements virtuels  
![Pile réseau virtuel](../../../media/Network-Function-Virtualization/Microsoft-Network-Function-Virtualization.png)  
  
La plateforme Microsoft a été conçue pour être une excellente plateforme pour générer et déployer des équipements virtuels. Voici pourquoi:  
  
-   Microsoft fournit des fonctions de clé réseau virtualisé avec Windows Server 2016.  
  
-   Vous pouvez déployer une appliance virtuelle à partir du fournisseur de votre choix.  
  
-   Vous pouvez déployer, configurer et gérer votre équipements virtuels avec le contrôleur de réseau Microsoft fourni avec Windows Server 2016. Pour plus d’informations sur le contrôleur de réseau, voir [contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md).  
  
-   Hyper-V peut héberger les systèmes d’exploitation invité supérieur dont vous avez besoin.  
  
## <a name="network-function-virtualization-in-windows-server-2016"></a>Virtualisation de fonction réseau dans Windows Server 2016  
  
### <a name="virtual-appliances-functions-provided-by-microsoft"></a>Fonctions d’équipements virtuels fournies par Microsoft  
Les équipements virtuels suivants sont fournis avec Windows Server 2016:  
  
**Équilibreur de charge logicielle**  
  
Un équilibreur de charge de 4 à la couche d’exploitation à l’échelle du centre de données. Il s’agit d’une version similaire d’équilibrage de charge d’Azure qui a été déployé à l’échelle de l’environnement Azure. Pour plus d’informations sur l’équilibrage de charge du logiciel Microsoft, consultez [l’équilibrage de charge logiciels (SLB) pour SDN](https://technet.microsoft.com/library/mt632286.aspx). Pour plus d’informations sur les Services de l’équilibrage de charge Microsoft Azure, voir [Microsoft Azure Load Balancing Services](https://azure.microsoft.com/blog/2014/04/08/microsoft-azure-load-balancing-services/).  
  
**Passerelle**. Cette passerelle fournit toutes les combinaisons des fonctions suivantes de la passerelle.  
  
-   **Passerelle de Site à site**  
  
    Cette passerelle fournit un protocole BGP (Border Gateway)-partagé, capable de passerelle qui permet aux clients accéder et gérer leurs ressources sur les connexions VPN de site à site à partir de sites distants, et qui autorise le flux de trafic réseau entre les ressources virtuelles dans les cloud et les clients des réseaux physiques. Pour plus d’informations sur la passerelle, voir [RAS passerelle haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Passerelle de transfert**  
  
    Cette passerelle achemine le trafic entre les réseaux virtuels et le réseau physique fournisseur d’hébergement. Par exemple, si les clients créer un ou plusieurs réseaux virtuels et ont besoin d’accéder aux ressources partagées sur le réseau physique sur le fournisseur d’hébergement, la passerelle de transfert peut acheminer le trafic entre le réseau virtuel et le réseau physique pour fournir aux utilisateurs qui travaillent sur le réseau virtuel avec les services dont ils ont besoin. Pour plus d’informations, voir [RAS passerelle haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle](https://technet.microsoft.com/library/mt626650.aspx).  
  
-   **Passerelles de tunneling GRE**  
  
    GRE en fonction des tunnels activent la connectivité entre les réseaux virtuels clients et les réseaux externes. Dans la mesure où le protocole GRE est léger et que la prise en charge est disponible sur la plupart des périphériques réseau, il devient un choix idéal pour le tunneling, où le chiffrement des données n’est pas nécessaire. Prise en charge GRE dans les tunnels de Site à Site (S2S) résout le problème de transfert entre les réseaux virtuels clients et les réseaux externes clients à l’aide d’une passerelle mutualisée. Pour plus d’informations sur les tunnels GRE, voir [Tunneling GRE dans Windows Server 2016](https://technet.microsoft.com/library/dn765485.aspx).  
  
**Routage plan de contrôle avec protocole BGP**  
  
Contrôle de routage de la virtualisation de réseau Hyper-V (HNV) est l’entité logique et centralisée dans le plan de contrôle, qui exécute tous les itinéraires de plan d’adressage du client et apprend dynamiquement et met à jour les routeurs passerelle distribués dans le réseau virtuel. Pour plus d’informations, voir [RAS passerelle haute disponibilité](https://technet.microsoft.com/library/mt631692.aspx) et [passerelle](https://technet.microsoft.com/library/mt626650.aspx).  
  
**Pare-feu mutualisé distribué**  
  
Le pare-feu protège la couche réseau de réseaux virtuels. Les stratégies sont appliquées au port SDN-vSwitch de chaque ordinateur virtuel du client. Elle protège tous les flux de trafic: est-ouest et nord sud. Les stratégies sont transmises par le biais du portail des locataires et le contrôleur de réseau les distribue à tous les hôtes applicables. Pour plus d’informations sur le pare-feu mutualisé distribué, voir [vue d’ensemble du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).  
  


