---
title: Nouveautés de la passerelle
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur les nouvelles fonctionnalités pour la passerelle, qui est basé sur un logiciel, mutualisée, routeur capable de protocole BGP (Border Gateway) dans Windows Server 2016.
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
ms.openlocfilehash: 1a9ac762f6cd80d3889cf72478b7a7f8ce9e5cb7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-ras-gateway"></a>Nouveautés de la passerelle

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur les nouvelles fonctionnalités pour la passerelle, qui est basé sur un logiciel, mutualisée, routeur capable de protocole BGP (Border Gateway) dans Windows Server 2016. Le routeur BGP de RAS passerelle mutualisée est conçu pour les fournisseurs de services Cloud (CSP) et les entreprises qui hébergent plusieurs réseaux virtuels clients à l’aide de la virtualisation de réseau Hyper-V.  
  
> [!NOTE]  
> Dans Windows Server 2012 R2, cette passerelle est appelée passerelle RRAS; et dans System Center Virtual Machine Manager, cette passerelle est nommée passerelle Windows Server.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Options de connectivité du site à site](#bkmk_s2s)  
  
-   [Pools de passerelle](#bkmk_pools)  
  
-   [Extensibilité de Pool de passerelle](#bkmk_gps)  
  
-   [Redondance de Pool de passerelle M + N](#bkmk_m)  
  
-   [Réflecteur d’itinéraire](#bkmk_rr)  
  
## <a name="bkmk_s2s"></a>Options de connectivité du site à site  
Cette passerelle prend désormais en charge trois types de connexions de site à site VPN: Internet Key Exchange version 2 (IKEv2) tunnels de réseau privé virtuel (VPN) site à site VPN de couche 3 (L3) et Encapsulation GRE (Generic Routing).  
  
Pour plus d’informations sur GRE, voir [Tunneling GRE dans Windows Server 2016](../../../../remote/remote-access/ras-gateway/gre-tunneling-windows-server.md).  
  
## <a name="bkmk_pools"></a>Pools de passerelle  
Dans Windows Server 2016, vous pouvez créer des pools de passerelle de types différents. Passerelle pools contiennent le nombre d’instances de la passerelle et acheminent le trafic réseau entre les réseaux physiques et virtuels. Pools de passerelle peuvent effectuer l’une des fonctions de passerelle individuelle - Internet Key Exchange version 2 (IKEv2) tunnels de réseau privé virtuel (VPN) site à site VPN de couche 3 (L3) et l’Encapsulation générique de routage (GRE) - ou le pool peut effectuer toutes ces fonctions et agir en tant qu’un pool mixte.  
  
Vous pouvez créer des pools de passerelle à l’aide d’une logique que vous préférez selon les besoins de votre infrastructure. Par exemple, vous pouvez créer des pools de passerelle basées sur les caractéristiques suivantes.  
  
-   Types de tunnel VPN IKEv2, L3 VPN, GRE VPN)  
  
-   Capacité  
  
-   Niveau de redondance (la fiabilité de la fonction de votre plan de facturation pour les locataires)  
  
-   Séparation personnalisée pour les clients  
  
Pour plus d’informations, voir [RAS passerelle haute disponibilité](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_gps"></a>Extensibilité de Pool de passerelle  
Vous pouvez adapter facilement un pool de passerelle vers le haut ou vers le bas en ajoutant ou supprimant des ordinateurs virtuels de passerelle dans le pool. Retrait ou l’ajout de passerelles n’interrompt pas les services fournis par un pool. Vous pouvez également ajouter et supprimer les pools d’ensemble des passerelles.  
  
Pour plus d’informations, voir [RAS passerelle haute disponibilité](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_m"></a>Redondance de Pool de passerelle M + N  
Chaque pool de passerelle est redondant M + N. Cela signifie qu’un suis «nombre d’ordinateurs virtuels de passerelle active (VM) est sauvegardé par un nombre «n» d’ordinateurs virtuels de passerelle en attente. M + N redondance vous offre davantage de flexibilité pour déterminer le niveau de fiabilité dont vous avez besoin lorsque vous déployez la passerelle. Au lieu d’utiliser qu’une seule passerelle mise en veille par RAS passerelle ordinateur virtuel actif, qui est la seule option de configuration avec Windows Server 2012 R2 - vous pouvez maintenant configurer les ordinateurs virtuels en attente autant que nécessaire. La fonctionnalité Gestionnaire de Service de passerelle de contrôleur réseau utilise efficacement la capacité d’ordinateurs virtuels de passerelle RAS mise en veille pour fournir un basculement fiable si active les ordinateurs virtuels de passerelle RAS échoue ou perd la connectivité.  
  
Pour plus d’informations, voir [RAS passerelle haute disponibilité](RAS-Gateway-High-Availability.md).  
  
## <a name="bkmk_rr"></a>Réflecteur d’itinéraire  
Le réflecteur d’itinéraire de protocole BGP (Border Gateway) est désormais inclus avec la passerelle et offre une alternative à la topologie de maille pleine BGP qui est requise pour la synchronisation d’itinéraire entre des routeurs. Avec la synchronisation de maille pleine, tous les routeurs BGP doivent se connecter avec tous les autres routeurs dans la topologie de routage. Lorsque vous utilisez réflecteur d’itinéraire, toutefois, le réflecteur d’itinéraire est le seul routeur qui se connecte avec tous les autres routeurs, appelés BGP clients, ce qui simplifie la synchronisation de l’itinéraire et réduire le trafic réseau. Le réflecteur d’itinéraire apprend les itinéraires de tous les calcule des itinéraires meilleures et redistribué par les itinéraires meilleures à ses clients BGP.  
  
Avec Windows Server 2016, vous pouvez configurer accès à distance de tunnels d’un client au se terminent sur plusieurs ordinateurs virtuels de passerelle RAS. Cela fournit une plus grande souplesse pour les fournisseurs de services Cloud face à des circonstances où une machine virtuelle de la passerelle RAS ne peut pas répondre à toutes les exigences de la bande passante des connexions client.  
  
Cette fonctionnalité, toutefois, présente la complexité supplémentaire de gestion de l’itinéraire et une synchronisation efficace des itinéraires entre les sites distants client et leurs ressources virtuelles dans le centre de données cloud. Fournir des clients avec des connexions à plusieurs passerelles RAS introduit également complexité supplémentaire dans une configuration à la fin de l’entreprise, où chaque site client aura voisins routage distincts.  
  
Un réflecteur d’itinéraire BGP dans le plan de contrôle de résoudre ces problèmes et rend le déploiement de la structure interne CSP transparent aux clients d’entreprise. Voici quelques points importants sur le réflecteur d’itinéraire BGP qui est inclus avec la passerelle et intégré à un contrôleur de réseau.  
  
-   Un réflecteur d’itinéraire dans un déploiement de logiciel défini de mise en réseau est une entité logique qui se trouve sur le plan de contrôle entre les passerelles RAS et le contrôleur de réseau. Il ne participe pas, toutefois, les données plan routage.  
  
-   Lorsque vous ajoutez un nouveau locataire à votre centre de données, le contrôleur de réseau configure automatiquement le premier client passerelle comme un réflecteur d’itinéraire.  
  
-   Chaque client possède un réflecteur d’itinéraire correspondant, et il se trouve sur un des ordinateurs virtuels de la passerelle de RAS qui sont associés à ce client.  
  
-   Un client de réflecteur d’itinéraire agit comme le réflecteur d’itinéraire pour tous les ordinateurs virtuels passerelle RAS qui sont associés au client. Les passerelles de client autres que le réflecteur d’itinéraire de passerelle RAS sont les Clients de réflecteur d’itinéraire. Le réflecteur d’itinéraire effectue une synchronisation itinéraire entre tous les Clients de réflecteur d’itinéraire afin que le chemin d’accès de données réelles routage peut se produire.  
  
-   Un réflecteur d’itinéraire ne fournit pas de services de réflecteur d’itinéraire pour la passerelle d’accès distant sur lequel il est configuré.  
  
-   Réflecteur d’itinéraire A met à jour le contrôleur de réseau avec les itinéraires d’entreprise qui correspondent à des sites d’entreprise du client. Ainsi, le contrôleur de réseau configurer les stratégies de virtualisation de réseau Hyper-V requis sur le réseau virtuel locataire pour l’accès de chemin d’accès de bout en bout pour les données.  
  
-   Si vos clients d’entreprise utilisent le routage BGP dans l’espace d’adressage du client, le réflecteur d’itinéraire de passerelle RAS est uniquement externe voisin BGP (eBGP) pour tous les sites de client correspondant. Cela est vrai, quelle que soit les points de terminaison de tunnel du client de l’entreprise. En d’autres termes, les ordinateurs virtuels à la passerelle RAS dans le fournisseur CSP quel que soit le centre de données termine le tunnel VPN de site à site pour un site client, l’homologue eBGP pour tous les sites client est le réflecteur d’itinéraire.  
  
Pour plus d’informations, voir [Architecture de déploiement de passerelle RAS](RAS-Gateway-Deployment-Architecture.md) et la demande de Internet Engineering Task Force (IETF) de la rubrique commentaires [RFC 4456 BGP itinéraire réflexion: une Alternative à complète de maillage interne BGP (IBGP)](https://tools.ietf.org/html/rfc4456).  
  

