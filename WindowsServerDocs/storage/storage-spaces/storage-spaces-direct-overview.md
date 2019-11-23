---
title: Vue d’ensemble des espaces de stockage direct
ms.prod: windows-server
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/26/2019
ms.assetid: 8bd0d09a-0421-40a4-b752-40ecb5350ffd
description: Vue d’ensemble de espaces de stockage direct, une fonctionnalité de Windows Server qui vous permet de Clusterer des serveurs avec un stockage interne dans une solution de stockage définie par logiciel.
ms.localizationpriority: medium
ms.openlocfilehash: 4e47adcebf7da87e9d3c96812f5d7d90ca00601b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402840"
---
# <a name="storage-spaces-direct-overview"></a>Vue d’ensemble des espaces de stockage direct

>S’applique à : Windows Server 2019, Windows Server 2016

Les espaces de stockage direct s’appuient sur des serveurs standard équipés de lecteurs locaux pour créer un stockage défini par logiciel hautement disponible et évolutif pour un coût nettement inférieur aux baies SAN ou NAS traditionnelles. Son architecture convergée ou hyper-convergée simplifie radicalement l’approvisionnement et le déploiement, alors que des fonctionnalités telles que la mise en cache, les niveaux de stockage et le codage d’effacement, ainsi que les dernières innovations matérielles telles que la mise en réseau RDMA et les lecteurs NVMe, offrent efficacité et performances inégalées.

