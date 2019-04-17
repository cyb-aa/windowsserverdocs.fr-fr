---
title: Haute disponibilité du contrôleur de réseau
description: Vous pouvez utiliser cette rubrique pour en savoir plus sur la haute disponibilité du contrôleur de réseau pour les logiciels défini de mise en réseau (SDN) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: 334b090d-bec4-4e67-8307-13831dbdd1d8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f260f3e4d8ca5fcd998824327478c2fbe3c81875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-high-availability"></a>Haute disponibilité du contrôleur de réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur le contrôleur de réseau haut disponibilité et de configuration de l’évolutivité pour \(SDN\) logiciel défini de mise en réseau.

Lorsque vous déployez SDN dans votre centre de données, vous pouvez utiliser le contrôleur de réseau déployer, surveiller et gérer de nombreux éléments de réseau, y compris les passerelles RAS, équilibreurs de charge logicielle, des stratégies de réseau virtuels pour la communication client, les stratégies de pare-feu de centre de données, \(QoS\) la qualité de Service pour les stratégies de type SDN, les stratégies de mise en réseau hybride et plus de manière centralisée.

Étant donné que le contrôleur de réseau est la pierre angulaire de gestion de type SDN, il est essentiel pour les déploiements de contrôleur de réseau haute disponibilité et la possibilité de vous fournir à l’échelle facilement vers le haut ou vers le bas des nœuds de contrôleur de réseau avec les besoins de votre centre de données.

Bien que vous pouvez déployer le contrôleur de réseau comme un cluster d’ordinateur unique, pour une haute disponibilité et le basculement, vous devez déployer le contrôleur de réseau dans un cluster avec un minimum de trois ordinateurs comprenant plusieurs ordinateurs.

>[!NOTE]
>Vous pouvez déployer le contrôleur de réseau sur les deux ordinateurs serveur ou sur \(VMs\) des ordinateurs virtuels qui exécutent Windows Server 2016 Datacenter edition. Si vous déployez le contrôleur de réseau sur les ordinateurs virtuels, les ordinateurs virtuels doivent être exécuté sur les ordinateurs hôtes Hyper-V qui exécutent également Datacenter edition. Contrôleur de réseau n’est pas disponible sur Windows Server 2016, Standard edition.

## <a name="network-controller-as-a-service-fabric-application"></a>Contrôleur de réseau en tant qu’une Application de Service de structure

Pour obtenir l’évolutivité et haute disponibilité, le contrôleur de réseau s’appuie sur Service Fabric. Service Fabric fournit une plateforme de systèmes distribués pour générer évolutive, fiable et facile à gérer des applications.

En tant que plateforme, Service Fabric fournit des fonctionnalités qui sont requises pour la création d’un système distribué évolutif. Il fournit des services d’hébergement sur plusieurs instances du système d’exploitation, la synchronisation des informations d’état entre les instances, élection un leader, la détection de défaillance, l’équilibrage de charge et plus encore.

>[!NOTE]
>Pour plus d’informations sur le Service Fabric dans Azure, consultez [vue d’ensemble de Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Lorsque vous déployez un contrôleur de réseau sur plusieurs ordinateurs virtuels, le contrôleur de réseau s’exécute en tant qu’une seule application Service Fabric sur un cluster de la structure du Service. Vous pouvez former un cluster de Service Fabric en vous connectant à un ensemble d’instances du système d’exploitation.

L’application de contrôleur de réseau se compose de plusieurs services Service Fabric avec état. Chaque service est responsable pour une fonction de réseau, telles que la gestion de réseau physique, la gestion de réseau virtuel, la gestion de pare-feu ou gestion de la passerelle. 

Chaque service Service Fabric a un réplica principal et deux réplicas secondaires. Le réplica principal service traite les demandes, tandis que les deux réplicas service secondaire fournissent une haute disponibilité dans les cas où le réplica principal est désactivé ou non disponible pour une raison quelconque.

L’illustration suivante représente un cluster réseau contrôleur Service Fabric avec cinq ordinateurs. Quatre services sont distribués sur les cinq ordinateurs: Service de pare-feu, Service de passerelle, service \(SLB\) l’équilibrage de charge logicielle et service \(Vnet\) de réseau virtuel.  Chacun des quatre services inclut un réplica du service principal et deux réplicas service secondaire.

![Cluster de structure de Service de contrôleur de réseau](../../../media/Network-Controller-HA/Network-Controller-HA.jpg)

## <a name="advantages-of-using-service-fabric"></a>Avantages de l’utilisation du Service Fabric

Voici les principaux avantages pour l’utilisation de la structure de Service pour les clusters de contrôleur de réseau.

### <a name="high-availability-and-scalability"></a>Évolutivité et haute disponibilité

Étant donné que le contrôleur de réseau est au cœur d’un réseau de centre de données, il doit résistants aux défaillances et être suffisamment évolutive pour autoriser les modifications agiles dans les réseaux de centre de données au fil du temps. Les fonctionnalités suivantes fournissent ces fonctionnalités: 

- **Basculement rapide**. Service Fabric fournit un basculement extrêmement rapide. Plusieurs réplicas à chaud service secondaire sont toujours disponibles. Si une instance de système d’exploitation devenue indisponible à cause de panne matérielle, un réplica secondaire est immédiatement promu à réplica principal. 
- **Flexibilité de montée en puissance parallèle**. Vous pouvez facilement et rapidement ces services fiables à partir de plusieurs instances des milliers d’instances de l’échelle et puis vers le bas quelques cas, en fonction de vos besoins en ressources. 

### <a name="persistent-storage"></a>Stockage persistant

L’application de contrôleur de réseau a des exigences de stockage volumineux pour sa configuration et l’état. L’application également doit être utilisable sur les interruptions planifiées et. Dans ce but, Service Fabric fournit un \(KVS\) magasin clé-valeur qui est un magasin répliqué, transactionnels et conservé.

### <a name="modularity"></a>Modularité

Contrôleur de réseau est conçu avec une architecture modulaire, chacun des services réseau, tels que le service de réseaux virtuels et le service de pare-feu, intégrée en tant que services individuels. 

Cette architecture d’application offre les avantages suivants.

1. Modularité permet de développement indépendant de chacun des services pris en charge, en tant que le contrôleur de réseau doit évoluer. Par exemple, le service d’équilibrage de charge logicielle permettre être mis à jour sans affecter les autres services ou le fonctionnement normal du contrôleur de réseau.
2. Modularité de contrôleur de réseau permet l’ajout de nouveaux services, comme le réseau évolue. Nouveaux services peuvent être ajoutés au contrôleur de réseau sans impact sur les services existants.

>[!NOTE]
>Dans Windows Server 2016, l’ajout de services tiers pour le contrôleur de réseau n’est pas pris en charge.

Modularité service Fabric utilise les schémas de modèle de service pour optimiser la facilité de développement, de déploiement et de maintenance d’une application.

## <a name="network-controller-deployment-options"></a>Options de déploiement de contrôleur de réseau

Pour déployer le contrôleur de réseau à l’aide de System Center Virtual Machine Manager \(VMM\), voir [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Pour déployer un contrôleur de réseau à l’aide de scripts, voir [déployer un logiciel défini Infrastructure à l’aide de Scripts réseau](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Pour déployer un contrôleur de réseau à l’aide de Windows PowerShell, voir [déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)

Pour plus d’informations sur le contrôleur de réseau, voir [contrôleur de réseau](Network-Controller.md).