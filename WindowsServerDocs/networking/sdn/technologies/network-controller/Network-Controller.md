---
title: Contrôleur de réseau
description: Cette rubrique fournit une vue d’ensemble du contrôleur de réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ace628c6ae9802c0c65d360aedfac8c80ac5537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875680"
---
# <a name="network-controller"></a>Contrôleur de réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Nouveau dans Windows Server 2016, contrôleur de réseau offre un centralisé programmable d’automation pour gérer, configurer, surveiller et dépanner l’infrastructure de réseau virtuel et physique dans votre centre de données. 

À l'aide du contrôleur de réseau, vous pouvez automatiser la configuration de l'infrastructure réseau au lieu d'effectuer une configuration manuelle des services et appareils réseau.

> [!NOTE]
> Outre cette rubrique, la documentation suivante sur le contrôleur de réseau est disponible.
> - [Haute disponibilité du contrôleur de réseau](network-controller-high-availability.md)
> - [Installation en matière de préparation pour le déploiement de contrôleur de réseau](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Étapes de post-déploiement pour contrôleur de réseau](post-deploy-steps-nc.md)
> - [Applets de commande de contrôleur réseau](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Vue d’ensemble du contrôleur de réseau

Contrôleur de réseau est un rôle de serveur hautement disponible et évolutif et fournit une interface de programmation d’application \(API\) qui permet de contrôleur de réseau pour communiquer avec le réseau et une deuxième API qui vous permet de communiquer avec le contrôleur de réseau.

Vous pouvez déployer le contrôleur de réseau dans le domaine et les environnements extérieurs au domaine. Dans les environnements de domaine, contrôleur de réseau s’authentifie les utilisateurs et les périphériques réseau à l’aide de Kerberos ; dans les environnements extérieurs au domaine, vous devez déployer des certificats pour l’authentification.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(machine virtuelle\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur des machines virtuelles sur trois Hyper différents\-hôtes V, vous devez activer l’Hyper\-hôtes V pour Sdn \(SDN\) en ajoutant les ordinateurs hôtes à l’aide de contrôleur de réseau la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous permettez à l’équilibreur de charge logiciel SDN de la fonction. Pour plus d’informations, consultez [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Le contrôleur de réseau communique avec les composants, services et appareils réseau à l'aide de l'API Southbound. Avec l'API Southbound, le contrôleur de réseau peut détecter les appareils réseau, détecter les configurations des services et rassembler toutes les informations nécessaires sur le réseau. En outre, l'API Southbound indique au contrôleur de réseau une voie pour envoyer des informations à l'infrastructure réseau, par exemple les modifications de configuration que vous avez apportées.

L'API Northbound du contrôleur de réseau vous offre la possibilité de recueillir des informations sur le réseau à partir du contrôleur de réseau et de les utiliser pour analyser et configurer le réseau.

L’API Northbound du contrôleur de réseau vous permet de configurer, surveiller, dépanner et déployer de nouveaux appareils sur le réseau à l’aide de Windows PowerShell, le Representational State Transfer \(REST\) API ou une application de gestion avec une interface utilisateur graphique, tels que System Center Virtual Machine Manager.

>[!NOTE]
>L'API Northbound du contrôleur de réseau est implémentée comme une interface REST.

Vous pouvez gérer votre réseau de centre de données avec le contrôleur de réseau à l’aide d’applications de gestion, tels que System Center Virtual Machine Manager \(SCVMM\)et System Center Operations Manager \(SCOM\), Étant donné que le contrôleur de réseau vous permet de configurer, surveiller, programme et résoudre les problèmes de l’infrastructure réseau qui est sous son contrôle.

À l'aide de Windows PowerShell, de l'API REST ou d'une application de gestion, vous pouvez utiliser le contrôleur de réseau pour gérer l'infrastructure réseau physique et virtuelle suivante :

- Ordinateurs virtuels Hyper-V et commutateurs virtuels

- Pare-feu de centre de données

- Remote Access Service \(RAS\) les passerelles mutualisées, les passerelles virtuel et les pools de passerelles

- Équilibreurs de charge logiciel

Dans l'illustration suivante, un administrateur utilise un outil de gestion qui interagit directement avec le contrôleur de réseau. Contrôleur de réseau fournit des informations sur l’infrastructure réseau, y compris les infrastructures physiques et virtuels, à l’outil de gestion et apporte des modifications de configuration en fonction des actions de l’administrateur lors de l’utilisation de l’outil.  

![Vue d’ensemble du contrôleur de réseau](../../../media/Network-Controller/NetController_overview.png)  

Si vous déployez un contrôleur de réseau dans un environnement de laboratoire de test, vous pouvez exécuter le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(machine virtuelle\) qui est installé sur un ordinateur hôte Hyper-V.

Pour la haute disponibilité dans des centres de données plus volumineux, vous pouvez déployer un cluster à l’aide de trois ordinateurs virtuels qui sont installés sur trois ou plusieurs hôtes Hyper-V. Pour plus d’informations, consultez [haute disponibilité du contrôleur de réseau](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Fonctionnalités du contrôleur de réseau

Les fonctionnalités du contrôleur de réseau suivantes vous permettent de configurer et de gérer des services et appareils réseau virtuels et physiques.  
  
-   [Gestion du pare-feu](#bkmk_firewall)  
  
-   [Gestion de l’équilibrage de charge logiciel](#bkmk_slb)  
  
-   [Gestion de réseau virtuel](#bkmk_virtual)  
  
-   [Gestion de la passerelle RAS](#bkmk_gateway)

>[!IMPORTANT]
>Sauvegarde du contrôleur de réseau et de restauration n’est pas actuellement disponible dans Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gestion du pare-feu

Cette fonctionnalité du contrôleur de réseau vous permet de configurer et gérer les règles de contrôle d'accès de pare-feu (autorisation ou refus) pour les ordinateurs virtuels de votre charge de travail pour le trafic réseau Est/Ouest et Nord/Sud dans votre centre de données. Les règles de pare-feu sont installées dans le port du commutateur virtuel des ordinateurs virtuels de la charge de travail et sont par conséquent distribuées sur votre charge de travail dans le centre de données. À l'aide de l'API Northbound, vous pouvez définir les règles de pare-feu pour le trafic entrant et sortant à partir de l'ordinateur virtuel de la charge de travail. Vous pouvez également configurer chaque règle de pare-feu pour consigner le trafic qui a été autorisé ou refusé par la règle.  

Pour plus d’informations, consultez [vue d’ensemble du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gestion de l’équilibrage de charge logiciel

Cette fonctionnalité du contrôleur de réseau vous permet d'activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.  
  
Pour plus d’informations, consultez [l’équilibrage de charge logiciel &#40;SLB&#41; pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gestion de réseau virtuel

Cette fonctionnalité du contrôleur de réseau vous permet de déployer et configurer la virtualisation du réseau Hyper-V, y compris le commutateur virtuel Hyper-V et les cartes réseau virtuelles sur des ordinateurs virtuels individuels ainsi que de stocker et distribuer des stratégies de réseau virtuel.

Le contrôleur de réseau prend en charge NVGRE (Network Virtualization Generic Routing Encapsulation) et VXLAN (Virtual Extensible Local Area Network).

### <a name="bkmk_gateway"></a>Gestion de la passerelle RAS

Cette fonctionnalité du contrôleur de réseau vous permet de déployer, configurer et gérer des machines virtuelles (VM) qui sont membres d’un pool de passerelle RAS, fournissant des services de passerelle à vos clients. Contrôleur de réseau vous permet de déployer automatiquement des machines virtuelles exécutant la passerelle RAS avec les fonctionnalités de passerelle suivantes :

> [!NOTE]
> Dans System Center Virtual Machine Manager, passerelle RAS est nommée passerelle Windows Server.

- Ajout et suppression des ordinateurs virtuels de passerelle du cluster, et indication du niveau de sauvegarde requis.

- Connectivité de passerelle de réseau privé virtuel (VPN) site à site entre les réseaux des clients distants et votre centre de données à l'aide d'IPsec.

- Connectivité de passerelle VPN site à site entre les réseaux des clients distants et votre centre de données à l'aide de l'encapsulation générique de routage (GRE).

- Fonctionnalité de transfert de couche 3.

- Protocole BGP (Border Gateway) routage, ce qui vous permet de gérer le routage du trafic réseau entre les réseaux de machines virtuelles de vos clients et leurs sites distants.

Contrôleur de réseau permettre placer des différentes connexions d’un client sur des passerelles distinctes. Vous pouvez utiliser une adresse IP publique unique pour toutes les connexions de passerelle ou ont différents publics des adresses IP pour un sous-ensemble des connexions. Contrôleur de réseau enregistre toutes les modifications d’état, qui peuvent être utilisées pour l’audit et à des fins de dépannage et les configuration de la passerelle.

Pour plus d’informations sur le protocole BGP, consultez [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Pour plus d’informations sur la passerelle RAS, consultez [passerelle RAS pour SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Options de déploiement de contrôleur de réseau

Pour déployer un contrôleur de réseau à l’aide de System Center Virtual Machine Manager \(VMM\), consultez [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Pour déployer un contrôleur de réseau à l’aide de scripts, consultez [déployer un logiciel défini Infrastructure à l’aide de Scripts réseau](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Pour déployer un contrôleur de réseau à l’aide de Windows PowerShell, consultez [déployer de contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
