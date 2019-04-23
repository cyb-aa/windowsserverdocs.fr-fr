---
title: Nouveautés de la passerelle du serveur d’accès à distance
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les nouvelles fonctionnalités pour la passerelle RAS, qui est basé sur un logiciel, mutualisée, routeur capable de protocole BGP (Border Gateway) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 709cb192-313a-47b5-954e-eb5f6fee51a7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5cc7d8bab3f2783750dbd723da745b1df3c2e462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863020"
---
# <a name="whats-new-in-ras-gateway"></a>Nouveautés de la passerelle du serveur d’accès à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les nouvelles fonctionnalités pour la passerelle RAS, qui est basé sur un logiciel, mutualisée, routeur capable de protocole BGP (Border Gateway) dans Windows Server 2016. Le routeur BGP mutualisée de passerelle RAS est conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels client à l’aide de la virtualisation de réseau Hyper-V.  
  
> [!NOTE]  
> Dans Windows Server 2012 R2, passerelle RAS est nommée passerelle RRAS ; et dans System Center Virtual Machine Manager, passerelle RAS est nommée passerelle Windows Server.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Options de connectivité de site à site](#bkmk_s2s)  
  
-   [Pools de passerelles](#bkmk_pools)  
  
-   [Évolutivité de Pool de passerelle](#bkmk_gps)  
  
-   [Redondance de Pool de passerelle M + N](#bkmk_m)  
  
-   [Réflecteur d’itinéraire](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Options de connectivité de site à site  
Passerelle RAS prend désormais en charge trois types de connexions VPN de site à site :  Internet Key Exchange version 2 (IKEv2) tunnels de réseau privé virtuel (VPN) site à site VPN de couche 3 (L3) et l’Encapsulation GRE (Generic Routing).  
  
Pour plus d’informations sur GRE, consultez [Tunneling GRE dans Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Pools de passerelles  
Dans Windows Server 2016, vous pouvez créer des pools de passerelles de types différents. Pools de passerelles contiennent de nombreuses instances de passerelle RAS et acheminent le trafic réseau entre les réseaux physiques et virtuels. Pools de passerelles peuvent effectuer toutes les fonctions de passerelle individuelles - Internet Key Exchange version 2 (IKEv2) tunnels de réseau privé virtuel (VPN) site à site VPN de couche 3 (L3) et l’Encapsulation générique de routage (GRE) - ou le pool peut effectuer toutes ces fonctions et agir en tant qu’un pool mixte.  
  
Vous pouvez créer des pools de passerelle à l’aide d’une logique que vous préférez selon les besoins de votre infrastructure. Par exemple, vous pouvez créer des pools de passerelle en fonction d’une des caractéristiques suivantes.  
  
-   Types de tunnel VPN IKEv2, VPN L3, GRE VPN)  
  
-   Capacité  
  
-   Niveau de redondance (selon votre plan de facturation pour les clients de la fiabilité)  
  
-   Séparation personnalisée pour les clients  
  
Pour plus d’informations, consultez [passerelle RAS haute disponibilité](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Évolutivité de Pool de passerelle  
Vous pouvez facilement augmenter un pool de passerelle vers le haut ou vers le bas en ajoutant ou supprimant des machines virtuelles de passerelle dans le pool. Suppression ou ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer des pools d’ensemble des passerelles.  
  
Pour plus d’informations, consultez [passerelle RAS haute disponibilité](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Redondance de Pool de passerelle M + N  
Chaque pool de passerelle est redondant M + N. Cela signifie qu’un suis ' nombre de passerelle active les machines virtuelles (VM) est sauvegardé par un nombre « n » des machines virtuelles de passerelle de secours. M + N redondance vous offre davantage de flexibilité dans la détermination du niveau de fiabilité dont vous avez besoin lorsque vous déployez la passerelle RAS. Au lieu d’utiliser une seule passerelle RAS secours par RAS passerelle machine virtuelle active, qui est la seule option de configuration avec Windows Server 2012 R2 - vous pouvez désormais configurer des machines virtuelles secours autant que nécessaire. La fonctionnalité de gestionnaire de Service de passerelle de contrôleur réseau utilise efficacement la capacité de machine virtuelle de passerelle RAS de secours pour assurer le basculement fiable si un actif échoue ou perd la connectivité de machine virtuelle de passerelle RAS.  
  
Pour plus d’informations, consultez [passerelle RAS haute disponibilité](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Réflecteur d’itinéraire  
Le réflecteur d’itinéraire de protocole BGP (Border Gateway) est désormais inclus avec la passerelle RAS et fournit une alternative à la topologie de maille pleine BGP est requis pour la synchronisation de routage entre des routeurs. Avec la synchronisation de maillage complet, tous les routeurs BGP doivent se connecter tous les autres routeurs dans la topologie de routage. Lorsque vous utilisez réflecteur d’itinéraire, toutefois, le réflecteur d’itinéraire est le seul routeur qui se connecte avec tous les autres routeurs, appelés BGP clients, simplifiant ainsi la synchronisation de l’itinéraire et réduire le trafic réseau. Le réflecteur d’itinéraire apprend les itinéraires de tous les calcule les itinéraires meilleures et redistribue les itinéraires meilleures à ses clients BGP.  
  
Avec Windows Server 2016, vous pouvez configurer les tunnels d’accès à distance d’un client individuel à la fermeture de plusieurs machines virtuelles de passerelle RAS. Cela offre une flexibilité accrue pour les fournisseurs de Service Cloud face à des circonstances où une seule machine virtuelle de la passerelle RAS ne peut pas répondre à toutes les exigences de bande passante des connexions client.  
  
Cette fonctionnalité, présente toutefois, la complexité supplémentaire de gestion d’itinéraires et une synchronisation efficace des itinéraires entre les sites distants de locataire et leurs ressources virtuelles dans le centre de données cloud. En fournissant des locataires avec des connexions à plusieurs passerelles RAS introduit également une complexité supplémentaire dans la configuration à la fin de l’entreprise, où chaque site client aura des voisins de routage distincts.  
  
Un réflecteur d’itinéraire BGP dans le plan de contrôle résout ces problèmes et rend le déploiement d’infrastructure interne CSP transparent pour les clients d’entreprise. Voici quelques points clés concernant le réflecteur d’itinéraire BGP qui est inclus avec la passerelle RAS et intégré avec un contrôleur de réseau.  
  
-   Réflecteur d’itinéraire A dans un déploiement de Sdn est une entité logique qui se trouve sur le plan de contrôle entre les passerelles RAS et le contrôleur de réseau. Il ne participe pas, toutefois, routage de plan de données.  
  
-   Lorsque vous ajoutez un nouveau locataire pour votre centre de données, contrôleur de réseau configure automatiquement le premier locataire passerelle RAS comme un réflecteur d’itinéraire.  
  
-   Chaque client possède un réflecteur d’itinéraire correspondant, et il se trouve sur l’une des machines virtuelles de la passerelle de RAS qui sont associés à ce client.  
  
-   Un client de réflecteur d’itinéraire agit en tant que le réflecteur d’itinéraire pour tous les ordinateurs virtuels passerelle RAS qui sont associés au locataire. Passerelles de client autres que le réflecteur d’itinéraire de passerelle RAS sont les Clients de réflecteur d’itinéraire. Le réflecteur d’itinéraire assure la synchronisation de routage entre tous les Clients de réflecteur d’itinéraire afin que le routage de chemin d’accès de données réelles peut se produire.  
  
-   Réflecteur d’itinéraire A ne fournit pas de services de réflecteur d’itinéraire pour la passerelle RAS sur lequel il est configuré.  
  
-   Réflecteur d’itinéraire A met à jour le contrôleur de réseau avec les itinéraires d’entreprise qui correspondent à des sites d’entreprise du client. Ainsi, le contrôleur de réseau configurer les stratégies de virtualisation de réseau Hyper-V requis sur le réseau virtuel client pour l’accès du chemin d’accès des données de bout en bout.  
  
-   Si vos clients d’entreprise utilisent le routage BGP dans l’espace d’adressage du client, le réflecteur d’itinéraire de passerelle RAS est voisin BGP (eBGP) externe uniquement pour tous les sites du client correspondant. Cela est vrai quel que soit les points de terminaison de tunnel du client d’entreprise. En d’autres termes, peu importe quelle machine virtuelle à la passerelle RAS dans le CSP, centre de données termine le tunnel VPN de site à site pour un site locataire, eBGP homologue pour tous les sites de locataire est le réflecteur d’itinéraire.  
  
Pour plus d’informations, consultez [Architecture de déploiement de passerelle RAS](RAS-Gateway-Deployment-Architecture.md) et la demande Internet Engineering Task Force (IETF) pour la rubrique de commentaires [RFC 4456 BGP itinéraire réflexion : Une Alternative à intégral Mesh BGP interne (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

