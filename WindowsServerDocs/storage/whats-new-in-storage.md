---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: Nouveautés du stockage dans Windows Server
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 05/29/2019
ms.openlocfilehash: 5469d663f64fdb453e03863f409b675473d3f6aa
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308571"
---
# <a name="whats-new-in-storage-in-windows-server"></a>Quelles sont les nouveautés dans le stockage dans Windows Server

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Cette rubrique décrit les fonctionnalités nouvelles et modifiées dans le stockage dans Windows Server 2019, Windows Server 2016, et libère de canal semi-annuel de serveur Windows.

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1903"></a>Quelles sont les nouveautés dans le stockage dans Windows Server 2019 et Windows Server, version 1903

Cette version de Windows Server ajoute les modifications et les technologies suivantes.

### <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Service de Migration de stockage migre désormais des comptes locaux, des clusters et des serveurs Linux

Service de Migration de stockage facilite la migration des serveurs vers une version plus récente de Windows Server. Il fournit un outil graphique qui inventorie les données sur les serveurs, puis transfère les données et la configuration sur de nouveaux serveurs, tout cela sans applications ou utilisateurs d’avoir à modifier quoi que ce soit.

Lorsque vous utilisez cette version de Windows Server pour orchestrer des migrations, nous avons ajouté les possibilités suivantes :

- Migrer des utilisateurs et groupes locaux vers le nouveau serveur
- Migrer le stockage à partir de clusters de basculement
- Migration du stockage d’un serveur Linux qui utilise Samba
- Synchroniser plus facilement les partages migrés dans Azure à l’aide d’Azure File Sync
- Migrer vers les nouveaux réseaux virtuels, tels que Azure

Pour plus d’informations sur le Service de Migration de stockage, consultez [vue d’ensemble du Service de Migration de stockage](storage-migration-service/overview.md).

### <a name="system-insights-disk-anomaly-detection"></a>Détection d’anomalie disque système Insights

[Système Insights](../manage/system-insights/overview.md) est une fonctionnalité d’analytique prédictive qui analyse les données de système de Windows Server localement et de mieux appréhender le fonctionnement du serveur. Il est fourni avec un nombre de fonctionnalités intégrées, mais nous avons ajouté la possibilité d’installer des fonctionnalités supplémentaires via Windows Admin Center, en commençant par la détection d’anomalie de disque.

Détection d’anomalie de disque est une nouvelle fonctionnalité qui met en surbrillance lorsque les disques sont comportent *différemment* que d’habitude. Si n’est pas différent nécessairement une mauvaise chose, voir ces instants anormales peut être utile lors du dépannage de problèmes sur vos systèmes.

Cette fonctionnalité est également disponible pour les serveurs exécutant Windows Server 2019.

### <a name="windows-admin-center-enhancements"></a>Améliorations de Windows Admin Center

