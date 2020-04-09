---
title: Planifier la mise en réseau Hyper-V dans Windows Server
description: Décrit ce qui est nécessaire pour la mise en réseau de base dans Hyper-V et fournit des liens vers des instructions
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3127c9579493ad8b317667b61de88304fd14f6cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860762"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Planifier la mise en réseau Hyper-V dans Windows Server

>S’applique à : Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019
  
Une compréhension élémentaire de la mise en réseau dans Hyper-V vous aide à planifier la mise en réseau pour les machines virtuelles. Cet article traite également de certaines considérations relatives à la mise en réseau lors de l’utilisation de la migration dynamique et de l’utilisation d’Hyper-V avec d’autres fonctionnalités serveur et des rôles.  
  
## <a name="hyper-v-networking-basics"></a>Notions de base de la mise en réseau Hyper-V  
La mise en réseau de base dans Hyper-V est relativement simple. Il utilise deux parties : un commutateur virtuel et une carte réseau virtuelle. Vous aurez besoin d’au moins une des deux pour établir la mise en réseau d’une machine virtuelle. Le commutateur virtuel se connecte à n’importe quel réseau Ethernet. La carte réseau virtuelle se connecte à un port sur le commutateur virtuel, ce qui permet à une machine virtuelle d’utiliser un réseau.  
  
Le moyen le plus simple d’établir une mise en réseau de base consiste à créer un commutateur virtuel lorsque vous installez Hyper-V. Ensuite, lorsque vous créez une machine virtuelle, vous pouvez la connecter au commutateur. La connexion au commutateur ajoute automatiquement une carte réseau virtuelle à la machine virtuelle. Pour obtenir des instructions, consultez [créer un commutateur virtuel pour les machines virtuelles Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Pour gérer différents types de mise en réseau, vous pouvez ajouter des commutateurs virtuels et des cartes réseau virtuelles. Tous les commutateurs font partie de l’hôte Hyper-V, mais chaque carte réseau virtuelle appartient à un seul ordinateur virtuel.  
  
Le commutateur virtuel est un commutateur réseau Ethernet de couche 2 basé sur logiciel. Il fournit des fonctionnalités intégrées pour la surveillance, le contrôle et la segmentation du trafic, ainsi que la sécurité et les Diagnostics.  Vous pouvez ajouter à l’ensemble des fonctionnalités intégrées en installant des plug-ins, également appelés *Extensions*. Celles-ci sont disponibles auprès des éditeurs de logiciels indépendants. Pour plus d’informations sur le commutateur et les extensions, consultez [commutateur virtuel Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Choix des cartes réseau et des commutateurs  
Hyper-V offre trois types de commutateurs virtuels et deux types de cartes réseau virtuelles. Vous devez choisir l’un de vos choix lorsque vous le créez. Vous pouvez utiliser le Gestionnaire Hyper-V ou le module Hyper-V pour Windows PowerShell pour créer et gérer des commutateurs virtuels et des cartes réseau virtuelles. Certaines fonctionnalités de mise en réseau avancées, telles que les listes de contrôle d’accès (ACL) de port étendues, peuvent être gérées uniquement à l’aide des applets de commande du module Hyper-V.  
  
Vous pouvez apporter des modifications à un commutateur virtuel ou à une carte réseau virtuelle après l’avoir créé. Par exemple, il est possible de remplacer un commutateur existant par un autre type, mais cela affecte les fonctionnalités de mise en réseau de tous les ordinateurs virtuels connectés à ce commutateur.  Vous ne pouvez donc pas effectuer cette opération, sauf si vous avez commis une erreur ou si vous avez besoin de tester un événement. En guise d’autre exemple, vous pouvez connecter une carte réseau virtuelle à un autre commutateur, ce que vous pouvez faire si vous souhaitez vous connecter à un autre réseau. Toutefois, vous ne pouvez pas modifier une carte réseau virtuelle d’un type en un autre. Au lieu de modifier le type, vous devez ajouter une autre carte réseau virtuelle et choisir le type approprié.  
  
Les types de commutateurs virtuels sont les suivants :  
  
-   **Commutateur virtuel externe** : se connecte à un réseau physique câblé par liaison à une carte réseau physique.  
  
-   **Commutateur virtuel interne** : se connecte à un réseau qui ne peut être utilisé que par les ordinateurs virtuels en cours d’exécution sur l’ordinateur hôte disposant du commutateur virtuel, et entre l’ordinateur hôte et les ordinateurs virtuels.  
  
-   **Commutateur virtuel privé** : se connecte à un réseau qui ne peut être utilisé que par les ordinateurs virtuels en cours d’exécution sur l’ordinateur hôte doté du commutateur virtuel, mais ne fournit pas de mise en réseau entre l’ordinateur hôte et les ordinateurs virtuels.  
  
Les types de cartes réseau virtuelles sont les suivants :  
  
-   **Carte réseau spécifique à Hyper-V** : disponible pour les ordinateurs virtuels de génération 1 et de génération 2. Il est conçu spécifiquement pour Hyper-V et nécessite un pilote qui est inclus dans les services d’intégration Hyper-V. Ce type de carte réseau est plus rapide et est recommandé, sauf si vous devez démarrer sur le réseau ou si vous exécutez un système d’exploitation invité non pris en charge. Le pilote requis est fourni uniquement pour les systèmes d’exploitation invités pris en charge. Notez que dans le Gestionnaire Hyper-V et les applets de commande de mise en réseau, ce type est simplement appelé carte réseau.  
  
-   **Carte réseau héritée** : disponible uniquement sur les ordinateurs virtuels de génération 1. Émule un adaptateur Fast Ethernet PCI basé sur Intel 21140 et peut être utilisé pour démarrer sur un réseau, ce qui vous permet d’installer un système d’exploitation à partir d’un service tel que les services de déploiement Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Mise en réseau Hyper-V et technologies associées  
Les versions récentes de Windows Server ont introduit des améliorations qui vous offrent davantage d’options pour la configuration de la mise en réseau pour Hyper-V. Par exemple, Windows Server 2012 a introduit la prise en charge de la mise en réseau convergée. Cela vous permet de router le trafic réseau via un commutateur virtuel externe. Windows Server 2016 s’appuie sur cela en autorisant l’accès direct à la mémoire à distance (RDMA) sur les cartes réseau liées à un commutateur virtuel Hyper-V. Vous pouvez utiliser cette configuration avec ou sans switch Embedded Teaming (SET). Pour plus d’informations, consultez [accès &#40;direct à&#41; la mémoire à distance RDMA &#40;et&#41; Switch Embedded Teaming Set](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Certaines fonctionnalités s’appuient sur des configurations de mise en réseau spécifiques ou sont mieux adaptées à certaines configurations. Tenez compte des cas suivants lors de la planification ou de la mise à jour de votre infrastructure réseau.  
  
**Clustering de basculement** : il est recommandé d’isoler le trafic de cluster et d’utiliser la qualité de service (QoS) Hyper-V sur le commutateur virtuel. Pour plus d’informations, consultez [Network Recommendations for a Hyper-V cluster (en](https://technet.microsoft.com/library/dn550728.aspx) anglais).  
  
**Migration dynamique** : utilisez les options de performances pour réduire l’utilisation du réseau et du processeur, ainsi que le temps nécessaire pour effectuer une migration dynamique. Pour obtenir des instructions, consultez [configurer des hôtes pour la migration dynamique sans clustering de basculement](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Espaces de stockage direct** : cette fonctionnalité s’appuie sur le protocole réseau SMB 3.0 et RDMA. Pour plus d’informations, consultez [espaces de stockage direct dans Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).