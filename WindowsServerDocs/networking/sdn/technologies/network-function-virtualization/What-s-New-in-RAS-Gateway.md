---
title: Nouveautés de la passerelle du serveur d’accès à distance
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les nouvelles fonctionnalités de la passerelle RAS, qui est un routeur prenant en charge les Border Gateway Protocol (BGP) basé sur le logiciel dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85595c47c599a72039e93e67ea2f33f92af7200c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405900"
---
# <a name="whats-new-in-ras-gateway"></a>Nouveautés de la passerelle du serveur d’accès à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les nouvelles fonctionnalités de la passerelle RAS, qui est un routeur prenant en charge les Border Gateway Protocol (BGP) basé sur le logiciel dans Windows Server 2016. Le routeur BGP multilocataire de la passerelle RAS est conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels de locataire à l’aide de la virtualisation de réseau Hyper-V.  
  
> [!NOTE]  
> Dans Windows Server 2012 R2, la passerelle RAS est nommée passerelle RRAS. et dans System Center Virtual Machine Manager, la passerelle RAS est nommée passerelle Windows Server.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Options de connectivité de site à site](#bkmk_s2s)  
  
-   [Pools de passerelle](#bkmk_pools)  
  
-   [Extensibilité du pool de passerelles](#bkmk_gps)  
  
-   [Redondance du pool de passerelle M + N](#bkmk_m)  
  
-   [Réflecteur d’itinéraire](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Options de connectivité de site à site  
La passerelle RAS prend désormais en charge trois types de connexions VPN de site à site :  Les tunnels VPN (réseau privé virtuel) de site à site (VPN), de couche 3 (L3) et d’encapsulation générique de routage (GRE) de protocole IKE (Internet Key Exchange) version 2 (IKEv2).  
  
Pour plus d’informations sur GRE, voir [tunneling GRE dans Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Pools de passerelle  
Dans Windows Server 2016, vous pouvez créer des pools de passerelle de types différents. Les pools de passerelle contiennent de nombreuses instances de passerelle RAS et acheminent le trafic réseau entre les réseaux physiques et virtuels. Les pools de passerelle peuvent exécuter n’importe quelle fonction de passerelle, à protocole IKE (Internet Key Exchange) version 2 (IKEv2), un réseau privé virtuel (VPN) de site à site, un VPN de couche 3 (L3) et des tunnels GRE (Generic Routing Encapsulation), ou le pool peut effectuer toutes ces opérations Functions et jouent le rôle de pool mixte.  
  
Vous pouvez créer des pools de passerelle à l’aide de n’importe quelle logique que vous préférez en fonction de vos besoins en matière d’infrastructure. Par exemple, vous pouvez créer des pools de passerelle basés sur l’une des caractéristiques suivantes.  
  
-   Types de tunnels (VPN IKEv2, VPN L3, VPN GRE)  
  
-   Capacité  
  
-   Niveau de redondance (fiabilité basé sur votre plan de facturation pour les locataires)  
  
-   Séparation personnalisée pour les clients  
  
Pour plus d’informations, consultez [haute disponibilité de la passerelle RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Extensibilité du pool de passerelles  
Vous pouvez facilement mettre à l’échelle un pool de passerelle en ajoutant ou en supprimant des machines virtuelles de passerelle dans le pool. La suppression ou l’ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer des pools complets de passerelles.  
  
Pour plus d’informations, consultez [haute disponibilité de la passerelle RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Redondance du pool de passerelle M + N  
Chaque pool de passerelle est M + N redondant. Cela signifie qu’un nombre « m » d’ordinateurs virtuels de passerelle active (VM) est sauvegardé par un nombre « N » de machines virtuelles de passerelle de secours. La redondance M + N offre une plus grande flexibilité pour déterminer le niveau de fiabilité dont vous avez besoin lorsque vous déployez la passerelle RAS. Au lieu d’utiliser une seule passerelle RAS de secours par machine virtuelle de passerelle RAS active, qui est la seule option de configuration avec Windows Server 2012 R2, vous pouvez désormais configurer autant de machines virtuelles de secours que nécessaire. La fonctionnalité de Service Manager de passerelle du contrôleur de réseau utilise efficacement la capacité de la machine virtuelle de la passerelle RAS de secours pour fournir un basculement fiable si une machine virtuelle de passerelle RAS active échoue ou perd la connectivité.  
  
Pour plus d’informations, consultez [haute disponibilité de la passerelle RAS](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Réflecteur d’itinéraire  
Le réflecteur d’itinéraire Border Gateway Protocol (BGP) est désormais inclus dans la passerelle RAS et fournit une alternative à la topologie de maillage complet BGP requise pour la synchronisation d’itinéraires entre les routeurs. Avec la synchronisation de maillage complète, tous les routeurs BGP doivent se connecter à tous les autres routeurs de la topologie de routage. Toutefois, lorsque vous utilisez le réflecteur d’itinéraire, le réflecteur d’itinéraire est le seul routeur qui se connecte à tous les autres routeurs, appelés clients BGP, ce qui simplifie la synchronisation des itinéraires et réduit le trafic réseau. Le réflecteur d’itinéraire apprend tous les routages, calcule les meilleurs itinéraires et redistribue les meilleurs itinéraires à ses clients BGP.  
  
Avec Windows Server 2016, vous pouvez configurer les tunnels d’accès à distance d’un locataire individuel pour qu’ils se terminent sur plusieurs machines virtuelles de passerelle RAS. Cela offre une plus grande flexibilité pour les fournisseurs de services Cloud dans le cas où une machine virtuelle de passerelle RAS ne peut pas satisfaire à toutes les exigences de bande passante des connexions client.  
  
Toutefois, cette fonctionnalité introduit la complexité supplémentaire de la gestion des itinéraires et une synchronisation efficace des itinéraires entre les sites distants du client et leurs ressources virtuelles dans le centre de donnes Cloud. Fournir des clients avec des connexions à plusieurs passerelles RAS introduit également une complexité supplémentaire dans la configuration au niveau de l’entreprise, où chaque site locataire aura des voisins de routage distincts.  
  
Un réflecteur d’itinéraire BGP dans le plan de contrôle résout ces problèmes et rend le déploiement de l’infrastructure interne du CSP transparent pour les locataires de l’entreprise. Voici quelques points clés relatifs au réflecteur d’itinéraires BGP inclus avec la passerelle RAS et intégrés au contrôleur de réseau.  
  
-   Un réflecteur d’itinéraire dans un déploiement réseau à définition logicielle est une entité logique qui se trouve sur le plan de contrôle entre les passerelles RAS et le contrôleur de réseau. Toutefois, il ne participe pas au routage du plan de données.  
  
-   Lorsque vous ajoutez un nouveau locataire à votre centre de donné, le contrôleur de réseau configure automatiquement la première passerelle RAS du client en tant que réflecteur d’itinéraire.  
  
-   Chaque locataire a un réflecteur d’itinéraire correspondant et se trouve sur l’une des machines virtuelles de passerelle RAS associées à ce locataire.  
  
-   Un réflecteur d’itinéraire de locataire fait office de réflecteur de routage pour toutes les machines virtuelles de passerelle RAS associées au locataire. Les passerelles de locataire autres que le réflecteur de routage de passerelle RAS sont les clients de réflecteur d’itinéraire. Le réflecteur d’itinéraire effectue une synchronisation d’itinéraire entre tous les clients de réflecteur d’itinéraires afin que le routage du chemin de données réel puisse se produire.  
  
-   Un réflecteur d’itinéraire ne fournit pas de services de réflecteur d’itinéraire pour la passerelle RAS sur laquelle il est configuré.  
  
-   Un réflecteur de route met à jour le contrôleur de réseau avec les itinéraires d’entreprise qui correspondent aux sites d’entreprise du locataire. Cela permet au contrôleur de réseau de configurer les stratégies de virtualisation de réseau Hyper-V requises sur le réseau virtuel du locataire pour l’accès au chemin de données de bout en bout.  
  
-   Si les clients de votre entreprise utilisent le routage BGP dans l’espace d’adressage du client, le réflecteur de l’itinéraire de la passerelle RAS est le seul voisin BGP externe (eBGP) pour tous les sites du locataire correspondant. Cela est vrai, quels que soient les points de terminaison de tunnel du locataire d’entreprise. En d’autres termes, quelle que soit la machine virtuelle de passerelle RAS dans le centre de production CSP qui termine le tunnel VPN de site à site pour un site locataire, l’homologue eBGP pour tous les sites locataires est le réflecteur d’itinéraire.  
  
Pour plus d’informations, consultez Architecture de déploiement de la [passerelle RAS](RAS-Gateway-Deployment-Architecture.md) et la rubrique Request for Comments IETF (Internet Engineering Task Force) [RFC 4456 BGP route Reflection : Une alternative à la maille complète BGP (IBGP) ](https://tools.ietf.org/html/rfc4456).  
  

