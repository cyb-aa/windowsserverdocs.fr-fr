---
title: Débit et performances du Tunnel GRE de la passerelle RAS
description: Cette rubrique, qui est destinée aux professionnels IT (Information Technology), fournit des informations de performances de débit sur les tunnels RAS passerelle Encapsulation GRE (Generic Routing).
manager: brianlic
ms.prod: windows-server-threshold
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73ae4e573d926f4a77b076c880c1d74ed69f032d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880760"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Débit et performances du Tunnel GRE de la passerelle RAS

>S’applique à : Windows Server \(canal semi-annuel\)

Vous pouvez utiliser cette rubrique pour en savoir plus sur le serveur d’accès à distance \(RAS\) passerelle Generic Routing Encapsulation \(GRE\) performances sur Windows Server, version 1709, dans un non - Sdn deTunnel\( SDN\) environnement de test basé sur.

Passerelle RAS est un routeur logiciel et une passerelle que vous pouvez utiliser en mode dédié ou en mode mutualisé. Cette rubrique présente un mode de client unique, la configuration haute disponibilité avec le Clustering de basculement. Les statistiques de performance de tunnel GRE sont présentés dans cette rubrique sont valides pour la passerelle RAS dans singele locataire et les modes mutualisées.

>[!NOTE]
>Le Clustering de basculement est une fonctionnalité de Windows Server qui vous permet de regrouper plusieurs serveurs dans un cluster à tolérance de pannes. Pour plus d’informations, consultez [le Clustering de basculement](../../../failover-clustering/failover-clustering-overview.md)

Mode de client unique permet aux organisations de toute taille pour déployer la passerelle comme un extérieur ou Internet\-accessible sur le réseau privé virtuel edge \(VPN\) server. En mode client unique, vous pouvez déployer la passerelle RAS sur un serveur physique ou une machine virtuelle \(machine virtuelle\). Cette rubrique décrit le déploiement de passerelle RAS sur deux Machines virtuelles \(machines virtuelles\) qui sont configurés dans un cluster de basculement.

>[!IMPORTANT]
>Étant donné que les tunnels GRE fournissent l’encapsulation, mais pas de chiffrement, vous n’employez pas de passerelle RAS configuré avec GRE comme une passerelle Internet. Pour en savoir plus sur les meilleures utilisations pour passerelle RAS avec les tunnels GRE, consultez [Tunneling GRE dans Windows Server](gre-tunneling-windows-server.md).

GRE est léger tunneling protocol qui peut encapsuler une grande variété de protocoles de couche réseau à l’intérieur du point virtuel\-à\-point des liens sur un réseau d’interconnexion de protocole Internet. L’implémentation Microsoft GRE encapsule à la fois IPv4 et IPv6.

