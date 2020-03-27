---
title: Contrôleur de réseau
description: Cette rubrique fournit une vue d’ensemble du contrôleur de réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ad13e41e756f0185a748fe9e17df64c71a8754bc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317072"
---
# <a name="network-controller"></a>Contrôleur de réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Nouveauté de Windows Server 2016, le contrôleur de réseau fournit un point d’automatisation centralisé et programmable pour gérer, configurer, surveiller et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de donnes. 

À l'aide du contrôleur de réseau, vous pouvez automatiser la configuration de l'infrastructure réseau au lieu d'effectuer une configuration manuelle des services et appareils réseau.

> [!NOTE]
> En plus de cette rubrique, la documentation du contrôleur de réseau suivante est disponible.
> - [Haute disponibilité du contrôleur de réseau](network-controller-high-availability.md)
> - [Configuration requise pour l’installation et la préparation du déploiement du contrôleur de réseau](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installer le rôle serveur Contrôleur de réseau en utilisant le Gestionnaire de serveur](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Étapes de la suite du déploiement pour le contrôleur de réseau](post-deploy-steps-nc.md)
> - [Applets de commande du contrôleur de réseau](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="network-controller-overview"></a><a name="bkmk_overview"></a>Vue d’ensemble du contrôleur de réseau

Le contrôleur de réseau est un rôle serveur hautement disponible et évolutif, et fournit une interface de programmation d’applications \(API\) qui permet au contrôleur de réseau de communiquer avec le réseau, et une deuxième API qui vous permet de communiquer avec le contrôleur de réseau.

Vous pouvez déployer le contrôleur de réseau dans les environnements domaine et non-domaine. Dans les environnements de domaine, le contrôleur de réseau authentifie les utilisateurs et les périphériques réseau à l’aide de Kerberos. dans les environnements n’appartenant pas à un domaine, vous devez déployer des certificats pour l’authentification.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur les hôtes physiques. Pour déployer le contrôleur de réseau, vous devez installer le rôle de serveur contrôleur de réseau sur un ordinateur virtuel Hyper-V \(\) d’ordinateur virtuel installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les machines virtuelles sur trois hôtes Hyper\-V différents, vous devez activer les hôtes Hyper\-V pour la mise en réseau définie par logiciel \(SDN\) en ajoutant les ordinateurs hôtes au contrôleur de réseau à l’aide de la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous activez le Load Balancer logiciel SDN pour fonctionner. Pour plus d’informations, consultez [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Le contrôleur de réseau communique avec les composants, services et appareils réseau à l'aide de l'API Southbound. Avec l'API Southbound, le contrôleur de réseau peut détecter les appareils réseau, détecter les configurations des services et rassembler toutes les informations nécessaires sur le réseau. En outre, l'API Southbound indique au contrôleur de réseau une voie pour envoyer des informations à l'infrastructure réseau, par exemple les modifications de configuration que vous avez apportées.

L'API Northbound du contrôleur de réseau vous offre la possibilité de recueillir des informations sur le réseau à partir du contrôleur de réseau et de les utiliser pour analyser et configurer le réseau.

L’API Northbound du contrôleur de réseau vous permet de configurer, de surveiller, de dépanner et de déployer de nouveaux appareils sur le réseau à l’aide de Windows PowerShell, de l’API REST\) de transfert d’État \(ou d’une application de gestion avec une interface utilisateur graphique, telle que System Center Virtual Machine Manager.

>[!NOTE]
>L'API Northbound du contrôleur de réseau est implémentée comme une interface REST.

Vous pouvez gérer votre réseau de centre de donnes avec le contrôleur de réseau à l’aide d’applications de gestion, telles que System Center Virtual Machine Manager \(SCVMM\)et System Center Operations Manager \(SCOM\), car le contrôleur de réseau vous permet de configurer, surveiller, programmer et dépanner l’infrastructure réseau qui est sous son contrôle.

À l'aide de Windows PowerShell, de l'API REST ou d'une application de gestion, vous pouvez utiliser le contrôleur de réseau pour gérer l'infrastructure réseau physique et virtuelle suivante :

- Ordinateurs virtuels Hyper-V et commutateurs virtuels

- Pare-feu de centre de données

- Service d’accès à distance \(les passerelles mutualisées\) les passerelles mutualisées, les passerelles virtuelles et les pools de passerelle

- Programmes d’équilibrage de la charge logicielle

Dans l'illustration suivante, un administrateur utilise un outil de gestion qui interagit directement avec le contrôleur de réseau. Le contrôleur de réseau fournit des informations sur l’infrastructure réseau, y compris l’infrastructure virtuelle et physique, à l’outil de gestion et modifie la configuration en fonction des actions de l’administrateur lors de l’utilisation de l’outil.  

![Vue d’ensemble du contrôleur de réseau](../../../media/Network-Controller/NetController_overview.png)  

Si vous déployez un contrôleur de réseau dans un environnement de laboratoire de test, vous pouvez exécuter le rôle de serveur contrôleur de réseau sur un ordinateur virtuel Hyper-V \(\) d’ordinateur virtuel installé sur un ordinateur hôte Hyper-V.

Pour une haute disponibilité dans des centres de plus grande taille, vous pouvez déployer un cluster à l’aide de trois machines virtuelles qui sont installées sur trois hôtes Hyper-V ou plus. Pour plus d’informations, consultez [haute disponibilité du contrôleur de réseau](network-controller-high-availability.md).

## <a name="network-controller-features"></a><a name="bkmk_features"></a>Fonctionnalités du contrôleur de réseau

Les fonctionnalités du contrôleur de réseau suivantes vous permettent de configurer et de gérer des services et appareils réseau virtuels et physiques.  
  
-   [Gestion de pare-feu](#bkmk_firewall)  
  
-   [Gestion des Load Balancer logicielles](#bkmk_slb)  
  
-   [Gestion de réseau virtuel](#bkmk_virtual)  
  
-   [Gestion des passerelles RAS](#bkmk_gateway)

>[!IMPORTANT]
>La sauvegarde et la restauration du contrôleur de réseau ne sont actuellement pas disponibles dans Windows Server 2016.
  
### <a name="firewall-management"></a><a name="bkmk_firewall"></a>Gestion de pare-feu

Cette fonctionnalité du contrôleur de réseau vous permet de configurer et gérer les règles de contrôle d'accès de pare-feu (autorisation ou refus) pour les ordinateurs virtuels de votre charge de travail pour le trafic réseau Est/Ouest et Nord/Sud dans votre centre de données. Les règles de pare-feu sont installées dans le port du commutateur virtuel des ordinateurs virtuels de la charge de travail et sont par conséquent distribuées sur votre charge de travail dans le centre de données. À l'aide de l'API Northbound, vous pouvez définir les règles de pare-feu pour le trafic entrant et sortant à partir de l'ordinateur virtuel de la charge de travail. Vous pouvez également configurer chaque règle de pare-feu pour consigner le trafic qui a été autorisé ou refusé par la règle.  

Pour plus d’informations, consultez [vue d’ensemble du pare-feu de centre](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)de données.

### <a name="software-load-balancer-management"></a><a name="bkmk_slb"></a>Gestion des Load Balancer logicielles

Cette fonctionnalité du contrôleur de réseau vous permet d'activer plusieurs serveurs pour héberger la même charge de travail, ce qui garantit évolutivité et haute disponibilité.  
  
Pour plus d’informations, consultez [équilibrage &#40;de charge&#41; logiciel SLB pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="virtual-network-management"></a><a name="bkmk_virtual"></a>Gestion de réseau virtuel

Cette fonctionnalité du contrôleur de réseau vous permet de déployer et configurer la virtualisation du réseau Hyper-V, y compris le commutateur virtuel Hyper-V et les cartes réseau virtuelles sur des ordinateurs virtuels individuels ainsi que de stocker et distribuer des stratégies de réseau virtuel.

Le contrôleur de réseau prend en charge NVGRE (Network Virtualization Generic Routing Encapsulation) et VXLAN (Virtual Extensible Local Area Network).

### <a name="ras-gateway-management"></a><a name="bkmk_gateway"></a>Gestion des passerelles RAS

Cette fonctionnalité de contrôleur de réseau vous permet de déployer, de configurer et de gérer des machines virtuelles qui sont membres d’un pool de passerelle RAS, en fournissant des services de passerelle à vos locataires. Le contrôleur de réseau vous permet de déployer automatiquement des machines virtuelles exécutant la passerelle RAS avec les fonctionnalités de passerelle suivantes :

> [!NOTE]
> Dans System Center Virtual Machine Manager, la passerelle RAS est nommée passerelle Windows Server.

- Ajout et suppression des ordinateurs virtuels de passerelle du cluster, et indication du niveau de sauvegarde requis.

- Connectivité de passerelle de réseau privé virtuel (VPN) site à site entre les réseaux des clients distants et votre centre de données à l'aide d'IPsec.

- Connectivité de passerelle VPN site à site entre les réseaux des clients distants et votre centre de données à l'aide de l'encapsulation générique de routage (GRE).

- Fonctionnalité de transfert de couche 3.

- Le routage Border Gateway Protocol (BGP), qui vous permet de gérer le routage du trafic réseau entre les réseaux d’ordinateurs virtuels de vos clients et leurs sites distants.

Le contrôleur de réseau peut placer différentes connexions d’un locataire sur des passerelles distinctes. Vous pouvez utiliser une adresse IP publique unique pour toutes les connexions de passerelle ou avoir des adresses IP publiques différentes pour un sous-ensemble des connexions. Le contrôleur de réseau consigne toutes les modifications de configuration et d’état de la passerelle, qui peuvent être utilisées à des fins d’audit et de dépannage.

Pour plus d’informations sur BGP, [consultez &#40;Border Gateway Protocol&#41;BGP](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Pour plus d’informations sur la passerelle RAS, consultez [passerelle RAS pour SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Options de déploiement du contrôleur de réseau

Pour déployer un contrôleur de réseau à l’aide de System Center Virtual Machine Manager \(VMM\), consultez [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Pour déployer un contrôleur de réseau à l’aide de scripts, consultez [déployer une infrastructure réseau définie par logiciel à l’aide de scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Pour déployer un contrôleur de réseau à l’aide de Windows PowerShell, consultez [déployer un contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
