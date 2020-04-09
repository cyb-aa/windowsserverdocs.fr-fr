---
title: Débit et performances du Tunnel GRE de la passerelle RAS
description: Cette rubrique, qui est destinée aux professionnels de l’informatique, fournit des informations sur les performances de débit des tunnels GRE (Generic Routing Encapsulation) de la passerelle RAS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3ac381270714ecebc0aa624152700155fe170400
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814462"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>Débit et performances du Tunnel GRE de la passerelle RAS

>S’applique à : canal semi-annuel Windows Server \(\)

Vous pouvez utiliser cette rubrique pour en savoir plus sur le serveur d’accès à distance \(l’encapsulation générique de routage\) de passerelle RAS \(les performances de tunnel GRE\) sur Windows Server, version 1709, dans un environnement de test de mise en réseau non défini par logiciel \(SDN\).

La passerelle RAS est un routeur logiciel et une passerelle que vous pouvez utiliser en mode à un seul locataire ou en mode multi-locataire. Cette rubrique traite d’un mode de locataire unique, de la configuration de haute disponibilité avec le clustering de basculement. Les statistiques de performances de tunnel GRE présentées dans cette rubrique sont valides pour la passerelle RAS dans les modes client singele et multi-locataire.

>[!NOTE]
>Le clustering de basculement est une fonctionnalité de Windows Server qui vous permet de regrouper plusieurs serveurs dans un cluster à tolérance de panne. Pour plus d’informations, consultez [clustering de basculement](../../../failover-clustering/failover-clustering-overview.md)

Le mode mono-client permet aux organisations de toutes tailles de déployer la passerelle comme un réseau privé virtuel \(\) serveur VPN de réseau privé virtuel ou\-Internet. En mode mono-client, vous pouvez déployer la passerelle RAS sur un serveur physique ou une machine virtuelle \(\)de machine virtuelle. Cette rubrique décrit le déploiement d’une passerelle RAS sur deux machines virtuelles \(les machines virtuelles\) configurées dans un cluster de basculement.

>[!IMPORTANT]
>Étant donné que les tunnels GRE fournissent l’encapsulation mais pas le chiffrement, vous ne devez pas utiliser la passerelle RAS configurée avec GRE en tant que passerelle de périphérie d’Internet. Pour en savoir plus sur les meilleures utilisations de la passerelle RAS avec les tunnels GRE, voir [tunneling GRE dans Windows Server](gre-tunneling-windows-server.md).

GRE est un protocole de tunneling léger qui peut encapsuler une grande variété de protocoles de couche réseau à l’intérieur de points virtuels\-pour\-des liens de point sur un réseau d’interconnexion de protocole Internet. L’implémentation Microsoft GRE encapsule IPv4 et IPv6.

Pour plus d’informations, consultez la section scénarios de déploiement de la **passerelle RAS** dans la rubrique [passerelle RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy). 

Dans ce scénario de test, qui est représenté dans l’illustration suivante, le flux de trafic mesuré passe de l’intranet de l’Organisation 2 à l’intranet 1 de l’organisation. Les machines virtuelles de charge de travail client envoient le trafic réseau de l’intranet 2 vers l’intranet 1 en utilisant la passerelle RAS.

