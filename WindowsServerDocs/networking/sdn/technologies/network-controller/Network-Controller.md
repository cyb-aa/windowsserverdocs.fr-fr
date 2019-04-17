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
ms.openlocfilehash: acb9abbd716e9930fb01431e7004abb72a7da10c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller"></a>Contrôleur de réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Nouveau dans Windows Server 2016, le contrôleur de réseau fournit un point programmable et centralisé de l’automatisation pour gérer, configurer, analyser et dépanner l’infrastructure réseau virtuelle et physique dans votre centre de données. 

À l’aide du contrôleur de réseau, vous pouvez automatiser la configuration de l’infrastructure réseau au lieu d’effectuer une configuration manuelle des périphériques réseau et services.

> [!NOTE]
> Outre cette rubrique, la documentation suivante sur le contrôleur de réseau est disponible.
> - [Haute disponibilité du contrôleur de réseau](network-controller-high-availability.md)
> - [Installation et configuration requise de préparation pour le déploiement de contrôleur de réseau](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Étapes de post-déploiement pour contrôleur de réseau](post-deploy-steps-nc.md)
> - [Applets de commande de contrôleur réseau](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Vue d’ensemble du contrôleur de réseau

Contrôleur de réseau est un rôle serveur hautement disponible et évolutif et fournit une programmation d’applications de l’interface \(API\) qui permet à un contrôleur de réseau communiquer avec le réseau et une deuxième API vous permettant de communiquer avec le contrôleur de réseau.

Vous pouvez déployer le contrôleur de réseau dans les environnements de domaine et un domaine. Dans les environnements de domaine, contrôleur de réseau authentifie les utilisateurs et périphériques réseau à l’aide de Kerberos; dans les environnements de domaine, vous devez déployer des certificats pour l’authentification.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(VM\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les ordinateurs virtuels sur trois différents hôtes Hyper\-V, vous devez activer les ordinateurs hôtes Hyper\-V pour \(SDN\) logiciel défini de mise en réseau en ajoutant les ordinateurs hôtes à un contrôleur de réseau à l’aide de la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous activez l’équilibrage de charge logicielle SDN de la fonction. Pour plus d’informations, voir [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Contrôleur de réseau communique avec les périphériques réseau, des services et composants à l’aide de l’API Southbound. Avec l’API Southbound, le contrôleur de réseau peut découvrir les périphériques réseau, détecter les configurations de service et rassembler toutes les informations que nécessaires sur le réseau. En outre, l’API Southbound donne le contrôleur de réseau une voie pour envoyer des informations à l’infrastructure réseau, telles que les modifications de configuration que vous avez apportées.

L’API northbound du contrôleur de réseau vous offre la possibilité de collecter des informations de réseau à partir du contrôleur de réseau et l’utiliser pour surveiller et de configurer le réseau.

L’API northbound du contrôleur de réseau vous permet de configurer, analyser, résoudre les problèmes et déployer de nouveaux appareils sur le réseau à l’aide de Windows PowerShell, l’API \(REST\) Representational State Transfer ou une application de gestion avec une interface graphique utilisateur, tels que System Center Virtual Machine Manager.

>[!NOTE]
>L’API northbound du contrôleur de réseau est implémentée comme une interface REST.

Vous pouvez gérer votre réseau de centre de données avec le contrôleur de réseau à l’aide de gestion des applications, telles que \(SCVMM\) System Center Virtual Machine Manager et System Center Operations Manager \(SCOM\), car le contrôleur de réseau vous permet de configurer, analyser, de programmer et dépanner l’infrastructure réseau qui se trouve sous son contrôle.

À l’aide de Windows PowerShell, l’API REST ou une application de gestion, vous pouvez utiliser le contrôleur de réseau pour gérer l’infrastructure réseau physique et virtuelle suivante:

- Ordinateurs virtuels Hyper-V et commutateurs virtuels

- Pare-feu de centre de données

- Les passerelles mutualisée \(RAS\) Service d’accès à distance, les passerelles virtuels et pools de passerelle

- Équilibreurs de charge logicielle

Dans l’illustration suivante, un administrateur utilise un outil de gestion qui interagit directement avec le contrôleur de réseau. Contrôleur de réseau fournit des informations sur l’infrastructure réseau, notamment l’infrastructure virtuelle et physique, à l’outil de gestion et apporte des modifications de configuration en fonction des actions de l’administrateur lors de l’utilisation de l’outil.  

![Vue d’ensemble du contrôleur de réseau](../../../media/Network-Controller/NetController_overview.png)  

Si vous déployez un contrôleur de réseau dans un environnement de laboratoire de test, vous pouvez exécuter le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(VM\) qui est installé sur un ordinateur hôte Hyper-V.

Pour une haute disponibilité dans les centres de données plus grandes, vous pouvez déployer un cluster à l’aide de trois ordinateurs virtuels qui sont installés sur trois ou plusieurs hôtes Hyper-V. Pour plus d’informations, voir [haute disponibilité du contrôleur de réseau](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Fonctionnalités du contrôleur réseau

Les fonctionnalités de contrôleur de réseau suivantes permettent de configurer et gérer virtuel et les périphériques réseau physiques et les services.  
  
-   [Gestion du pare-feu](#bkmk_firewall)  
  
-   [Gestion de l’équilibrage de charge logicielle](#bkmk_slb)  
  
-   [Gestion de réseau virtuel](#bkmk_virtual)  
  
-   [Gestion de la passerelle RAS](#bkmk_gateway)

>[!IMPORTANT]
>Sauvegarde du contrôleur de réseau et restauration n’est pas actuellement disponible dans Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gestion du pare-feu

Cette fonctionnalité du contrôleur de réseau vous permet de configurer et gérer l’autorisation ou refus des règles de contrôle d’accès de pare-feu pour votre charge de travail ordinateurs virtuels est/ouest et un trafic réseau Nord/Sud dans votre centre de données. Les règles de pare-feu sont installées dans le port de commutateur virtuel de la charge de travail ordinateurs virtuels, et par conséquent, ils sont distribués sur votre charge de travail dans le centre de données. À l’aide de l’API Northbound, vous pouvez définir les règles de pare-feu pour le trafic entrant et sortant à partir de la charge de travail ordinateurs virtuels. Vous pouvez également configurer chaque règle de pare-feu pour consigner le trafic qui a été autorisé ou refusé par la règle.  

Pour plus d’informations, voir [vue d’ensemble du pare-feu de centre de données](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gestion de l’équilibrage de charge logicielle

Cette fonctionnalité du contrôleur de réseau vous permet activer plusieurs serveurs pour héberger la même charge de travail, en fournissant une évolutivité et haute disponibilité.  
  
Pour plus d’informations, voir [l’équilibrage de charge logicielle & #40; SLB & #41; pour SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gestion de réseau virtuel

Cette fonctionnalité du contrôleur de réseau vous permet de déployer et configurer la virtualisation de réseau Hyper-V, y compris le commutateur virtuel Hyper-V et les cartes réseau virtuelles sur des ordinateurs virtuels individuels et stocker et distribuer des stratégies de réseau virtuel.

Contrôleur de réseau prend en charge réseau virtualisation générique Encapsulation routage (NVGRE) et virtuel Extensible réseau Local VXLAN ().

### <a name="bkmk_gateway"></a>Gestion de la passerelle RAS

Cette fonctionnalité du contrôleur de réseau vous permet de déployer, configurer et gérer des machines virtuelles (VM) qui sont membres d’un pool de passerelle, en fournissant des services de passerelle à vos clients. Contrôleur de réseau vous permet de déployer automatiquement des ordinateurs virtuels en cours d’exécution cette passerelle avec les fonctionnalités suivantes de la passerelle:

> [!NOTE]
> Dans System Center Virtual Machine Manager, cette passerelle est nommée passerelle Windows Server.

- Ajouter et supprimer des ordinateurs virtuels passerelle du cluster, spécifiez le niveau de sauvegarde requis.

- Connectivité de passerelle de réseau privé virtuel (VPN) site à site entre les réseaux des clients distants et votre centre de données à l’aide d’IPsec.

- Connectivité de passerelle de site VPN entre les réseaux des clients distants et votre centre de données à l’aide de l’Encapsulation GRE (Generic Routing).

- Couche 3 fonctionnalité de transfert.

- Protocole BGP (Border Gateway) routage, qui vous permet de gérer le routage du trafic réseau entre les réseaux d’ordinateurs virtuels de vos clients et leurs sites distants.

Contrôleur de réseau peut placer des connexions différentes d’un client sur des passerelles distinctes. Vous pouvez utiliser une adresse IP publique unique pour toutes les connexions de passerelle ou ont différents publics des adresses IP pour un sous-ensemble des connexions. Contrôleur de réseau consigne toutes les configuration de la passerelle et les changements d’état, qui peuvent être utilisés pour l’audit et à des fins de dépannage.

Pour plus d’informations sur BGP, voir [Border Gateway Protocol & #40; Protocole BGP & #41; ](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Pour plus d’informations sur la passerelle, voir [passerelle pour SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Options de déploiement de contrôleur de réseau

Pour déployer le contrôleur de réseau à l’aide de System Center Virtual Machine Manager \(VMM\), voir [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Pour déployer un contrôleur de réseau à l’aide de scripts, voir [déployer un logiciel défini Infrastructure à l’aide de Scripts réseau](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Pour déployer un contrôleur de réseau à l’aide de Windows PowerShell, voir [déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