Pour plus d’informations, consultez la section **scénarios de déploiement de passerelle RAS** dans la rubrique [passerelle RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

Dans ce scénario de test, ce qui est illustré dans l’illustration suivante, le flux de trafic est mesuré déplace à partir de l’Intranet d’organisation 2 out à l’Intranet d’organisation 1. Machines virtuelles de charge de travail de locataire envoyer le trafic réseau à partir de l’Intranet 2 à 1 de l’Intranet à l’aide de passerelle RAS.

![Présentation du scénario de test de tunnel GRE de la passerelle RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configuration d’environnement de test

Cette section fournit des informations sur l’environnement de test et la configuration de passerelle RAS.

Dans l’environnement de test, les machines virtuelles de passerelle RAS sont déployées sur Hyper\-hôtes V dans un basculement de cluster pour la haute disponibilité.

### <a name="hyper-v-host-configuration"></a>Hyper\-V Configuration de l’hôte

Deux Hyper\-hôtes V sont configurés pour prendre en charge le scénario de test de la manière suivante. 

- Deux double\-hébergées ordinateurs physiques sont configurés avec Windows Server, version 1709
- Les deux cartes réseau physiques dans chacun des deux serveurs sont connectés à des sous-réseaux différents - qui représentent les sous-réseaux d’un Intranet d’entreprise. Les réseaux et la prise en charge de matériel ont une capacité de 10 Gbits/s.
- L’hyperthreading sur les serveurs physiques est désactivée. Ainsi, le débit maximal de cartes réseau physiques.
- Le Hyper\-rôle serveur V est installé sur les deux serveurs et configuré avec deux externe Hyper\-V commutateurs virtuels, un pour chaque carte réseau physique.
- Étant donné que les deux serveurs sont connectés au même intranet, les serveurs peuvent communiquer entre eux.
- Le Hyper\-hôtes V sont configurés dans un cluster de basculement sur le réseau intranet. 

>[!NOTE]
>Pour plus d’informations, voir [Commutateur virtuel Hyper\-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configuration de machine virtuelle

Deux machines virtuelles sont configurés pour prendre en charge le scénario de test de la manière suivante.

- Sur chaque serveur sur qu'une machine virtuelle est installée qui est en cours d’exécution Windows Server, version 1709. Chaque machine virtuelle est configurée avec 10 cœurs et 8 Go de RAM.
- Chaque machine virtuelle est également configuré avec deux cartes réseau virtuelles. Une carte réseau virtuelle est connectée au commutateur virtuel 1 de l’Intranet, et l’autre carte réseau virtuel est connecté au commutateur virtuel 2 de l’Intranet.
- Chaque machine virtuelle a passerelle RAS installé et configuré comme un GRE\-en fonction de serveur VPN.
- Les ordinateurs virtuels passerelle sont configurés dans un cluster de basculement. Lors de la mise en cluster, une machine virtuelle est active et l’autre machine virtuelle est passif.

### <a name="workload-hyper-v-hosts-and-vms"></a>Charge de travail Hyper\-V hôtes et machines virtuelles

Pour ce test, deux charges de travail Hyper\-hôtes V sont installés sur l’intranet, et chaque hôte possède une seule machine virtuelle installée. Si vous dupliquez ce test dans votre propre environnement de test, vous pouvez installer autant de serveurs de la charge de travail et de machines virtuelles, selon vos besoins.

- Charge de travail Hyper\-hôtes V ont une carte réseau physique installé qui est connecté à l’Intranet de l’organisation.
- Dans Hyper\-V Virtual Switch, un seul commutateur virtuel est créé sur chaque hôte. Le commutateur est externe et est lié à la première carte réseau connectée à l’intranet.
- Les machines virtuelles de charge de travail sont configurés avec 2 Go de RAM et de 2 cœurs.
- La charge de travail des machines virtuelles chacun avoir une carte réseau virtuelle connectée au commutateur virtuel intranet.

### <a name="traffic-generator-tool"></a>Outil de génération de trafic

L’outil de génération de trafic qui est utilisé dans ce test est l’outil ctsTraffic. Le référentiel Git pour cet outil se trouve dans [ https://github.com/Microsoft/ctsTraffic ](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Performances de la passerelle RAS

Les illustrations de cette section décrivent les affichages de Gestionnaire des tâches de débit de tunnel GRE avec plusieurs connexions TCP.

Vous pouvez obtenir jusqu'à 2.0 débit Gbits/s sur plusieurs\-core des machines virtuelles qui sont configurées en tant que passerelles de RAS GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Performances de Tunnel GRE avec plusieurs Sessions TCP

Avec plusieurs sessions TCP, l’utilisation du processeur atteint 100 % et le débit maximal sur le tunnel GRE est 2.0 Gbits/s.

L’illustration suivante représente l’utilisation du processeur sur les deux machines virtuelles de la passerelle RAS. La machine virtuelle active, RAS passerelle VM #1, est sur la gauche, tandis que la machine virtuelle passive, passerelle RAS machine virtuelle 2, se trouve à droite.

![Utilisation du processeur de la machine virtuelle de passerelle dans le Gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

L’illustration suivante représente le débit du réseau Ethernet sur les ordinateurs virtuels de passerelle RAS. La machine virtuelle active, RAS passerelle VM #1, est sur la gauche, tandis que la machine virtuelle passive, passerelle RAS machine virtuelle 2, se trouve à droite.

![Débit de réseau Ethernet de machine virtuelle de passerelle dans le Gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Performances de Tunnel GRE avec une connexion TCP

Avec la configuration de test a été remplacée par plusieurs sessions TCP à une seule session TCP, qu’un seul cœur de processeur atteint la capacité maximale sur les ordinateurs virtuels de passerelle RAS.

Le débit maximal sur le tunnel GRE est entre 400-500 Mbits/s.

L’illustration suivante représente l’utilisation du processeur sur les deux machines virtuelles de la passerelle RAS. La machine virtuelle active, RAS passerelle VM #1, est sur la gauche, tandis que la machine virtuelle passive, passerelle RAS machine virtuelle 2, se trouve à droite.

![Utilisation du processeur de la machine virtuelle de passerelle dans le Gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


L’illustration suivante représente le débit du réseau Ethernet sur les ordinateurs virtuels de passerelle RAS. La machine virtuelle active, RAS passerelle VM #1, est sur la gauche, tandis que la machine virtuelle passive, passerelle RAS machine virtuelle 2, se trouve à droite.

![Débit de réseau Ethernet de machine virtuelle de passerelle dans le Gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Pour plus d’informations sur les performances de la passerelle RAS, consultez [réglage des performances HNV passerelle dans les réseaux à définition logicielle](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).