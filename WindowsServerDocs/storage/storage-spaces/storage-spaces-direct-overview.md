---
title: Présentation des espaces de stockage direct
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Vue d’ensemble des espaces de stockage Direct, une fonctionnalité de Windows Server qui vous permet de serveurs en cluster avec un stockage interne dans une solution de stockage à définition logicielle.
ms.localizationpriority: medium
ms.openlocfilehash: d71e4fc404f760102a2b22f71bbeec680868e9b8
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262782"
---
# Présentation des espaces de stockage direct

>S’applique à: Windows Server 2019, Windows Server 2016

Les espaces de stockage direct tirent parti de serveurs standard utilisant des disques locaux pour créer un stockage à définition logicielle hautement disponible et évolutif, pour un coût nettement inférieur à celui des baies SAN ou NAS traditionnelles. Son architecture convergé voire hyperconvergée simplifie considérablement la configuration et le déploiement, et des fonctionnalités telles que la mise en cache, les niveaux de stockage et codage d’effacement, conjointement avec les dernières innovations matérielles telles que la mise en réseau RDMA et les lecteurs NVMe, offrent efficacité sans égal et les performances.

Espaces de stockage Direct est inclus dans Windows Server 2019 Datacenter, Windows Server 2016 Datacenter et [Builds de Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Pour d’autres applications des espaces de stockage, tels que des clusters SAS partagé et les serveurs autonomes, consultez la [vue d’ensemble des espaces de stockage](overview.md). Si vous recherchez des informations sur l’utilisation des espaces de stockage sur un PC Windows 10, consultez [Les espaces de stockage dans Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Comprendre</a></strong>
            <ul>
              <li>Vue d’ensemble (vous vous trouvez ici)</li>
              <li><a href="understand-the-cache.md">Fonctionnement du cache</a></li>
              <li><a href="storage-spaces-fault-tolerance.md">Tolérance de panne et efficacité du stockage</a></li>
              <li><a href="drive-symmetry-considerations.md">Considérations relatives à la symétrie des lecteurs</a></li>
              <li><a href="understand-storage-resync.md">Comprendre et contrôler la resynchronisation du stockage</a></li>
              <li><a href="understand-quorum.md">Quorum de cluster et le pool de présentation</a></li>
              <li><a href="cluster-sets.md">Jeux de clusters</a></li>
            </ul>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Planifier</a></strong>
            <ul>
              <li><a href="storage-spaces-direct-hardware-requirements.md">Configuration matérielle requise</a></li>
              <li><a href="csv-cache.md">Utilisation du cache de lecture en mémoire pour le volume partagé de cluster</li>
              <li><a href="choosing-drives.md">Choisissez les lecteurs</a></li>
              <li><a href="plan-volumes.md">Planifier les volumes</a></li>
              <li><a href="storage-spaces-direct-in-vm.md">Utilisation des clusters de machines virtuelles invités</a></li>
              <li><a href="storage-spaces-direct-disaster-recovery.md">Récupération d'urgence</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Déployer</a></strong>
            <ul>
                    <li><a href="deploy-storage-spaces-direct.md">Déployer des espaces de stockage direct</a></li>
                    <li><a href="create-volumes.md">Créer des volumes</a></li>
              <li><a href="nested-resiliency.md">Résilience imbriquée</a></li>
              <li><a href="../../failover-clustering/manage-cluster-quorum.md">Configurer le quorum</a></li>
              <li><a href="upgrade-storage-spaces-direct-to-windows-server-2019.md">Mettre à niveau un cluster direct d’espaces de stockage vers Windows Server2019</a></li>
            </ul>
        </td>        
        <td style="padding: 5px; border: 0;">
            <strong>Gérer</a></strong>
            <ul>
              <li><a href="../../manage/windows-admin-center/use/manage-hyper-converged.md">Gérer avec Windows Admin Center</a></li>
              <li><a href="add-nodes.md">Ajouter des serveurs ou des lecteurs</a></li>
              <li><a href="maintain-servers.md">Mise d'un serveur hors connexion pour la maintenance</li>
              <li><a href="remove-servers.md">Supprimer des serveurs</a></li>
              <li><a href="resize-volumes.md">Étendre les volumes</a></li>
              <li><a href="../update-firmware.md">Mettre à jour le microprogramme des disques</a></li>
              <li><a href="performance-history.md">Historique des performances</a></li>
              <li><a href="delimit-volume-allocation.md">Délimiter l'allocation des volumes</a></li>
              <li><a href="configure-azure-monitor.md">Utilisez le moniteur Azure sur un cluster hyperconvergé</a></li>
            </ul>
        </td>
    </tr>
    <tr style="border: 0;">
         <td style="padding: 5px; border: 0;">
            <strong>Résolution des problèmes</a></strong>
            <ul>
              <li><a href="storage-spaces-states.md">Résoudre les problèmes liés à l’intégrité et les états opérationnels</a></li>
              <li><a href="data-collection.md">Collecter des données de diagnostics avec des espaces de stockage Direct</a></li>
            </ul>
         <td style="padding: 5px; border: 0;">
            <strong>Billets de blog récents</a></strong>
            <ul>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/">13,7 millions/s par seconde avec des espaces de stockage Direct: le nouvel enregistrement au secteur d’activité d’une infrastructure hyperconvergée</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/">Infrastructure hyperconvergée dans Windows Server 2019 - l’horloge de compte à rebours démarre à présent!</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/">Annonces big cinq à partir de Windows Server sommet</a></li>
              <li><a href="https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/">10 000 clusters espaces de stockage Direct et prendre en compte les …</a></li>
            </ul>
</table>

## Vidéos

**Présentation vidéo rapide (5 minutes)**

<iframe src="https://www.youtube-nocookie.com/embed/raeUiNtMk0E" width="560" height="315" allowfullscreen></iframe>

**Espaces de stockage Direct au Microsoft Ignite 2018 (1 heure)**

[Regardez sur YouTube](https://www.youtube.com/watch?v=5kaUiW3qo30)

**Espaces de stockage Direct au Microsoft Ignite 2017 (1 heure)**

[Regardez sur YouTube](https://www.youtube.com/watch?v=YDr2sqNB-3c)

**Lancer l’événement au Microsoft Ignite 2016 (1 heure)**

[Regardez sur YouTube](https://www.youtube.com/watch?v=-LK2ViRGbWs)

## Principaux avantages

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Simplicité.</b> Créez votre premier cluster d'espaces de stockage direct en moins de 15minutes à partir de vos serveurs standard exécutant WindowsServer2016. Pour les utilisateurs de System Center, le déploiement se résume à cocher une case.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/performance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Performances sans égal.</b> Qu’ils soient 100% flash ou hybrides, les espaces de stockage direct dépassent facilement <a href="https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/">150000E/S mixtes 4K aléatoires par seconde sur chaque serveur</a> avec une latence faible et homogène du fait de leur architecture avec hyperviseur incorporé, de leur cache de lecture/écriture intégré et de la prise en charge de lecteursNVMe de pointe montés directement sur le bus PCIe.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Tolérance de panne. </b> La résilience intégrée gère les défaillances des lecteurs, des serveurs ou des composants en assurant une disponibilité permanente. Des déploiements de plus grande envergure sont également configurables pour assurer une <a href="../../failover-clustering/fault-domains.md">tolérance de pannes des châssis et des racks</a>. Quand du matériel tombe en panne, il suffit de le remplacer. Les logiciels se réparent par eux-mêmes sans étapes d’administration fastidieuses.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Efficacité des ressources.</b> Le codage de l’effacement offre un stockage jusqu’à 2,4fois plus efficace grâce à des innovations uniques comme les codes de reconstruction locale et la hiérarchisation en temps réel ReFS. Les gains profitent ainsi aux lecteurs de disque dur et aux charges de travail mixtes à chaud/à froid, tout en limitant l’utilisation du processeur pour réaffecter les ressources là où elles sont le plus nécessaire: au niveau des machines virtuelles.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Facilité de gestion.</b> Les <a href="../storage-qos/storage-qos-overview.md">contrôles de qualité de service de stockage</a> permettent de garder les machines virtuelles fortement sollicitées dans des limites d’E/S par seconde minimales et maximales par machines virtuelles. Le <a href="../../failover-clustering/health-service-overview.md">service de contrôle d’intégrité</a> intègre des fonctionnalités de surveillance et d’alerte en continu, et les nouvelles API facilitent la collecte de métriques de performances et de capacité d’une grande richesse à l’échelle du cluster.
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:100px">
            <img src="media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png" width="100" alt="">
        </td>
        <td style="padding: 10px; border: 0;">
            <b>Extensibilité.</b> Avec une capacité maximale de 16serveurs et de plus de 400lecteurs, avec jusqu’à 1pétaoctet (1000 téraoctets) de stockage par cluster. Pour une montée en charge, il suffit d’ajouter des lecteurs ou des serveurs supplémentaires; les espaces de stockage direct intègrent automatiquement les nouveaux lecteurs et les utilisent aussitôt. L’efficacité et les performances du stockage s’améliorent de façon prévisible en fonction des besoins.
        </td>
    </tr>
</table>

## Options de déploiement

Les espaces de stockage direct ont été conçus pour deuxtypes de déploiement:

### Déploiement convergé

**Stockage et calcul dans des clusters séparés.** L’option de déploiement convergé (ou «désagrégé») dispose au-dessus des espaces de stockage direct une couche constituée d’un serveur de fichiers avec montée en puissance parallèle (SoFS) pour offrir un stockageNAS sur des partages de fichiersSMB3. Cela permet une mise à l’échelle des capacités de calcul et/ou des charges de travail indépendamment du cluster de stockage, ce qui est essentiel pour les déploiements à grande échelle comme les infrastructures IaaS (Infrastructure as a Service) Hyper-V des fournisseurs de services et des grandes entreprises.

![Les espaces de stockage direct offrent du stockage à l’aide de la fonctionnalité de serveur de fichiers avec montée en puissance parallèle pour les ordinateurs virtuels Hyper-V dans un autre serveur ou cluster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### Déploiements hyperconvergés

**Un cluster de calcul et de stockage.** Le déploiement hyperconvergé exécute les machines virtuelles Hyper-V ou les bases de données SQLServer directement sur les serveurs de stockage en stockant leurs fichiers sur les volumes locaux. Il n’est donc plus nécessaire de configurer l’accès aux serveurs de fichiers ni les autorisations. Les coûts en matériel des déploiements dans les PME ou les succursales/bureaux distants s’en trouvent réduits. Voir [déployer des espaces de stockage Direct](deploy-storage-spaces-direct.md).

![Les espaces de stockage direct offrent du stockage aux ordinateurs virtuels Hyper-V dans le même cluster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## Fonctionnement

Les espaces de stockage direct sont une évolution des espaces de stockage inaugurés par Windows Server2012. Ils tirent parti des nombreuses fonctionnalités de Windows Server que vous connaissez déjà, telles que le clustering de basculement, le système de fichiers de volume partagé de cluster (CSV), le protocole SMB3 (Server Message Block) et bien entendu les espaces de stockage. De même, ils introduisent de nouvelles technologies, plus particulièrement le Software Storage Bus.

Voici une vue d’ensemble de la pile des espaces de stockage direct:

![Pile des espaces de stockage direct](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Matériel de mise réseau.** Les espaces de stockage direct utilisent SMB3, dont SMBDirect et SMBMultichannel, via une connexion Ethernet pour assurer la communication entre les serveurs. Nous recommandons fortement le 10+GbE avec un accès direct à la mémoire à distance (RDMA), iWARP ou RoCE.

**Matériel de stockage.** De 2 à 16serveurs équipés de disques SATA, SAS ou NVMe locaux. Chaque serveur doit disposer d’au moins 2lecteurs SSD et au moins 4lecteurs supplémentaires. Les appareils SATA et SAS doivent se trouver derrière un adaptateur de bus hôte/carte de bus hôte (HBA) et un expandeur SAS. Nous recommandons fortement les plateformes conçues avec soin et largement validées de nos partenaires (à venir).

**Clustering de basculement.** Les fonctionnalités de clustering intégrées à Windows Server servent à connecter les serveurs.

**Software Storage Bus.** Le Software Storage Bus est une nouveauté des espaces de stockage direct. Il s’étend sur le cluster et établit une infrastructure de stockage à définition logicielle dans laquelle tous les serveurs peuvent voir les lecteurs locaux des uns des autres. Vous pouvez l’envisager comme une solution de remplacement au câblage coûteux et restrictif des solutions Fibre Channel ou SAS partagé.

**Cache au niveau du bus de stockage.** Le Software Storage Bus lie dynamiquement les lecteurs les plus rapides (p.ex., les SSD) aux lecteurs les plus lents (p.ex., les disques durs classiques) pour assurer une mise en cache en lecture/écriture côté serveur qui accélère les E/S et dope le débit.

**Pool de stockage.** L’ensemble de lecteurs qui constitue la base des espaces de stockage s’appelle «pool de stockage». Il est créé automatiquement et tous les lecteurs éligibles sont automatiquement découverts et ajoutés à cet ensemble. Nous vous recommandons vivement d’utiliser un pool par cluster avec les paramètres par défaut. Lisez notre message de blog [Deep Dive into the Storage Pool](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) (Étude approfondie des pools de stockage) pour en savoir plus.

**Espaces de stockage.** Les espaces de stockage apportent une tolérance de panne aux «disques» virtuels grâce à [la mise en miroir, au codage d’effacement ou aux deux à la fois](storage-spaces-fault-tolerance.md). Vous pouvez les considérer comme une technologie RAID distribuée, définie par voie logicielle et utilisant les disques du pool. Dans les espaces de stockage direct, ces disques virtuels assurent généralement une résilience pour deux défaillances simultanées de disque ou de serveur (p.ex., mise en miroir à 3voies, avec chaque copie de données sur un serveur différent), même si une tolérance de panne au niveau des châssis et des racks est également disponible.

**Resilient File System (ReFS).** ReFS est le premier système de fichiers conçu spécialement pour la virtualisation. Il permet notamment d’accélérer considérablement les opérations sur les fichiers .vhdx, telles que la création, l’extension et la fusion de points de contrôle. Les sommes de contrôle intégrées permettent quant à elles de détecter et corriger les erreurs de bit. Par ailleurs, il introduit des niveaux en temps réel qui assurent la rotation des données entre les niveaux de stockage dits «à chaud» et «à froid» en temps réel en fonction de l’utilisation.

**Volumes partagés de cluster.** Le système de fichiers CSV unifie tous les volumes ReFS dans un même espace de noms accessible à n’importe quel serveur, si bien que chaque serveur et chaque volume se présente et agit comme s’il était monté localement.

**Serveur de fichiers avec montée en puissance parallèle.** Cette ultime couche n’est nécessaire que dans les déploiements convergés. À la faveur du protocole d’accès SMB3, les clients (p.ex., un autre cluster exécutant Hyper-V) peuvent accéder aux fichiers distants via le réseau, transformant ainsi efficacement vos espaces de stockage direct en un stockage NAS (Network Attached Storage).

## Expériences clients

Il existe [plus de 10 000 clusters](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) des espaces de stockage en cours d’exécution dans le monde entier Direct. Les organisations de toutes tailles, des petites entreprises déploiement seulement deux nœuds, pour les grandes entreprises et les gouvernements déployez des centaines de nœuds, dépendent espaces de stockage Direct pour leurs applications critiques et de l’infrastructure.

Visitez [Microsoft.com/HCI](https://www.microsoft.com/hci) pour lire leurs articles:

[![Gsupprimer des logos client](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## Outils de gestion

Les outils suivants peuvent être utilisés pour gérer et/ou de surveiller les espaces de stockage Direct:

| Nom | Graphique ou de ligne de commande? | Payé ou inclus? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Graphique    | Inclus |
| Gestionnaire du Cluster de basculement & le Gestionnaire de serveur                                 | Graphique    | Inclus |
| WindowsPowerShell                                                        | En ligne de commande | Inclus |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) & [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Graphique    | Paid     |

## Prise en main

Essayez les espaces de stockage direct [dans MicrosoftAzure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/), ou téléchargez une version d’évaluation avec licence de 180jours de WindowsServer sur la page [Windows Server-Évaluations](https://go.microsoft.com/fwlink/?linkid=842602).

## Voir également

-   [Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)
-   [Réplica de stockage](../storage-replica/storage-replica-overview.md)
-   [Stockage blog Microsoft](https://blogs.technet.microsoft.com/filecab/)
-   [Débit d’espaces de stockage direct avec iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (blog TechNet)
-   [Nouveautés du clustering de basculement dans Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
-   [Qualité de service de stockage](../storage-qos/storage-qos-overview.md)
-   [Support Windows IT Pro](https://www.microsoft.com/itpro/windows/support)
