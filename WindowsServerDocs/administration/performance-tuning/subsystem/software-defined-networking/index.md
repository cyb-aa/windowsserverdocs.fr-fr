---
title: Réglage des performances des réseaux à définition logicielle
description: Conseils pour le réglage des performances des réseaux à définition logicielle
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: dfb8d997c6e04381e5be0ba2c3a7ca27a851df50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891320"
---
# <a name="performance-tuning-software-defined-networks"></a>Réglage des performances des réseaux à définition logicielle

La mise en réseau à définition logicielle dans Windows Server 2016 associe un contrôleur de réseau, des hôtes Hyper-V, des passerelles d’équilibreur de charge logiciel et des passerelles HNV.  Pour le réglage de chacun de ces composants, consultez les sections suivantes :

## <a name="network-controller"></a>Contrôleur de réseau

Le contrôleur de réseau est un rôle Windows Server qui doit être activé sur les machines virtuelles s’exécutant sur des hôtes qui sont configurés pour utiliser la mise en réseau à définition logicielle et sont contrôlés par le contrôleur de réseau.

Trois machines virtuelles compatibles avec le contrôleur de réseau sont suffisantes pour bénéficier d’une haute disponibilité et de performances maximales.  Chaque machine virtuelle doit être dimensionnée en respectant les instructions fournies dans la section Configuration requise du rôle de machine virtuelle d’infrastructure SDN de la rubrique [Planifier une infrastructure SDN](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).

### <a name="sdn-quality-of-service-qos"></a>Qualité de service (QoS) des réseaux à définition logicielle

Pour que le trafic des machines virtuelles soit adapté de façon efficace et juste, nous vous recommandons de configurer la qualité de service de la mise en réseau à définition logicielle sur les machines virtuelles de la charge de travail.  Pour plus d’informations sur la configuration de la qualité de service de la mise en réseau à définition logicielle, consultez la rubrique [Configurer la qualité de service pour une carte réseau de machine virtuelle locataire](../../../../networking/sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md).

## <a name="hyper-v-host-networking"></a>Mise en réseau d’hôte Hyper-V

Les instructions fournies dans la section [Hyper-V network I/O performance (Performances d’E/S du réseau Hyper-V)](#netio) du guide [Performance Tuning for Hyper-V Servers (Réglage des performances des serveurs Hyper-V)](../../role/remote-desktop/session-hosts.md) sont applicables lorsque SDN est utilisé, mais cette section présente des instructions supplémentaires qui doivent être respectées pour garantir des performances optimales lors de l’utilisation de SDN.

### <a name="physical-network-adapter-nic-teaming"></a>Appairage de cartes réseau physiques

Pour des fonctionnalités de basculement et des performances optimales, nous vous recommandons de configurer les cartes réseau physiques à appairer.  Lorsque vous utilisez SDN, vous devez créer l’équipe avec Switch Embedded Teaming (SET).  

Le nombre optimal de membres d’équipe est de deux, car le trafic virtualisé est réparti entre les deux membres de l’équipe pour les directions entrante et sortante.  Vous pouvez avoir plus de deux membres dans l’équipe. Toutefois, dans ce cas, le trafic entrant est réparti sur au plus deux des cartes.  Le trafic sortant est toujours réparti entre toutes les cartes si le paramètre par défaut de l’équilibrage de charge dynamique reste configuré sur le commutateur virtuel.


### <a name="encapsulation-offloads"></a>Déchargements d’encapsulation

SDN s’appuie sur l’encapsulation de paquets pour virtualiser le réseau.  Pour des performances optimales, il est important que la carte réseau prenne en charge le déchargement de matériel pour le format d’encapsulation qui est utilisé.  Les différents formats d’encapsulation n’apportent aucun gain de performances significatif les uns par rapport aux autres.  VXLAN est format d’encapsulation par défaut lorsque le contrôleur de réseau est utilisé.

Vous pouvez déterminer le format d’encapsulation utilisé via le contrôleur de réseau avec l’applet de commande PowerShell suivante :

``` syntax
    (Get-NetworkControllerVirtualNetworkConfiguration -connectionuri $uri).properties.networkvirtualizationprotocol
```

Pour de meilleures performances, si VXLAN est retourné, vous devez vous assurer que vos cartes réseau physiques prennent en charge le déchargement de tâches VXLAN.  Si NVGRE est retourné, vos cartes réseau physiques doivent prendre en charge le déchargement de tâches NVGRE.

### <a name="mtu"></a>MTU

L’encapsulation entraîne l’ajout d’octets supplémentaires à chaque paquet.  Afin d’éviter la fragmentation de ces paquets, le réseau physique doit être configuré pour utiliser des trames jumbo.  Une valeur MTU de 9 234 est la taille recommandée pour VXLAN ou NVGRE et doit être configurée sur le commutateur physique pour les interfaces physiques des ports hôte (L2) et des interfaces de routeur (L3) des VLAN sur lesquels les paquets encapsulés sont envoyés.  Cela inclut les réseaux de transit, de fournisseur HNV et de gestion.

MTU sur l’ordinateur hôte Hyper-V est configuré via la carte réseau, et l’Agent hôte du contrôleur de réseau exécuté sur l’ordinateur hôte Hyper-V s’ajuste automatiquement à la surcharge d’encapsulation si cela est pris en charge par le pilote de carte réseau.  

Une fois que le trafic sort du réseau virtuel via une passerelle, l’encapsulation est supprimée et la taille MTU d’origine envoyée à partir de la machine virtuelle est utilisée.

### <a name="single-root-io-virtualization-sr-iov"></a>Virtualisation d’E/S d’une racine unique (SR-IOV)

SDN est implémentée sur l’hôte Hyper-V à l’aide d’une extension de commutateur de transfert dans le commutateur virtuel.  Pour que cette extension de commutateur traite les paquets, SR-IOV ne doit pas être utilisé sur des interfaces réseau virtuelles configurées pour une utilisation avec le contrôleur de réseau, car cela force le trafic de machine virtuelle à ignorer le commutateur virtuel.

SR-IOV peut toujours être activé sur le commutateur virtuel, si vous le souhaitez, et peut être utilisé par les cartes réseau de machine virtuelle qui ne sont pas contrôlées par le contrôleur de réseau.  Ces machines virtuelles SR-IOV peuvent coexister sur le même commutateur virtuel que les machines virtuelles contrôlées par le contrôleur de réseau qui n’utilisent pas SR-IOV.

Si vous utilisez des cartes réseau de 40 Gbits, nous vous recommandons d’activer SR-IOV sur le commutateur virtuel pour les passerelles d’équilibrage de charge logiciel (SLB) afin d’atteindre le débit maximal.  Ce sujet est traité plus en détail dans la section [Passerelles d’équilibreur de charge logiciel](slb-gateway-performance.md).

## <a name="hnv-gateways"></a>Passerelle HNV

Vous trouverez plus d’informations sur le réglage des passerelles HNV pour une utilisation avec SDN dans la section [Passerelles HNV](hnv-gateway-performance.md).

## <a name="software-load-balancer-slb"></a>Équilibrage de la charge logicielle

Les passerelles SLB peuvent uniquement être utilisées avec le contrôleur de réseau et SDN.  Vous trouverez plus d’informations sur le réglage de la fonctionnalité SDN pour une utilisation avec des passerelles SLB dans la section [Passerelles d’équilibreur de charge logiciel](slb-gateway-performance.md).
