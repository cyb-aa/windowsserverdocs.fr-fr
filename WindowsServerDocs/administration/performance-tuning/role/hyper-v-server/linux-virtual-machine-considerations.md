---
title: Considérations relatives à la Machine virtuelle Linux
description: Machine virtuelle Linux et BSD
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cc6aab7825754579269eb05e591ca2a3cf5a561b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869680"
---
# <a name="linux-virtual-machine-considerations"></a>Considérations relatives à la Machine virtuelle Linux

BSD machines virtuelles Linux et avoir des considérations supplémentaires par rapport aux machines virtuelles de Windows dans Hyper-V.

Le premier point important est si les Services d’intégration sont présentes ou si la machine virtuelle est en cours d’exécution simplement sur le matériel émulé avec aucun révération. Une table des versions de Linux et BSD ayant intégré ou téléchargeable Integration Services est disponible dans [pris en charge de Linux et FreeBSD machines virtuelles pour Hyper-V sur Windows](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows). Ces pages comportent des grilles de fonctionnalités Hyper-V disponibles disponibles pour les versions de distribution de Linux et des remarques sur ces fonctionnalités, le cas échéant.

Même si l’invité est en cours d’exécution Integration Services, il peut être configuré avec le matériel hérité qui ne présente pas les meilleures performances. Par exemple, configurer et utiliser une carte ethernet virtuelle pour l’invité au lieu d’utiliser une carte réseau héritée. Avec Windows Server 2016, mise en réseau avancés tels que SR-IOV sont également disponibles.

## <a name="linux-network-performance"></a>Performances réseau Linux

Linux par défaut active l’accélération matérielle et les déchargements par défaut. Si vRSS est activé dans les propriétés d’une carte réseau sur l’ordinateur hôte et l’invité Linux a la possibilité d’utiliser vRSS la fonctionnalité est activée. Dans Powershell, ce même paramètre peut être modifié avec le `EnableNetAdapterRSS` commande.

De même, la fonctionnalité VMMQ (RSS de commutateur virtuel) peut être activée sur la carte réseau physique utilisée par l’invité **propriétés** > **configurer...**   >  **Avancé** onglet > définir **RSS de commutateur virtuel** à **activé** ou activer VMMQ dans Powershell à l’aide de ce qui suit :

```PowerShell
 Set-VMNetworkAdapter -VMName **$VMName** -VmmqEnabled $True
 ```

Dans l’invité un paramétrage supplémentaire TCP peut être effectué par l’augmentation des limites. Pour des performances optimales répartissant la charge de travail sur plusieurs processeurs et des charges de travail approfondie génère le meilleur débit, comme charges de travail virtualisées aura une latence plus élevée que « système nu » celles.

Certains paramètres de réglage d’exemple qui ont été utiles dans les tests d’évaluation réseau incluent :

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

Un outil utile pour le réseau microbenchmarks est ntttcp, qui est disponible sur Linux et Windows. La version de Linux est open source et disponibles à partir de [ntttcp-for-linux sur github.com](https://github.com/Microsoft/ntttcp-for-linux). Vous trouverez la version de Windows dans le [centre de téléchargement](https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769). Lors du paramétrage des charges de travail, il est préférable d’utiliser des flux autant que nécessaire pour obtenir le meilleur débit. À l’aide de ntttcp pour le trafic de modèle, le `-P` paramètre définit le nombre de connexions parallèles utilisées.

## <a name="linux-storage-performance"></a>Performances de stockage Linux

Certaines meilleures pratiques, comme suit, sont répertoriés sur [meilleures pratiques pour l’exécution de Linux sur Hyper-V](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/best-practices-for-running-linux-on-hyper-v). Le noyau Linux a différents planificateurs d’e/s pour réorganiser les demandes avec différents algorithmes. NOOP est une file d’attente premier sorti qui transmet la décision de planification d’être effectuées par l’hyperviseur. Il est recommandé d’utiliser NOOP comme planificateur lors de l’exécution de machine virtuelle Linux sur Hyper-V. Modifier le planificateur pour un périphérique spécifique, dans la configuration du chargeur de démarrage (/ etc/grub.conf, par exemple), ajoutez `elevator=noop` et les paramètres de noyau, puis redémarrez.

Comme pour la mise en réseau, performances de l’invité Linux avec stockage bénéficie le meilleur parti de plusieurs files d’attente avec suffisamment approfondi pour occuper l’hôte. Performances de stockage Microbenchmarking sont sans doute préférable avec l’outil de test d’évaluation fio avec le moteur libaio.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances du processeur de Hyper-V](processor-performance.md)

-   [Performances de la mémoire Hyper-V](memory-performance.md)

-   [Stockage Hyper-V les performances d’e/s](storage-io-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)
