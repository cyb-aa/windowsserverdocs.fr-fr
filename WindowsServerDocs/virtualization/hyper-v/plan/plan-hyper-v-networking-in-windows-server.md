---
title: Plan de mise en réseau Hyper-V dans Windows Server
description: Décrit ce qui est nécessaire pour la mise en réseau de base dans Hyper-V et fournit des liens vers des instructions
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 83c014b917566a78796d061dd88962966ec0c9ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829870"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Plan de mise en réseau Hyper-V dans Windows Server

>S'applique à : Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019
  
Une connaissance élémentaire de mise en réseau dans Hyper-V vous permet de planifier la mise en réseau pour les machines virtuelles. Cet article décrit également certaines considérations relatives à la mise en réseau lors de l’utilisation de la migration dynamique et lors de l’utilisation de Hyper-V avec d’autres fonctionnalités de serveur et les rôles.  
  
## <a name="hyper-v-networking-basics"></a>Principes fondamentaux de la mise en réseau de Hyper-V  
Mise en réseau de base dans Hyper-V est relativement simple. Elle utilise deux parties : un commutateur virtuel et une carte réseau virtuelle. Vous devez au moins un de chacun d’eux à établir la mise en réseau pour une machine virtuelle. Le commutateur virtuel se connecte à n’importe quel réseau Ethernet. La carte réseau virtuelle se connecte à un port sur le commutateur virtuel, ce qui rend possible pour une machine virtuelle à utiliser un réseau.  
  
Pour établir la mise en réseau de base, le plus simple consiste à créer un commutateur virtuel lorsque vous installez Hyper-V. Ensuite, lorsque vous créez une machine virtuelle, vous pouvez vous connecter au commutateur. Se connecter automatiquement au commutateur ajoute une carte réseau virtuelle à la machine virtuelle. Pour obtenir des instructions, consultez [créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Pour gérer différents types de mise en réseau, vous pouvez ajouter des commutateurs virtuels et les cartes réseau virtuelles. Tous les commutateurs font partie de l’hôte Hyper-V, mais chaque carte réseau virtuelle appartient à une seule machine virtuelle.  
  
Le commutateur virtuel est un commutateur de réseau Ethernet basé sur logiciel des layer-2. Il fournit des fonctionnalités intégrées de surveillance, contrôle et segmentation du trafic, ainsi que la sécurité et diagnostics.  Vous pouvez ajouter à l’ensemble des fonctionnalités intégrées en installant le plug-ins, également appelés *extensions*. Ils sont disponibles à partir des éditeurs de logiciels indépendants. Pour plus d’informations sur le commutateur et les extensions, consultez [commutateur virtuel Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Choix de carte réseau et du commutateur  
Hyper-V propose trois types de commutateurs virtuels et les deux types de cartes réseau virtuelles. Vous allez choisir celui de chaque souhaité lors de sa création. Vous pouvez utiliser le Gestionnaire Hyper-V ou le module Hyper-V pour Windows PowerShell pour créer et gérer des commutateurs virtuels et les cartes réseau virtuelles. Certaines fonctionnalités de mise en réseau avancées, tels que les listes de contrôle d’accès de port étendues (ACL), peuvent uniquement être gérées à l’aide des applets de commande dans le module Hyper-V.  
  
Vous pouvez modifier certains à un commutateur virtuel ou de la carte réseau virtuelle après sa création. Par exemple, il est possible de modifier un commutateur existant en un type différent, mais cela affecte les fonctionnalités réseau de tous les ordinateurs virtuels connectés à ce commutateur.  Par conséquent, vous probablement ne sont pas cela, sauf si vous avez fait une erreur ou devrez tester quelque chose. Autre exemple, vous pouvez vous connecter à une carte réseau virtuelle à un autre commutateur, vous pouvez le faire si vous souhaitez vous connecter à un autre réseau. Toutefois, vous ne pouvez pas modifier une carte réseau virtuelle d’un type vers un autre. Au lieu de modifier le type, vous ajoutez une autre carte réseau virtuelle et choisissez le type approprié.  
  
Types de commutateur virtuel sont :  
  
-   **Commutateur virtuel externe** -se connecte à un réseau câblé, physique en le liant à une carte réseau physique.  
  
-   **Commutateur virtuel interne** -se connecte à un réseau qui peut être utilisé uniquement par les machines virtuelles en cours d’exécution sur l’hôte qui a le commutateur virtuel et entre l’hôte et les machines virtuelles.  
  
-   **Commutateur virtuel privé** -se connecte à un réseau qui peut être utilisé uniquement par les machines virtuelles en cours d’exécution sur l’ordinateur hôte qui a le commutateur virtuel, mais ne fournit pas de mise en réseau entre l’hôte et les machines virtuelles.  
  
Types d’adaptateur de réseau virtuel sont :  
  
-   **Carte réseau spécifique de Hyper-V** - disponible pour la génération 1 et machines virtuelles de génération 2. Il est spécifiquement conçu pour Hyper-V et nécessite un pilote qui est inclus dans les services d’intégration Hyper-V. Ce type de carte réseau plus rapide et est recommandé, sauf si vous avez besoin de démarrer au réseau ou exécutez un système d’exploitation non pris en charge. Le pilote requis est fourni que pour les systèmes d’exploitation invités pris en charge. Notez que dans le Gestionnaire Hyper-V et les applets de commande de mise en réseau, ce type est simplement appelé une carte réseau.  
  
-   **Carte réseau héritée** : disponible uniquement dans les machines virtuelles de génération 1. Émule un basé sur Intel 21140 carte Fast Ethernet PCI et peut être utilisé pour démarrer à un réseau, par conséquent, vous pouvez installer un système d’exploitation à partir d’un service tel que les Services de déploiement Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Mise en réseau Hyper-V et les technologies associées  
Les versions récentes de Windows Server introduit des améliorations qui offrent des options supplémentaires pour la configuration de mise en réseau pour Hyper-V. Par exemple, Windows Server 2012 introduit la prise en charge pour convergé mise en réseau. Cela vous permet d’acheminer le trafic réseau via un commutateur virtuel externe. Windows Server 2016 s’appuie sur cela en autorisant l’accès de mémoire Direct à distance (RDMA) sur les cartes réseau liées à un commutateur virtuel Hyper-V. Vous pouvez utiliser cette configuration avec ou sans commutateur SET (Embedded Teaming). Pour plus d’informations, consultez [Remote Direct Memory Access &#40;RDMA&#41; et Switch Embedded Teaming &#40;définie&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Certaines fonctionnalités s’appuient sur des configurations de réseau spécifiques, ou faire mieux sous certaines configurations. Prenons les lors de la planification ou la mise à jour de votre infrastructure réseau.  
  
**Le clustering de basculement** -il vaut mieux isoler le trafic de cluster et l’utilisation de qualité de Service (QoS) Hyper-V sur le commutateur virtuel. Pour plus d’informations, consultez [recommandations de réseau pour un Cluster Hyper-V](https://technet.microsoft.com/library/dn550728.aspx)  
  
**Migration dynamique** -utiliser les options de performances afin de réduire le temps nécessaire pour effectuer une migration dynamique et réseau et l’utilisation du processeur. Pour obtenir des instructions, consultez [configurer des hôtes pour la migration dynamique sans Clustering de basculement](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Espaces de stockage Direct** -cette fonctionnalité repose sur le protocole de réseau SMB3.0 et RDMA. Pour plus d’informations, consultez [espaces de stockage Direct dans Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).