![Vue d’ensemble du scénario de test de tunnel GRE de la passerelle RAS](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>Configuration de l’environnement de test

Cette section fournit des informations sur l’environnement de test et la configuration de la passerelle RAS.

Dans l’environnement de test, les machines virtuelles de la passerelle RAS sont déployées sur les ordinateurs hôtes Hyper\-V dans un cluster de basculement pour une haute disponibilité.

### <a name="hyper-v-host-configuration"></a>Configuration de l’hôte Hyper\-V

Deux hôtes Hyper\-V sont configurés pour prendre en charge le scénario de test de la manière suivante. 

- Deux ordinateurs physiques à double\-hébergés sont configurés avec Windows Server, version 1709
- Les deux cartes réseau physiques de chacun des deux serveurs sont connectées à différents sous-réseaux, qui représentent tous les deux des sous-réseaux d’un intranet d’entreprise. Les réseaux et le matériel de prise en charge ont une capacité de 10 Gbits/s.
- L’hyperthreading sur les serveurs physiques est désactivé. Cela fournit le débit maximal des cartes réseau physiques.
- Le rôle serveur Hyper\-V est installé sur les deux serveurs et configuré avec deux commutateurs virtuels Hyper\-V externes, un pour chaque carte réseau physique.
- Étant donné que les deux serveurs sont connectés au même intranet, les serveurs peuvent communiquer entre eux.
- Les ordinateurs hôtes Hyper\-V sont configurés dans un cluster de basculement sur le réseau intranet. 

>[!NOTE]
>Pour plus d’informations, voir [Commutateur virtuel Hyper\-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch).

### <a name="vm-configuration"></a>Configuration de la machine virtuelle

Deux machines virtuelles sont configurées pour prendre en charge le scénario de test de la manière suivante.

- Sur chaque serveur, une machine virtuelle exécutant Windows Server version 1709 est installée. Chaque machine virtuelle est configurée avec 10 cœurs et 8 Go de RAM.
- Chaque machine virtuelle est également configurée avec deux cartes réseau virtuelles. Une carte réseau virtuelle est connectée au commutateur virtuel intranet 1, et l’autre carte réseau virtuelle est connectée au commutateur virtuel intranet 2.
- Une passerelle RAS est installée et configurée sur chaque machine virtuelle en tant que serveur VPN\-GRE.
- Les machines virtuelles de passerelle sont configurées dans un cluster de basculement. En cas de cluster, une machine virtuelle est active et l’autre est passive.

### <a name="workload-hyper-v-hosts-and-vms"></a>Ordinateurs hôtes Hyper\-V et machines virtuelles de charge de travail

Pour ce test, deux ordinateurs hôtes Hyper\-V de charge de travail sont installés sur l’intranet, et chaque ordinateur hôte dispose d’une machine virtuelle installée. Si vous dupliquez ce test dans votre propre environnement de test, vous pouvez installer autant de serveurs de charge de travail et de machines virtuelles que nécessaire à vos besoins.

- Les ordinateurs hôtes Hyper\-V de charge de travail ont une carte réseau physique installée qui est connectée à l’intranet de l’organisation.
- Dans le commutateur virtuel Hyper\-V, un commutateur virtuel est créé sur chaque ordinateur hôte. Le commutateur est externe et est lié à l’une des cartes réseau connectées à l’intranet.
- Les machines virtuelles de charge de travail sont configurées avec 2 Go de RAM et 2 cœurs.
- Les machines virtuelles de charge de travail disposent chacune d’une carte réseau virtuelle connectée au commutateur virtuel intranet.

### <a name="traffic-generator-tool"></a>Outil Traffic Generator

L’outil Traffic Generator utilisé dans ce test est l’outil ctsTraffic. Le référentiel git pour cet outil se trouve dans [https://github.com/Microsoft/ctsTraffic](https://github.com/Microsoft/ctsTraffic).

## <a name="ras-gateway-performance"></a>Performances de la passerelle RAS

Les illustrations de cette section décrivent le gestionnaire des tâches pour afficher le débit de tunnel GRE avec plusieurs connexions TCP.

Vous pouvez atteindre jusqu’à 2,0 Gbits/s de débit sur les machines virtuelles à plusieurs\-cœurs configurées en tant que passerelles RAS GRE.

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>Performances du tunnel GRE avec plusieurs sessions TCP

Avec plusieurs sessions TCP, l’utilisation du processeur atteint 100% et le débit maximal sur le tunnel GRE est de 2,0 Gbits/s.

L’illustration suivante représente l’utilisation du processeur sur les deux machines virtuelles de la passerelle RAS. La machine virtuelle active de la passerelle RAS #1 est sur la gauche, tandis que la machine virtuelle passive de la passerelle RAS #2 se trouve à droite.

![Utilisation du processeur de la machine virtuelle de la passerelle dans le gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

L’illustration suivante représente le débit du réseau Ethernet sur les machines virtuelles de la passerelle RAS. La machine virtuelle active de la passerelle RAS #1 est sur la gauche, tandis que la machine virtuelle passive de la passerelle RAS #2 se trouve à droite.

![Débit réseau Ethernet de la passerelle de la passerelle dans le gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>Performances du tunnel GRE avec une connexion TCP

Une fois que la configuration de test est passée de plusieurs sessions TCP à une session TCP unique, un seul cœur de processeur atteint la capacité maximale sur les machines virtuelles de la passerelle RAS.

Le débit maximal sur le tunnel GRE est compris entre 400-500 Mbits/s.

L’illustration suivante représente l’utilisation du processeur sur les deux machines virtuelles de la passerelle RAS. La machine virtuelle active de la passerelle RAS #1 est sur la gauche, tandis que la machine virtuelle passive de la passerelle RAS #2 se trouve à droite.

![Utilisation du processeur de la machine virtuelle de la passerelle dans le gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


L’illustration suivante représente le débit du réseau Ethernet sur les machines virtuelles de la passerelle RAS. La machine virtuelle active de la passerelle RAS #1 est sur la gauche, tandis que la machine virtuelle passive de la passerelle RAS #2 se trouve à droite.

![Débit réseau Ethernet de la passerelle de la passerelle dans le gestionnaire des tâches](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

Pour plus d’informations sur les performances de la passerelle RAS, consultez [réglage des performances de la passerelle HNV dans réseaux à définition logicielle](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance).