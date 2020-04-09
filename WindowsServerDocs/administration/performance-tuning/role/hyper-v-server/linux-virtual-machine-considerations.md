---
title: Considérations sur les machines virtuelles Linux
description: Machine virtuelle Linux et BSD
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7abc1ef5473365dd26dce1167bb685f116822a7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851742"
---
# <a name="linux-virtual-machine-considerations"></a>Considérations sur les machines virtuelles Linux

Les machines virtuelles Linux et BSD ont des considérations supplémentaires par rapport aux machines virtuelles Windows dans Hyper-V.

La première considération est de savoir si Integration Services sont présents ou si la machine virtuelle s’exécute uniquement sur du matériel émulé sans aucune intervention. Un tableau des versions de Linux et BSD qui ont des Integration Services intégrées ou téléchargeables est disponible dans [les machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows). Ces pages offrent des grilles de fonctionnalités Hyper-V disponibles pour les versions de distribution Linux et des notes sur ces fonctionnalités, le cas échéant.

Même lorsque l’invité exécute Integration Services, il peut être configuré avec un matériel hérité qui n’offre pas les meilleures performances. Par exemple, configurez et utilisez une carte Ethernet virtuelle pour l’invité au lieu d’utiliser une carte réseau héritée. Avec Windows Server 2016, les réseaux avancés tels que SR-IOV sont également disponibles.

## <a name="linux-network-performance"></a>Performances du réseau Linux

Linux par défaut active l’accélération matérielle et les déchargements par défaut. Si l’option vRSS est activée dans les propriétés d’une carte réseau sur l’ordinateur hôte et que l’invité Linux a la possibilité d’utiliser vRSS, la fonctionnalité est activée. Dans PowerShell, ce même paramètre peut être modifié à l’aide de la commande `EnableNetAdapterRSS`.

De même, la fonctionnalité VMMQ (RSS du commutateur virtuel) peut être activée sur la carte réseau physique utilisée par les **Propriétés** invité > **configurer...**  > onglet **avancé** > définir le format **RSS du commutateur virtuel** sur **activé** ou activer VMMQ dans PowerShell à l’aide des éléments suivants :

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

Dans l’invité, le réglage TCP supplémentaire peut être effectué en renforçant les limites. Pour une plus grande charge de travail d’étalement des performances sur plusieurs UC et avec des charges de travail profondes, les charges de travail virtualisées présentent une latence supérieure à celle des « systèmes nus ».

Voici quelques exemples de paramètres de paramétrage qui ont été utiles dans les tests d’évaluation réseau :

```PowerShell
net.core.netdev_max_backlog = 30000
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.ipv4.tcp_wmem = 4096 12582912 33554432
net.ipv4.tcp_rmem = 4096 12582912 33554432
net.ipv4.tcp_max_syn_backlog = 80960
net.ipv4.tcp_slow_start_after_idle = 0
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10240 65535
net.ipv4.tcp_abort_on_overflow = 1
```

Ntttcp, disponible à la fois sur Linux et Windows, est un outil utile pour les microbancs d’essai réseau. La version Linux est open source et disponible à partir de [ntttcp-for-Linux sur GitHub.com](https://github.com/Microsoft/ntttcp-for-linux). La version de Windows se trouve dans le [Centre de téléchargement](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Lors du paramétrage des charges de travail, il est préférable d’utiliser autant de flux que nécessaire pour obtenir le meilleur débit. En utilisant ntttcp pour modéliser le trafic, le paramètre `-P` définit le nombre de connexions parallèles utilisées.

## <a name="linux-storage-performance"></a>Performances de stockage Linux

Certaines meilleures pratiques, comme les suivantes, sont répertoriées dans [meilleures pratiques pour l’exécution de Linux sur Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v). Le noyau Linux a des planificateurs d’e/s différents pour réorganiser les demandes avec des algorithmes différents. NOOP est une file d’attente de premier plan qui transmet la décision de planification à effectuer par l’hyperviseur. Il est recommandé d’utiliser NOOP comme planificateur lors de l’exécution de la machine virtuelle Linux sur Hyper-V. Pour modifier le planificateur d’un appareil spécifique, dans la configuration du chargeur de démarrage (/etc/grub.conf, par exemple), ajoutez `elevator=noop` aux paramètres du noyau, puis redémarrez.

À l’instar de la mise en réseau, les performances des invités Linux avec stockage bénéficient le plus de plusieurs files d’attente avec suffisamment de profondeur pour maintenir l’hôte occupé. Les performances de stockage de microtest sont probablement meilleures avec l’outil de benchmark fio avec le moteur libaio.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)