Espaces de stockage direct est inclus dans les builds Windows Server 2019 Datacenter, Windows Server 2016 datacenter et [Windows Server Insider Preview](https://insider.windows.com/for-business-getting-started-server/). 

Pour d’autres applications d’espaces de stockage, telles que les clusters SAS partagés et les serveurs autonomes, consultez [vue d’ensemble des espaces de stockage](overview.md). Si vous recherchez des informations sur l’utilisation des espaces de stockage sur un PC Windows 10, consultez [espaces de stockage dans Windows 10](https://support.microsoft.com/help/12438/windows-10-storage-spaces).

|       |       |
|   -   |   -   |
| **Comprendre**<br><ul><li>Vue d’ensemble (vous vous trouvez ici)</li><li>[Fonctionnement du cache](understand-the-cache.md)</li><li>[Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)<li>[Considérations relatives à la symétrie des lecteurs](drive-symmetry-considerations.md)</li><li>[Comprendre et contrôler la resynchronisation du stockage](understand-storage-resync.md)</li><li>[Présentation du quorum de cluster et du pool](understand-quorum.md)</li><li>[Jeux de clusters](cluster-sets.md)</li> | **Planification**<br><ul><li>[Configuration matérielle requise](storage-spaces-direct-hardware-requirements.md)</li><li>[Utilisation du cache de lecture en mémoire pour le volume partagé de cluster](csv-cache.md)</li><li>[Choisir des lecteurs](choosing-drives.md)</li><li>[Planifier les volumes](plan-volumes.md)</li><li>[Utiliser des clusters de machines virtuelles invités](storage-spaces-direct-in-vm.md)</li><li>[Récupération d'urgence](storage-spaces-direct-disaster-recovery.md)</li> |
| **Déployer**<br><ul><li>[Déployer des espaces de stockage direct](deploy-storage-spaces-direct.md)</li><li>[Créer des volumes](create-volumes.md)</li><li>[Résilience imbriquée](nested-resiliency.md)</li><li>[Configurer le quorum](../../failover-clustering/manage-cluster-quorum.md)</li><li>[Mise à niveau d’un cluster d’espaces de stockage direct vers Windows Server 2019](upgrade-storage-spaces-direct-to-windows-server-2019.md)</li><li>[Comprendre et déployer la mémoire persistante](deploy-pmem.md)</li> | **Gérer**<br><ul><li>[Gérer avec Windows Admin Center](../../manage/windows-admin-center/use/manage-hyper-converged.md)</li><li>[Ajouter des serveurs ou des lecteurs](add-nodes.md)</li><li>[Mise d'un serveur hors connexion pour la maintenance](maintain-servers.md)</li><li>[Supprimer des serveurs](remove-servers.md)</li><li>[Étendre les volumes](resize-volumes.md)</li><li>[Supprimer des volumes](delete-volumes.md)</li><li>[Mettre à jour le microprogramme des disques](../update-firmware.md)</li><li>[Historique des performances](performance-history.md)</li><li>[Délimiter l'allocation des volumes](delimit-volume-allocation.md)</li><li>[Utiliser Azure Monitor sur un cluster hyper-convergé](configure-azure-monitor.md)</li> |
| **Résolution des problèmes**<br><ul><li>[Scénarios de résolution des problèmes](troubleshooting-storage-spaces.md)</li><li>[Résoudre les problèmes d’intégrité et les États opérationnels](storage-spaces-states.md)</li><li>[Collecter les données de diagnostic avec espaces de stockage direct](data-collection.md)</li><li>[Gestion du contrôle d’intégrité de mémoire de classe stockage](Storage-class-memory-health.md)</li> | **Billets de blog récents**<br><ul><li>[13,7 millions e/s par seconde avec espaces de stockage direct : le nouvel enregistrement du secteur pour l’infrastructure hyper-convergée](https://blogs.technet.microsoft.com/filecab/2018/10/30/windows-server-2019-and-intel-optane-dc-persistent-memory/)</li><li>[Infrastructure hyper-convergée dans Windows Server 2019-l’horloge du compte à rebours démarre maintenant !](https://blogs.technet.microsoft.com/filecab/2018/10/02/hci-the-countdown-clock-starts-now/)</li><li>[Cinq grandes annonces du Windows Server Summit](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap)</li><li>[10 000 espaces de stockage direct les clusters et le comptage...](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/)</li> |

## <a name="videos"></a>Vidéos

**Vue d’ensemble de la vidéo rapide (5 minutes)**

> [!Video https://www.youtube-nocookie.com/embed/raeUiNtMk0E]

**Espaces de stockage direct à Microsoft enflamme 2018 (1 heure)**

> [!Video https://www.youtube-nocookie.com/embed/5kaUiW3qo30]

**Espaces de stockage direct à Microsoft enflamme 2017 (1 heure)**

> [!Video https://www.youtube-nocookie.com/embed/YDr2sqNB-3c]

**Événement de lancement chez Microsoft enflamme 2016 (1 heure)**

> [!Video https://www.youtube-nocookie.com/embed/LK2ViRGbWs]

## <a name="key-benefits"></a>Principaux avantages

|       |       |
|   -   |   -   |
| ![Simplicité](media/storage-spaces-direct-in-windows-server-2016/simplicity-icon.png)   | **Simple.** Créez votre premier cluster d’espaces de stockage direct en moins de 15 minutes à partir de vos serveurs standard Windows Server 2016. Pour les utilisateurs de System Center, le déploiement se résume à cocher une case.       |
| ![Performances inégalées](media/storage-spaces-direct-in-windows-server-2016/performance-icon.png)   | **Performances inégalées.** Qu’ils soient 100 % flash ou hybrides, les espaces de stockage direct dépassent facilement [150 000 E/S mixtes 4K aléatoires par seconde sur chaque serveur](https://blogs.technet.microsoft.com/filecab/2016/07/26/storage-iops-update-with-storage-spaces-direct/) avec une latence faible et homogène du fait de leur architecture avec hyperviseur incorporé, de leur cache de lecture/écriture intégré et de la prise en charge de lecteurs NVMe de pointe montés directement sur le bus PCIe.      |
| ![Tolérance de panne](media/storage-spaces-direct-in-windows-server-2016/fault-tolerance-icon.png)   | **Tolérance de panne.** La résilience intégrée gère les défaillances des lecteurs, des serveurs ou des composants en assurant une disponibilité permanente. Les déploiements de grande envergure peuvent aussi être configurés pour assurer une [tolérance de panne au niveau des châssis et des racks](../../failover-clustering/fault-domains.md). Quand du matériel tombe en panne, il suffit de le remplacer. Les logiciels se réparent par eux-mêmes sans étapes d’administration fastidieuses.       |
| ![Efficacité des ressources](media/storage-spaces-direct-in-windows-server-2016/efficiency-icon.png)   | **Efficacité des ressources.** Le codage de l’effacement offre un stockage jusqu’à 2,4 fois plus efficace grâce à des innovations uniques comme les codes de reconstruction locale et la hiérarchisation en temps réel ReFS. Les gains profitent ainsi aux lecteurs de disque dur et aux charges de travail mixtes à chaud/à froid, tout en limitant l’utilisation du processeur pour réaffecter les ressources là où elles sont le plus nécessaire: au niveau des machines virtuelles.       |
| ![Facilité de gestion](media/storage-spaces-direct-in-windows-server-2016/manageability-icon.png)   | **Facilité de gestion**. Les [contrôles de qualité de service de stockage](../storage-qos/storage-qos-overview.md) permettent de garder les machines virtuelles fortement sollicitées dans des limites d’E/S par seconde minimales et maximales par machines virtuelles. Le [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md) intègre des fonctionnalités de surveillance et d’alerte en continu, et les nouvelles API facilitent la collecte de métriques de performances et de capacité d’une grande richesse à l’échelle du cluster.      |
| ![Extensibilité](media/storage-spaces-direct-in-windows-server-2016/scalability-icon.png)   | **Évolutivité**. Avec une capacité maximale de 16 serveurs et de plus de 400 lecteurs, avec jusqu’à 1 pétaoctet (1 000 téraoctets) de stockage par cluster. Pour une montée en charge, il suffit d’ajouter des lecteurs ou des serveurs supplémentaires ; les espaces de stockage direct intègrent automatiquement les nouveaux lecteurs et les utilisent aussitôt. L’efficacité et les performances du stockage s’améliorent de façon prévisible en fonction des besoins.       |

## <a name="deployment-options"></a>Options de déploiement

Les espaces de stockage direct ont été conçus pour deux types de déploiement :

### <a name="converged"></a>Convergé

**Stockage et calcul dans des clusters distincts.** L’option de déploiement convergé (ou « désagrégé ») dispose au-dessus des espaces de stockage direct une couche constituée d’un serveur de fichiers avec montée en puissance parallèle (SoFS) pour offrir un stockage NAS sur des partages de fichiers SMB3. Cela permet une mise à l’échelle des capacités de calcul et/ou de la charge de travail indépendamment du cluster de stockage, ce qui est essentiel dans le cas des déploiements à grande échelle tels que les infrastructures IaaS (Infrastructure as a Service) Hyper-V pour les fournisseurs de services et les grandes entreprises.

![Les espaces de stockage direct offrent du stockage à l’aide de la fonctionnalité de serveur de fichiers avec montée en puissance parallèle pour les ordinateurs virtuels Hyper-V dans un autre serveur ou cluster](media/storage-spaces-direct-in-windows-server-2016/converged-minimal.png)

### <a name="hyper-converged"></a>Déploiements hyperconvergés

**Un cluster pour le calcul et le stockage.** Le déploiement hyperconvergé exécute les machines virtuelles Hyper-V ou les bases de données SQL Server directement sur les serveurs de stockage en stockant leurs fichiers sur les volumes locaux. Il n’est donc plus nécessaire de configurer l’accès à un serveur de fichiers ni les autorisations, et les coûts en matériel s’en trouvent réduits dans le cas des déploiements dans les PME ou les succursales/bureaux distants. Consultez [déployer des espaces de stockage direct](deploy-storage-spaces-direct.md).

![Les espaces de stockage direct offrent du stockage aux ordinateurs virtuels Hyper-V dans le même cluster](media/storage-spaces-direct-in-windows-server-2016/hyper-converged-minimal.png)

## <a name="how-it-works"></a>Fonctionnement

Les espaces de stockage direct sont une évolution des espaces de stockage inaugurés par Windows Server 2012. Ils tirent parti des nombreuses fonctionnalités de Windows Server que vous connaissez déjà, telles que le clustering de basculement, le système de fichiers de volume partagé de cluster (CSV), le protocole SMB3 (Server Message Block) et bien entendu les espaces de stockage. De même, ils introduisent de nouvelles technologies, plus particulièrement le Software Storage Bus.

Voici une vue d’ensemble de la pile des espaces de stockage direct :

![Pile d’espaces de stockage direct](media/storage-spaces-direct-in-windows-server-2016/converged-full-stack.png)

**Matériel de mise en réseau.** Les espaces de stockage direct utilisent SMB3, dont SMB Direct et SMB Multichannel, via une connexion Ethernet pour assurer la communication entre les serveurs. Nous recommandons fortement le 10+ GbE avec un accès direct à la mémoire à distance (RDMA), iWARP ou RoCE.

**Matériel de stockage.** De 2 à 16serveurs équipés de disques SATA, SAS ou NVMe locaux. Chaque serveur doit disposer d’au moins 2 lecteurs SSD et au moins 4 lecteurs supplémentaires. Les appareils SATA et SAS doivent se trouver derrière un adaptateur de bus hôte/carte de bus hôte (HBA) et un expandeur SAS. Nous recommandons fortement les plateformes conçues avec soin et largement validées de nos partenaires (à venir).

**Clustering de basculement.** Les fonctionnalités de clustering intégrées à Windows Server servent à connecter les serveurs.

**Bus de stockage logiciel.** Le Software Storage Bus est une nouveauté des espaces de stockage direct. Il s’étend sur le cluster et établit une infrastructure de stockage à définition logicielle dans laquelle tous les serveurs peuvent voir les lecteurs locaux des uns des autres. Vous pouvez l’envisager comme une solution de remplacement au câblage coûteux et restrictif des solutions Fibre Channel ou SAS partagé.

**Cache de la couche de stockage.** Le bus de stockage de logiciels lie dynamiquement les lecteurs les plus rapides présents (par exemple, SSD) à des lecteurs plus lents (par ex., des disques durs) pour fournir une mise en cache en lecture/écriture côté serveur qui accélère l’e/s et augmente le débit.

**Pool de stockage.** L’ensemble de lecteurs qui constitue la base des espaces de stockage s’appelle « pool de stockage ». Il est créé automatiquement et tous les lecteurs éligibles sont automatiquement découverts et ajoutés à cet ensemble. Nous vous recommandons vivement d’utiliser un pool par cluster avec les paramètres par défaut. Lisez notre message de blog [Deep Dive into the Storage Pool](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) (Étude approfondie des pools de stockage) pour en savoir plus.

**Espaces de stockage.** Les espaces de stockage apportent une tolérance de panne aux « disques » virtuels grâce à [la mise en miroir, au codage d’effacement ou aux deux à la fois](storage-spaces-fault-tolerance.md). Vous pouvez les considérer comme une technologie RAID distribuée et définie par logiciel utilisant les lecteurs du pool. Dans les espaces de stockage direct, ces disques virtuels assurent généralement une résilience pour deux défaillances simultanées de disque ou de serveur (p.ex., mise en miroir à 3 voies, avec chaque copie de données sur un serveur différent), même si une tolérance de panne au niveau des châssis et des racks est également disponible.

**Système de fichiers résilient (ReFS).** ReFS est le premier système de fichiers conçu spécialement pour la virtualisation. Il permet notamment d’accélérer considérablement les opérations sur les fichiers .vhdx, telles que la création, l’extension et la fusion de points de contrôle. Les sommes de contrôle intégrées permettent quant à elles de détecter et corriger les erreurs de bit. Par ailleurs, il introduit des niveaux en temps réel qui assurent la rotation des données entre les niveaux de stockage dits «à chaud» et «à froid» en temps réel en fonction de l’utilisation.

**Volumes partagés de cluster.** Le système de fichiers CSV unifie tous les volumes ReFS dans un même espace de noms accessible à n’importe quel serveur, si bien que chaque serveur et chaque volume se présente et agit comme s’il était monté localement.

**Serveur de fichiers avec montée en puissance parallèle.** Cette ultime couche n’est nécessaire que dans les déploiements convergés. À la faveur du protocole d’accès SMB3, les clients (p.ex., un autre cluster exécutant Hyper-V) peuvent accéder aux fichiers distants via le réseau, transformant ainsi efficacement les espaces de stockage direct en dispostifs de stockage NAS (Network Attached Storage).

## <a name="customer-stories"></a>Témoignages de clients

Il existe [plus de 10 000 clusters](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum/) à travers le monde qui exécutent espaces de stockage direct. Les organisations de toutes tailles, des petites entreprises qui déploient simplement deux nœuds, aux grandes entreprises et aux gouvernements déployant des centaines de nœuds, dépendent de espaces de stockage direct pour leurs applications et leur infrastructure critiques.

Visitez [Microsoft.com/HCI](https://www.microsoft.com/hci) pour lire ses histoires :

[![grille des logos des clients](media/storage-spaces-direct-in-windows-server-2016/customer-stories.png)](https://www.microsoft.com/hci)

## <a name="management-tools"></a>Outils de gestion

Les outils suivants peuvent être utilisés pour gérer et/ou surveiller espaces de stockage direct :

| Nom | Graphique ou ligne de commande ? | Payé ou inclus ? |
|-----------------|----------------------------|-------------------|
| [Windows Admin Center](../../manage/windows-admin-center/overview.md)     | Interface    | Inclus |
| Gestionnaire de serveur & Gestionnaire du cluster de basculement                                 | Interface    | Inclus |
| Windows PowerShell                                                        | Ligne de commande | Inclus |
| [System Center Virtual Machine Manager (SCVMM)](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-storage-spaces-direct-vmm) <br>& [Operations Manager (SCOM)](https://www.microsoft.com/download/details.aspx?id=54700) | Interface    | Payant     |

## <a name="get-started"></a>Commencer

Essayez les espaces de stockage direct [dans Microsoft Azure](https://blogs.technet.microsoft.com/filecab/2016/05/05/s2dazuretp5/), ou téléchargez une version d’évaluation avec licence de 180 jours de Windows Server sur la page [Windows Server - Évaluations](https://go.microsoft.com/fwlink/?linkid=842602).

## <a name="see-also"></a>Voir également

- [Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)
- [Réplica de stockage](../storage-replica/storage-replica-overview.md)
- [Blog sur le stockage sur Microsoft](https://blogs.technet.microsoft.com/filecab/)
- [Débit d’espaces de stockage direct avec iWARP](https://blogs.technet.microsoft.com/filecab/2017/03/13/storage-spaces-direct-throughput-with-iwarp) (blog TechNet)
- [Nouveautés du clustering de basculement dans Windows Server](../../failover-clustering/whats-new-in-failover-clustering.md)  
- [Qualité de service de stockage](../storage-qos/storage-qos-overview.md)
- [Support technique pour les professionnels de l’informatique Windows](https://www.microsoft.com/itpro/windows/support)
