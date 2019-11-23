---
title: Contrôleur de réseau haute disponibilité
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la haute disponibilité du contrôleur de réseau pour la mise en réseau SDN (Software Defined Networking) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 11f392e99803f0e0ddd0f8b62c9dbca5827a831c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405932"
---
# <a name="network-controller-high-availability"></a>Contrôleur de réseau haute disponibilité

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la configuration de la haute disponibilité et de l’évolutivité du contrôleur de réseau pour la mise en réseau \(SDN\).

Lorsque vous déployez SDN dans votre centre de informations, vous pouvez utiliser le contrôleur de réseau pour déployer, surveiller et gérer de manière centralisée de nombreux éléments réseau, y compris les passerelles RAS, les équilibreurs de charge logiciels, les stratégies de réseau virtuel pour la communication avec les locataires, les stratégies de pare-feu de centre de informations, la qualité de service \(QoS\) pour les

Étant donné que le contrôleur de réseau est la pierre angulaire de la gestion de SDN, il est essentiel pour les déploiements de contrôleur de réseau de fournir une haute disponibilité et la possibilité de mettre à l’échelle facilement des nœuds de contrôleur de réseau avec vos besoins en matière de centre de donnes.

Bien que vous puissiez déployer un contrôleur de réseau en tant que cluster à ordinateur unique, pour la haute disponibilité et le basculement, vous devez déployer le contrôleur de réseau dans un cluster à plusieurs ordinateurs avec un minimum de trois machines.

>[!NOTE]
>Vous pouvez déployer un contrôleur de réseau sur les ordinateurs serveurs ou sur des machines virtuelles \(les machines virtuelles\) qui exécutent Windows Server 2016 Datacenter Edition. Si vous déployez un contrôleur de réseau sur des machines virtuelles, les machines virtuelles doivent être en cours d’exécution sur des ordinateurs hôtes Hyper-V qui exécutent également Datacenter Edition. Le contrôleur de réseau n’est pas disponible sur Windows Server 2016 Standard Edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Contrôleur de réseau en tant qu’application Service Fabric

Pour obtenir une disponibilité et une évolutivité élevées, le contrôleur de réseau s’appuie sur Service Fabric. Service Fabric fournit une plateforme de systèmes distribués pour créer des applications évolutives, fiables et faciles à gérer.

En tant que plateforme, Service Fabric fournit les fonctionnalités nécessaires à la création d’un système distribué évolutif. Il permet l’hébergement de service sur plusieurs instances de système d’exploitation, la synchronisation des informations d’État entre les instances, l’élection d’un responsable, la détection de défaillance, l’équilibrage de charge, etc.

>[!NOTE]
>Pour plus d’informations sur les Service Fabric dans Azure, consultez [vue d’ensemble d’azure service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Lorsque vous déployez un contrôleur de réseau sur plusieurs ordinateurs, le contrôleur de réseau s’exécute en tant qu’application Service Fabric unique sur un cluster Service Fabric. Vous pouvez former un cluster Service Fabric en connectant un ensemble d’instances de système d’exploitation.

L’application de contrôleur de réseau est composée de plusieurs services de Service Fabric avec état. Chaque service est responsable d’une fonction réseau, telle que la gestion de réseau physique, la gestion de réseau virtuel, la gestion de pare-feu ou la gestion de passerelle. 

Chaque service de Service Fabric a un réplica principal et deux réplicas secondaires. Le réplica de service principal traite les demandes, tandis que les deux réplicas de service secondaires offrent une haute disponibilité dans les cas où le réplica principal est désactivé ou indisponible pour une raison quelconque.

L’illustration suivante représente un contrôleur de réseau Service Fabric cluster avec cinq machines. Quatre services sont répartis sur les cinq ordinateurs : le service de pare-feu, le service de passerelle, l’équilibrage de la charge logicielle \(service SLB\) et le service\) de réseau virtuel \(vnet.  Chacun des quatre services comprend un réplica de service principal et deux réplicas de service secondaires.

![Contrôleur de réseau Service Fabric cluster](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Avantages de l’utilisation de Service Fabric

Voici les principaux avantages de l’utilisation de Service Fabric pour les clusters de contrôleur de réseau.

### <a name="high-availability-and-scalability"></a>Haute disponibilité et évolutivité

Étant donné que le contrôleur de réseau est le cœur d’un réseau de centre de temps, il doit être résilient à la défaillance et être suffisamment évolutif pour permettre des modifications agiles dans les réseaux de centres de centres au fil du temps. Les fonctionnalités suivantes offrent les possibilités suivantes : 

- **Basculement rapide**. Service Fabric offre un basculement extrêmement rapide. Plusieurs réplicas de service secondaire à chaud sont toujours disponibles. Si une instance de système d’exploitation devient indisponible en raison d’une défaillance matérielle, l’un des réplicas secondaires est immédiatement promu au réplica principal. 
- **Agilité de l’échelle**. Vous pouvez facilement et rapidement mettre à l’échelle ces services fiables à partir de quelques milliers d’instances, puis revenir à quelques instances, en fonction de vos besoins en ressources. 

### <a name="persistent-storage"></a>Stockage persistant

L’application du contrôleur de réseau a des exigences de stockage importantes pour sa configuration et son état. L’application doit également être utilisable sur des interruptions planifiées et non planifiées. À cet effet, Service Fabric fournit un magasin clé-valeur \(KVS\) qui est un magasin répliqué, transactionnel et persistant.

### <a name="modularity"></a>Modularité

Le contrôleur de réseau est conçu avec une architecture modulaire, avec chacun des services réseau, tels que le service des réseaux virtuels et le service de pare-feu, conçu\-dans en tant que services individuels. 

Cette architecture d’application offre les avantages suivants.

1. La modularité du contrôleur de réseau permet le développement indépendant de chacun des services pris en charge, car les besoins évoluent. Par exemple, le service d’équilibrage de charge logiciel peut être mis à jour sans affecter les autres services ou le fonctionnement normal du contrôleur de réseau.
2. La modularité du contrôleur de réseau permet l’ajout de nouveaux services au fur et à mesure que le réseau évolue. De nouveaux services peuvent être ajoutés au contrôleur de réseau sans impact sur les services existants.

>[!NOTE]
>Dans Windows Server 2016, l’ajout de services tiers au contrôleur de réseau n’est pas pris en charge.

La modularité Service Fabric utilise des schémas de modèle de service pour optimiser la facilité de développement, de déploiement et de maintenance d’une application.

## <a name="network-controller-deployment-options"></a>Options de déploiement du contrôleur de réseau

Pour déployer un contrôleur de réseau à l’aide de System Center Virtual Machine Manager \(VMM\), consultez [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Pour déployer un contrôleur de réseau à l’aide de scripts, consultez [déployer une infrastructure réseau définie par logiciel à l’aide de scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Pour déployer un contrôleur de réseau à l’aide de Windows PowerShell, consultez [déployer un contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Pour plus d’informations sur le contrôleur de réseau, consultez [contrôleur de réseau](Network-Controller.md).