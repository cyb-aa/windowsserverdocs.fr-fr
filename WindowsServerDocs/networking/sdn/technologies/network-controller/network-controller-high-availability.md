---
title: Contrôleur de réseau haute disponibilité
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la haute disponibilité du contrôleur de réseau pour la mise en réseau SDN (Software Defined) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd3ae9f4c1f1fc3035fae9ace880046312df2f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813350"
---
# <a name="network-controller-high-availability"></a>Contrôleur de réseau haute disponibilité

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur le contrôleur de réseau haute disponibilité et évolutivité configuration pour Sdn \(SDN\).

Lorsque vous déployez SDN dans votre centre de données, vous pouvez utiliser le contrôleur de réseau pour déployer, surveiller et gérer de nombreux éléments de réseau, y compris les passerelles RAS, équilibreurs de charge logiciels, des stratégies de mise en réseau virtuels pour la communication client, de manière centralisée les pare-feu de centre de données stratégies de qualité de Service \(QoS\) pour SDN stratégies, stratégies de mise en réseau hybride et bien plus encore.

Étant donné que le contrôleur de réseau est la pierre angulaire de gestion SDN, il est essentiel pour les déploiements de contrôleur de réseau haute disponibilité et la possibilité de vous fournir à l’échelle facilement monter ou Descendre les nœuds de contrôleur de réseau avec les besoins de votre centre de données.

Bien que vous pouvez déployer le contrôleur de réseau en tant que machine unique cluster, pour le basculement et une haute disponibilité, vous devez déployer le contrôleur de réseau dans un cluster de machines multiples avec un minimum de trois ordinateurs.

>[!NOTE]
>Vous pouvez déployer le contrôleur de réseau sur des ordinateurs de serveur ou sur des machines virtuelles \(machines virtuelles\) qui exécutent Windows Server 2016 Datacenter edition. Si vous déployez le contrôleur de réseau sur des machines virtuelles, les machines virtuelles doivent s’exécuter sur les hôtes Hyper-V qui sont exécutent également Datacenter edition. Contrôleur de réseau n’est pas disponible sur Windows Server 2016 Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Contrôleur de réseau comme une Application Service Fabric

Pour obtenir évolutivité et haute disponibilité, le contrôleur de réseau s’appuie sur Service Fabric. Service Fabric fournit une plateforme de systèmes distribués pour créer évolutives, fiables et facile à gérer des applications.

En tant que plateforme, Service Fabric fournit des fonctionnalités qui est requises pour la création d’un système distribué évolutif. Il assure l’hébergement de service sur plusieurs instances de système d’exploitation, synchronisation des informations d’état entre des instances, élire un responsable, la détection de défaillance, l’équilibrage de charge et bien plus encore.

>[!NOTE]
>Pour plus d’informations sur Service Fabric dans Azure, consultez [vue d’ensemble d’Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Lorsque vous déployez un contrôleur de réseau sur plusieurs ordinateurs, contrôleur de réseau s’exécute comme une seule application de Service Fabric sur un cluster Service Fabric. Vous pouvez former un cluster Service Fabric en vous connectant à un ensemble d’instances de système d’exploitation.

L’application de contrôleur de réseau se compose de plusieurs services Service Fabric avec état. Chaque service est responsable pour une fonction réseau, telles que la gestion de réseau physique, la gestion de réseau virtuel, gestion du pare-feu ou gestion de la passerelle. 

Chaque Service service Fabric a un réplica principal et deux réplicas secondaires. Le réplica principal de service traite les demandes, tandis que les deux réplicas de service secondaire fournissent une haute disponibilité dans les cas où le réplica principal est désactivé ou indisponible pour une raison quelconque.

L’illustration suivante représente un cluster Service Fabric contrôleur de réseau avec cinq machines. Quatre services sont réparties entre les cinq ordinateurs : Service, Service de passerelle, équilibrage de charge de pare-feu \(SLB\) service et le réseau virtuel \(réseau virtuel\) service.  Chacun des quatre services inclut un réplica de service principal et deux réplicas de service secondaire.

![Cluster Service Fabric de contrôleur de réseau](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Avantages de l’utilisation de Service Fabric

Voici les principaux avantages d’utilisation de Service Fabric pour les clusters de contrôleur de réseau.

### <a name="high-availability-and-scalability"></a>Évolutivité et haute disponibilité

Étant donné que le contrôleur de réseau est le cœur d’un réseau de centre de données, il doit être résistant aux défaillances et être suffisamment flexible pour permettre des modifications agiles dans des réseaux de centre de données au fil du temps. Les fonctionnalités suivantes fournissent ces capacités : 

- **Basculement rapide**. Service Fabric fournit un basculement très rapide. Plusieurs réplicas de service secondaire à chaud sont toujours disponibles. Si une instance de système d’exploitation devenue indisponible en raison d’une défaillance matérielle, un des réplicas secondaires est promu immédiatement au réplica principal. 
- **Agilité de mise à l’échelle**. Vous pouvez facilement et rapidement mettre à l’échelle de ces services fiables à partir de quelques instances allant jusqu'à plusieurs milliers d’instances, puis vers le bas à nouveau quelques instances, selon vos besoins en ressources. 

### <a name="persistent-storage"></a>Stockage persistant

L’application de contrôleur de réseau a des exigences de stockage volumineux pour sa configuration et son état. L’application également doit être utilisable sur les interruptions planifiées et. À cet effet, Service Fabric fournit un Store de clé-valeur \(KVS\) qui est un magasin répliqué, transactionnels et persistant.

### <a name="modularity"></a>Modularité

Contrôleur de réseau est conçu avec une architecture modulaire, avec chacun des services réseau, telles que les services de réseaux virtuels et pare-feu, conçues\-dans en tant que services individuels. 

Cette architecture d’application offre les avantages suivants.

1. Contrôleur de réseau modularité permet un développement indépendant de chacun des services pris en charge, en tant que doit évoluer. Par exemple, le service Équilibrage de charge peut être mis à jour sans affecter aucune des autres services ou le fonctionnement normal du contrôleur de réseau.
2. Modularité de contrôleur de réseau permet l’ajout de nouveaux services, à mesure que le réseau évolue. Nouveaux services peuvent être ajoutés au contrôleur de réseau sans impact sur les services existants.

>[!NOTE]
>Dans Windows Server 2016, l’ajout de services tiers au contrôleur de réseau n’est pas pris en charge.

Modularité de service Fabric utilise les schémas de modèle de service pour optimiser la facilité de développement, de déploiement et de maintenance d’une application.

## <a name="network-controller-deployment-options"></a>Options de déploiement de contrôleur de réseau

Pour déployer un contrôleur de réseau à l’aide de System Center Virtual Machine Manager \(VMM\), consultez [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Pour déployer un contrôleur de réseau à l’aide de scripts, consultez [déployer un logiciel défini Infrastructure à l’aide de Scripts réseau](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Pour déployer un contrôleur de réseau à l’aide de Windows PowerShell, consultez [déployer de contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Pour plus d’informations sur le contrôleur de réseau, consultez [contrôleur de réseau](Network-Controller.md).