Une nouvelle version de Windows Admin Center est out, ajout de nouvelles fonctionnalités à Windows Server. Pour plus d’informations sur les fonctionnalités les plus récentes, consultez [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="whats-new-in-storage-in-windows-server-2019-and-windows-server-version-1809"></a>Quelles sont les nouveautés dans le stockage dans Windows Server 2019 et Windows Server, version 1809

Cette version de Windows Server ajoute les modifications et les technologies suivantes.

### <a name="manage-storage-with-windows-admin-center"></a>Gérer le stockage avec Windows Admin Center

[Windows Admin Center](../manage/windows-admin-center/overview.md) est une nouvelle application déployée localement, basée sur navigateur pour la gestion des serveurs, des clusters, infrastructure Hyper-convergée avec espaces de stockage Direct, PC et Windows 10. Il est fourni sans coût supplémentaire au-delà de Windows et est prêt pour la production.

Pour être honnête, Windows Admin Center est un téléchargement distinct qui s’exécute sur Windows Server 2019 et d’autres versions de Windows, mais qu’il est nouveau et nous ne voulions vous manquez pas cette opportunité...

### <a name="storage-migration-service"></a>Service de migration du stockage

Le Service de migration du stockage est une nouvelle technologie qui facilite la migration des serveurs vers une version plus récente de Windows Server. Il offre un outil graphique qui répertorie les données sur les serveurs, transfère les données et la configuration vers les nouveaux serveurs puis, de manière facultative, fait passer les identités des anciens serveurs sur les nouveaux serveurs afin que les applications et les utilisateurs n’aient rien à modifier. Pour plus d’informations, consultez la section [Service de migration du stockage](storage-migration-service/overview.md).

### <a id="storage-spaces-direct"></a>Espaces de stockage Direct (Windows Server 2019 uniquement)

Il existe un certain nombre d’améliorations pour les espaces de stockage Direct dans Windows Server 2019 (espaces de stockage Direct n’est pas inclus dans Windows Server, canal semi-annuel) :

- **Déduplication et compression des volumes ReFS**

    Store jusqu'à dix fois plus de données sur le même volume avec déduplication et compression pour le système de fichiers ReFS. (Il a [un simple clic](https://www.youtube.com/watch?v=PRibTacyKko&feature=youtu.be) pour allumer avec Windows Admin Center.) Le magasin de blocs de taille variable avec la compression facultatif optimise le taux d’économies, tandis que l’architecture de post-traitement multithread conserve impact sur les performances minimal. Prend en charge de volumes pouvant atteindre 64 To et sera dédupliquer les 4 premiers To de chaque fichier.

- **Prise en charge native de la mémoire persistante**

    Libère des performances inédites avec la prise en charge native des espaces de stockage direct pour les modules de mémoire persistante, notamment Intel® Optane™ DC PM et NVDIMM-N. Utilisez la mémoire persistante comme cache pour accélérer la plage de travail active ou comme capacité pour garantir une faible latence régulière de l'ordre de la microseconde. Gérez la mémoire persistante comme vous le feriez pour n'importe quel autre lecteur dans PowerShell ou le Windows Admin Center.

- **Résilience imbriquée pour l’infrastructure Hyper-convergée de deux nœuds à la périphérie**

    Résistez à deux défaillances matérielles à la fois avec une toute nouvelle option de résilience logicielle inspirée de RAID 5+1. Avec la résilience imbriquée, un cluster d'espaces de stockage direct de deux nœuds peut fournir un stockage accessible en continu pour les applications et les machines virtuelles, même si un nœud de serveur tombe en panne et si un lecteur est défaillant sur l'autre nœud de serveur.

- **Lecteur flash de clusters de deux serveurs à l’aide d’une clé USB en tant que témoin**

    Utiliser une faible coût mémoire flash USB connectée à votre routeur pour agir en tant que témoin dans des clusters de deux serveurs. Si un serveur tombe en panne et puis sauvegarder, le cluster de lecteur USB sait identifier le serveur de données les plus récentes. Pour plus d’informations, consultez le [stockage sur le blog de Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Windows Admin Center**

    Gérez et surveillez les espaces de stockage direct avec le nouveau [tableau de bord spécialisé](../manage/windows-admin-center/use/manage-hyper-converged.md) et la nouvelle expérience dans le Windows Admin Center. Créez, ouvrez, développez ou supprimez des volumes en quelques clics seulement. Surveillez les performances telles que les IOPS et la latence des E/S, depuis le cluster global jusqu'au SSD ou disque dur individuel. Disponible sans coût supplémentaire pour Windows Server 2016 et Windows Server 2019.

- **Historique des performances**

    Obtenez sans effort une visibilité de l'utilisation des ressources et des performances avec [l'historique intégré](storage-spaces/performance-history.md). Plus de 50 compteurs essentiels couvrant le calcul, la mémoire, le réseau et le stockage sont automatiquement collectés et stockés sur le cluster jusqu'à un an. Surtout, il n'y a rien à installer, à configurer ou à démarrer : cela fonctionne, tout simplement. Visualisez dans le Windows Admin Center ou posez une requête et traitez dans PowerShell.

- **Mettre à l’échelle jusqu'à 4 po par cluster**

    Dimensionnez sur un échelle de plusieurs pétaoctets, ce qui est idéal pour le multimédia, les sauvegardes et l'archivage. Dans Windows Server 2019, les espaces de stockage direct prennent en charge jusqu'à 4 pétaoctets (Po) = 4 000 téraoctets de capacité brute par pool de stockage. Les recommandations associées en matière de capacité sont également augmentées : par exemple, vous pouvez créer deux fois plus de volumes (64 au lieu de 32), chacun deux fois plus gros qu'auparavant (64 To au lieu de 32 To). Assembler plusieurs clusters dans un [configuration du cluster](storage-spaces/cluster-sets.md) pour encore plus grande échelle au sein du stockage d’un espace de noms. Pour plus d’informations, consultez le [stockage sur le blog de Microsoft](https://blogs.technet.microsoft.com/filecab/2018/06/27/windows-server-summit-recap/).

- **Parité avec accélération miroir est 2 X plus rapide**

    Avec la parité accélérée par la mise en miroir, vous pouvez créer des volumes d'espaces de stockage direct qui sont en partie miroir et en partie parité, comme le mélange RAID-1 et RAID-5/6, pour obtenir le meilleur des deux mondes. (Il a [plus facile que vous ne le pensez](https://www.youtube.com/watch?v=R72QHudqWpE) dans Windows Admin Center.) Dans Windows Server 2019, les performances de la parité avec accélération miroir sont plus que doublé par rapport à Windows Server 2016 grâce à des optimisations.

- **Lecteur de détection d’observation ABERRANTE latence**

    Identifiez facilement les lecteurs ayant une latence anormale grâce à une surveillance proactive et à la détection intégré des anomalies, inspirées par le succès de longue date de l'approche de Microsoft Azure. Qu'il s'agisse de latence moyenne ou de quelque chose de plus subtil, comme une latence au 99e percentile, les lecteurs lents sont automatiquement étiquetés avec l'état « latence anormale » dans PowerShell et le Windows Admin Center.

- **Délimiter manuellement l’allocation de volumes pour augmenter la tolérance de panne**

    Ainsi, les administrateurs délimiter manuellement l’allocation de volumes dans les espaces de stockage Direct. Cette opération afin de peut augmenter considérablement d’une tolérance de panne dans certaines conditions, mais impose certaines considérations relatives à la gestion ajoutés et la complexité. Pour plus d’informations, consultez [délimiter l’allocation de volumes](storage-spaces/delimit-volume-allocation.md).

### <a name="storage-replica2019"></a>Réplica de stockage

Il existe un certain nombre d’améliorations à [le réplica de stockage](storage-replica/storage-replica-overview.md) dans cette version :

#### <a name="storage-replica-in-windows-server-standard-edition"></a>Réplica de stockage dans Windows Server, Édition Standard

Vous pouvez maintenant utiliser le réplica de stockage avec Windows Server, Édition Standard en plus de Datacenter Edition. Réplica de stockage s’exécutant sur Windows Server, Édition Standard, présente les limitations suivantes :

- Le réplica de stockage réplique un volume unique au lieu d’un nombre illimité de volumes.
- Volumes peuvent avoir une taille de 2 To au lieu d’une taille illimitée.

#### <a name="storage-replica-log-performance-improvements"></a>Améliorations des performances du journal de réplica de stockage

Nous avons également apporté des améliorations à la façon dont le journal de réplica de stockage effectue le suivi de réplication, améliorer le débit de la réplication et la latence, en particulier de stockage exclusivement flash, ainsi que des clusters d’espaces de stockage Direct qui répliquent entre eux.

Pour obtenir des performances accrues, tous les membres du groupe de réplication doivent exécuter Windows Server 2019.

#### <a name="test-failover"></a>Test de basculement

Vous pouvez désormais temporairement monter une capture instantanée de stockage répliqué sur un serveur de destination pour le test ou de sauvegarde à des fins. Pour plus d'informations, consultez le [Forum Aux Questions sur le réplica de stockage](https://aka.ms/srfaq).

#### <a name="windows-admin-center-support"></a>Prise en charge de Windows Admin Center

Prise en charge pour la gestion graphique de réplication est maintenant disponible dans Windows Admin Center via l’outil Gestionnaire de serveur. Cela comprend la réplication de serveur à serveur, réplication de cluster de cluster à cluster, ainsi que stretch.

#### <a name="miscellaneous-improvements"></a>Améliorations diverses

Le réplica de stockage contient également les améliorations suivantes :

-   Modifie asynchrone étire les comportements de cluster afin que les basculements automatiques se produisent maintenant
-   Plusieurs correctifs de bogues

### <a name="smb"></a>SMB

- **Suppression de l’authentification SMB1 et invité**: Windows Server n’installe plus le SMB1 client et le serveur par défaut. En outre, la possibilité d’authentification en tant qu’invité dans SMB2 et versions ultérieures est désactivée par défaut. Pour plus d’informations, consultez [SMBv1 n’est pas installé par défaut dans Windows 10, version 1709 et Windows Server, version 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilité et sécurité de SMB2/SMB3**: Options supplémentaires pour la compatibilité de sécurité et d’application ont été ajoutées, y compris la possibilité de désactiver oplocks dans SMB2 + pour les applications héritées, ainsi que d’exiger la signature ou le chiffrement sur chaque connexion à partir d’un client. Pour plus d’informations, consultez l’aide du module SMBShare PowerShell.

### <a name="data-deduplication"></a>Déduplication des données

- **Prend en charge de la déduplication des données désormais ReFS**: Vous n’avez plus devez choisir entre les avantages d’un système de fichiers moderne avec ReFS et de la déduplication des données : désormais, vous pouvez activer la déduplication des données chaque fois que vous pouvez activer (Refs). Augmentez l’efficacité du stockage de plus de 95 % avec ReFS.
- **API de port d’entrée/sortie optimisée pour les volumes dédupliqués**: Les développeurs peuvent désormais tirer parti des connaissances de la déduplication des données a sur le stockage de données efficacement pour déplacer des données entre les volumes, serveurs et efficacement des clusters.

### <a name="file-server-resource-manager"></a>Outils de gestion de ressources pour serveur de fichiers

Windows Server 2019 inclut la possibilité d’empêcher le service de File Server Resource Manager à partir de la création d’un journal des modifications (également appelé un journal USN) sur tous les volumes au démarrage du service. Cela permet d'économiser de l’espace sur chaque volume, mais désactivera la classification des fichiers en temps réel. Pour plus d’informations, voir [Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1803"></a>Quelles sont les nouveautés dans le stockage dans Windows Server, version 1803

### <a name="file-server-resource-manager"></a>Outils de gestion de ressources pour serveur de fichiers

Windows Server, version 1803 inclut la possibilité d’empêcher le service de File Server Resource Manager à partir de la création d’un journal des modifications (également appelé un journal USN) sur tous les volumes au démarrage du service. Cela permet d'économiser de l’espace sur chaque volume, mais désactivera la classification des fichiers en temps réel. Pour plus d’informations, voir [Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers](fsrm/fsrm-overview.md).

## <a name="whats-new-in-storage-in-windows-server-version-1709"></a>Quelles sont les nouveautés dans le stockage dans Windows Server, version 1709

Windows Server, version 1709 est la première version de Windows Server dans le canal semi-annuel. Le canal semi-annuel est un avantage de Software Assurance et est entièrement pris en charge en production de 18 mois, avec une nouvelle version de tous les six mois.

Pour plus d’informations, voir la [Présentation du canal semi-annuel de Windows Server](../get-started/semi-annual-channel-overview.md).

### <a name="storage-replica"></a>Réplica de stockage

La protection de récupération d’urgence ajoutée par le réplica de stockage est maintenant développée pour inclure :

- **Test de basculement** : le montage du stockage de destination est maintenant possible grâce à la fonctionnalité de test de basculement. Vous pouvez monter une capture instantanée du stockage répliqué sur les nœuds de destination de manière temporaire à des fins de test ou de sauvegarde. Pour plus d'informations, consultez le [Forum Aux Questions sur le réplica de stockage](https://aka.ms/srfaq).
- **Prise en charge Windows Admin Center**: Prise en charge pour la gestion graphique de réplication est maintenant disponible dans Windows Admin Center via l’outil Gestionnaire de serveur. Cela comprend la réplication de serveur à serveur, réplication de cluster de cluster à cluster, ainsi que stretch.

Le réplica de stockage contient également les améliorations suivantes :

-   Modifie asynchrone étire les comportements de cluster afin que les basculements automatiques se produisent maintenant
-   Plusieurs correctifs de bogues

### <a name="smb"></a>SMB

- **Suppression de l’authentification SMB1 et invité**: Windows Server, version 1709 n’installe plus le SMB1 client et le serveur par défaut. En outre, la possibilité d’authentification en tant qu’invité dans SMB2 et versions ultérieures est désactivée par défaut. Pour plus d’informations, consultez [SMBv1 n’est pas installé par défaut dans Windows 10, version 1709 et Windows Server, version 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilité et sécurité de SMB2/SMB3**: Options supplémentaires pour la compatibilité de sécurité et d’application ont été ajoutées, y compris la possibilité de désactiver oplocks dans SMB2 + pour les applications héritées, ainsi que d’exiger la signature ou le chiffrement sur chaque connexion à partir d’un client. Pour plus d’informations, consultez l’aide du module SMBShare PowerShell.

### <a name="data-deduplication"></a>Déduplication des données

- **Prend en charge de la déduplication des données désormais ReFS**: Vous n’avez plus devez choisir entre les avantages d’un système de fichiers moderne avec ReFS et de la déduplication des données : désormais, vous pouvez activer la déduplication des données chaque fois que vous pouvez activer (Refs). Augmentez l’efficacité du stockage de plus de 95 % avec ReFS.
- **API de port d’entrée/sortie optimisée pour les volumes dédupliqués**: Les développeurs peuvent désormais tirer parti des connaissances de la déduplication des données a sur le stockage de données efficacement pour déplacer des données entre les volumes, serveurs et efficacement des clusters.

## <a name="whats-new-in-storage-in-windows-server-2016"></a>Nouveautés du stockage dans Windows Server 2016

### <a name="s2d"></a>Espaces de stockage Direct  
Les espaces de stockage direct permettent de créer un stockage à haute disponibilité et scalable en utilisant des serveurs avec un stockage local. Le déploiement et la gestion des systèmes de stockage défini par un logiciel sont simplifiés, et il est maintenant possible d’utiliser de nouvelles classes de périphériques de disque, comme SSD SATA et NVMe, ce qui ne pouvait pas se faire auparavant avec les espaces de stockage en cluster avec des disques partagés.  

**Quels avantages cette modification procure-t-elle ?**  
Les espaces de stockage direct permettent aux fournisseurs de services et aux entreprises d’utiliser des serveurs standard avec un stockage local pour créer un stockage défini par un logiciel à haute disponibilité et scalable. L’utilisation de serveurs avec un stockage local réduit la complexité, augmente la scalabilité et permet d’utiliser des dispositifs de stockage, ce qui ne pouvait pas se faire auparavant, tels que des disques SSD SATA pour réduire le coût de stockage Flash ou NVMe pour de meilleures performances.  

Avec les espaces de stockage direct, vous n’avez plus besoin d’une structure SAS partagée, ce qui simplifie le déploiement et la configuration. À la place, ils utilisent le réseau comme structure de stockage en tirant parti de SMB3 et SMB Direct (RDMA) pour un stockage efficace de haut débit et avec un processeur à faible latence. Pour monter en charge, ajoutez simplement des serveurs pour augmenter la capacité de stockage et les performances d’E/S  
Pour plus d’informations, voir [Espaces de stockage direct dans Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md).  

**En quoi le fonctionnement est-il différent ?**  
Cette fonctionnalité est une nouveauté de Windows Server 2016.  

### <a name="storage-replica"></a>Réplica de stockage

Le réplica de stockage permet une réplication synchrone indépendante du stockage, au niveau du bloc, entre des clusters ou des serveurs pour la récupération d’urgence, ainsi que l’extension d’un cluster de basculement entre des sites. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers. La réplication asynchrone permet l’extension de site au-delà de plages métropolitaines avec la possibilité de perte de données.  

**Quels avantages cette modification procure-t-elle ?**  
La réplication du stockage permet d’effectuer les opérations suivantes :  

* Proposer une seule solution de reprise d’activité fournisseur pour les interruptions planifiées et non planifiées des charges de travail critiques.
* Utiliser le transport SMB3 avec des performances, une scalabilité et une fiabilité reconnues.
* Étendre les clusters de basculement Windows aux distances métropolitaines.
* Utiliser des logiciels Microsoft de bout en bout pour le stockage et le clustering, comme Hyper-V, le réplica de stockage, les espaces de stockage, le cluster, le serveur de fichiers avec montée en puissance parallèle, SMB3, la déduplication et ReFS/NTFS.
* Réduire les coûts et la complexité comme suit : 
    * La réplication est indépendante du matériel et ne nécessite pas de configuration de stockage particulière, comme DAS ou SAN.
    * Elle autorise des technologies de mise en réseau et de stockage des produits.
    * Elle facilite la gestion graphique pour les nœuds individuels et les clusters via le Gestionnaire du cluster de basculement.
    * Elle inclut des options de script complètes et à grande échelle via Windows PowerShell. 
* Réduire les temps d’arrêt et augmenter la fiabilité et la productivité intrinsèques à Windows.  
* Fournir la capacité de prise en charge, les mesures de performances et les fonctionnalités de diagnostic.  

Pour plus d’informations, voir [Réplica de stockage dans Windows Server 2016](storage-replica/storage-replica-overview.md).  

**En quoi le fonctionnement est-il différent ?**  
Cette fonctionnalité est une nouveauté de Windows Server 2016.  

### <a name="storage-qos"></a>Qualité de Service de stockage  
Vous pouvez maintenant utiliser la qualité de service (QoS) de stockage pour analyser de manière centralisée les performances de stockage de bout en bout et créer des stratégies de gestion avec Hyper-V et des clusters CSV dans Windows Server 2016.  

**Quels avantages cette modification procure-t-elle ?**  
Il est désormais possible de créer des stratégies QoS de stockage sur un serveur CSV et de les affecter à un ou plusieurs disques virtuels sur des machines virtuelles Hyper-V. Les performances du stockage sont automatiquement réajustées pour répondre aux besoins des stratégies lorsque les charges de travail et de stockage varient.  

* Chaque stratégie peut spécifier une réserve (minimum) et/ou une limite (maximum) à appliquer à une collection de flux de données, comme un disque dur virtuel, une machine virtuelle unique ou un groupe de machines virtuelles, ou encore un service ou un locataire.  
* À l’aide de Windows PowerShell ou WMI, vous pouvez effectuer les tâches suivantes :  
    * créer des stratégies sur un cluster CSV ;
    * énumérer les stratégies disponibles sur un cluster CSV ;
    * affecter une stratégie à un disque dur virtuel sur une machine virtuelle Hyper-V. 
    * Analyser les performances de chaque flux et l’état de la stratégie.  
* Si plusieurs disques durs virtuels partagent la même stratégie, les performances sont réparties équitablement afin de répondre à la demande entre les valeurs minimale et maximale de la stratégie. Par conséquent, une stratégie peut servir à gérer un disque dur virtuel, une machine virtuelle, plusieurs machines virtuelles comprenant un service ou toutes les machines virtuelles détenues par un locataire.  

**En quoi le fonctionnement est-il différent ?**  
Cette fonctionnalité est une nouveauté de Windows Server 2016. Auparavant, Windows Server ne proposait aucune fonctionnalité d’administration des réserves minimales, de surveillance des flux de l’ensemble des disques virtuels sur le cluster via une commande unique ou de gestion centralisée basée sur des stratégies.  

Pour plus d’informations, voir [Qualité de service de stockage](storage-qos/storage-qos-overview.md)

### <a name="dedup"></a>Déduplication des données  
| Fonctionnalité | Nouveauté ou mise à jour | Description |
|---------------|----------------|-------------|
| [Prise en charge pour les gros Volumes](data-deduplication/whats-new.md#large-volume-support) | Mise à jour terminée | Avant Windows Server 2016, les volumes devaient être dimensionnés de façon spécifique pour l’activité attendue, avec des tailles de volume supérieures à 10 To qui ne convenaient pas à la déduplication. Dans Windows Server 2016, la déduplication des données prend en charge des tailles de volume **pouvant atteindre 64 To**. |
| [Prise en charge des fichiers volumineux](data-deduplication/whats-new.md#large-file-support) | Mise à jour terminée | Avant Windows Server 2016, les fichiers dont la taille était proche de 1 To ne convenaient pas à la déduplication. Dans Windows Server 2016, les fichiers dont la taille peut atteindre **jusqu’à 1 To** sont entièrement pris en charge. |
| [Prise en charge de Nano Server](data-deduplication/whats-new.md#nano-server-support) | Nouveau | La déduplication des données est disponible et entièrement prise en charge dans la nouvelle option de déploiement de Nano Server pour Windows Server 2016. |
| [Prise en charge de sauvegarde simplifiée](data-deduplication/whats-new.md#simple-backup-support) | Nouveau | Dans Windows Server 2012 R2, les applications de sauvegarde virtualisées, telles que [Data Protection Manager](https://technet.microsoft.com/library/hh758173.aspx) de Microsoft, étaient prises en charge via une série d’étapes de configuration manuelles. Dans Windows Server 2016, un nouveau type d’utilisation « Sauvegarde » par défaut a été ajouté pour un déploiement transparent de la déduplication des données pour les applications de sauvegarde virtualisées. |
| [Prise en charge de système d’exploitation de Cluster mises à niveau propagées](data-deduplication/whats-new.md#cluster-upgrade-support) | Nouveau | La déduplication des données prend entièrement en charge la nouvelle fonctionnalité de [mise à niveau propagée de système d’exploitation de cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) de Windows Server 2016. |

### <a name="smb-hardening-improvements"></a>Améliorations pour les connexions SYSVOL et NETLOGON la sécurisation renforcée SMB  
Dans Windows 10 et Windows Server 2016, les connexions clientes aux partages SYSVOL et NETLOGON par défaut des services de domaine Active Directory sur les contrôleurs de domaine nécessitent maintenant une signature SMB et une authentification mutuelle (par exemple, Kerberos).   

**Quels avantages cette modification procure-t-elle ?**  
Cette modification réduit le risque d’attaques de l’intercepteur.   

**En quoi le fonctionnement est-il différent ?**  
Si la signature SMB et l’authentification mutuelle ne sont pas disponibles, un ordinateur Windows 10 ou Windows Server 2016 ne peut pas traiter les scripts ni la stratégie de groupe basés sur un domaine.  

> [!NOTE]  
> Les valeurs de Registre pour ces paramètres ne sont pas présentes par défaut, mais les règles de sécurisation renforcée s’appliquent jusqu’à ce qu’elles soient remplacées par une stratégie de groupe ou d’autres valeurs de Registre.  

Pour plus d’informations sur ces améliorations de sécurité - reflétant sécurisation renforcée UNC, consultez l’article de la Base de connaissances Microsoft [3000483](https://support.microsoft.com/kb/3000483) et [MS15-011 & MS15-014 : Stratégie de groupe de renforcement](https://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy).  

### <a name="work-folders"></a>Dossiers de travail
Notification de modification améliorée lorsque le serveur de dossiers de travail exécute Windows Server 2016 et le client de dossiers de travail est Windows 10.

**Quels avantages cette modification procure-t-elle ?**<br>
Pour Windows Server 2012 R2, lorsque les modifications apportées aux fichiers sont synchronisées avec le serveur Dossiers de travail, les clients ne sont pas informés des modifications et attendent jusqu’à 10 minutes pour obtenir la mise à jour.  Lorsque vous utilisez Windows Server 2016, le serveur de dossiers de travail informe immédiatement les clients Windows 10 et les modifications de fichiers sont synchronisées immédiatement.

**En quoi le fonctionnement est-il différent ?**<br>
Cette fonctionnalité est une nouveauté de Windows Server 2016. Pour que vous puissiez l’utiliser, le serveur Dossiers de travail doit être doté de Windows Server 2016 et le client doit héberger Windows 10.

Si vous utilisez un client plus ancien ou si le serveur Dossiers de travail exécute Windows Server 2012 R2, le client continue à interroger le système toutes les 10 minutes, afin d’obtenir les modifications.

### <a name="refs"></a>ReFS 
La nouvelle version de ReFS permet de réaliser des déploiements de stockage à grande échelle avec diverses charges de travail, offrant ainsi plus de fiabilité, de résilience et d'extensibilité pour vos données.     

**Quels avantages cette modification procure-t-elle ?**<br>
ReFS offre les améliorations suivantes :

* ReFS offre une nouvelle fonctionnalité de hiérarchisation du stockage, permettant d'accélérer les performances et d'augmenter la capacité de stockage. Cette nouvelle fonctionnalité offre les avantages suivants :
    * Plusieurs types de résilience sur le même lecteur virtuel (utilisation de la mise en miroir pour les performances et de la parité pour la capacité, par exemple).
    * Meilleure réactivité pour les dérives de plages de travail.  
* L'introduction du clonage de bloc améliore considérablement les performances des opérations de machine virtuelle, comme les opérations de fusion de point de contrôle .vhdx.
* Le nouvel outil d'analyse ReFS permet de réparer les fuites de stockage et de récupérer les données en cas de corruption critique. 

**En quoi le fonctionnement est-il différent ?**<br>
Ces fonctionnalités sont nouvelles dans Windows Server 2016. 

## <a name="see-also"></a>Voir aussi  
* [Nouveautés de Windows Server 2016](../get-started/what-s-new-in-windows-server-2016.md)  
