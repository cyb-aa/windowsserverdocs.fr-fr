---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: "Nouveautés du stockage dans WindowsServer"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: kumudd
ms.date: 09/15/2016
ms.openlocfilehash: 9aab6246f7ddc86629834bf20a7d21cc4ce2ec8f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-storage-in-windows-server-2016"></a>Nouveautés du stockage dans WindowsServer2016

>S’applique à: WindowsServer2016

Cette rubrique décrit les fonctionnalités nouvelles et modifiées du stockage dans Windows Server2016.

## <a name="s2d"></a>Espaces de stockage directs  
Les espaces de stockage direct permettent de créer un stockage à haute disponibilité et scalable en utilisant des serveurs avec un stockage local. Le déploiement et la gestion des systèmes de stockage défini par un logiciel sont simplifiés, et il est maintenant possible d’utiliser de nouvelles classes de périphériques de disque, comme SSD SATA et NVMe, ce qui ne pouvait pas se faire auparavant avec les espaces de stockage en cluster avec des disques partagés.  

**Quels avantages cette modification procure-t-elle?**  
Les espaces de stockage direct permettent aux fournisseurs de services et aux entreprises d’utiliser des serveurs standard avec un stockage local pour créer un stockage défini par un logiciel à haute disponibilité et scalable. L’utilisation de serveurs avec un stockage local réduit la complexité, augmente la scalabilité et permet d’utiliser des dispositifs de stockage, ce qui ne pouvait pas se faire auparavant, tels que des disques SSD SATA pour réduire le coût de stockage Flash ou NVMe pour de meilleures performances.  

Avec les espaces de stockage direct, vous n’avez plus besoin d’une structure SAS partagée, ce qui simplifie le déploiement et la configuration. À la place, ils utilisent le réseau comme structure de stockage en tirant parti de SMB3 et SMB Direct (RDMA) pour un stockage efficace de haut débit et avec un processeur à faible latence. Pour monter en charge, ajoutez simplement des serveurs pour augmenter la capacité de stockage et les performances d’E/S  
Pour plus d’informations, voir [Espaces de stockage direct dans Windows Server2016](storage-spaces/storage-spaces-direct-overview.md).  

**En quoi le fonctionnement est-il différent?**  
Cette fonctionnalité est une nouveauté de Windows Server2016.  

## <a name="storage-replica"></a>Réplica de stockage  
Le réplica de stockage permet une réplication synchrone indépendante du stockage, au niveau du bloc, entre des clusters ou des serveurs pour la récupération d’urgence, ainsi que l’extension d’un cluster de basculement entre des sites. La réplication synchrone permet la mise en miroir des données dans des sites physiques avec des volumes cohérents en cas d’incident, ce qui garantit aucune perte de données au niveau du système de fichiers. La réplication asynchrone permet l’extension de site au-delà de plages métropolitaines avec la possibilité de perte de données.  

**Quels avantages cette modification procure-t-elle?**  
La réplication du stockage permet d’effectuer les opérations suivantes:  

* Proposer une seule solution de reprise d’activité fournisseur pour les interruptions planifiées et non planifiées des charges de travail critiques.
* Utiliser le transport SMB3 avec des performances, une scalabilité et une fiabilité reconnues.
* Étendre les clusters de basculement Windows aux distances métropolitaines.
* Utiliser des logiciels Microsoft de bout en bout pour le stockage et le clustering, comme Hyper-V, le réplica de stockage, les espaces de stockage, le cluster, le serveur de fichiers avec montée en puissance parallèle, SMB3, la déduplication et ReFS/NTFS.
* Réduire les coûts et la complexité comme suit: 
    * La réplication est indépendante du matériel et ne nécessite pas de configuration de stockage particulière, comme DAS ou SAN.
    * Elle autorise des technologies de mise en réseau et de stockage des produits.
    * Elle facilite la gestion graphique pour les nœuds individuels et les clusters via le Gestionnaire du cluster de basculement.
    * Elle inclut des options de script complètes et à grande échelle via Windows PowerShell. 
* Réduire les temps d’arrêt et augmenter la fiabilité et la productivité intrinsèques à Windows.  
* Fournir la capacité de prise en charge, les mesures de performances et les fonctionnalités de diagnostic.  

Pour plus d’informations, voir [Réplica de stockage dans Windows Server2016](storage-replica/storage-replica-overview.md).  

**En quoi le fonctionnement est-il différent?**  
Cette fonctionnalité est une nouveauté de Windows Server2016.  

## <a name="storage-qos"></a>Qualité de service de stockage  
Vous pouvez maintenant utiliser la qualité de service (QoS) du stockage pour surveiller de manière centralisée les performances du stockage de bout en bout et créer des stratégies de gestion avec des clustersCSV etHyper-V dans Windows Server2016.  

**Quels avantages cette modification procure-t-elle?**  
Il est désormais possible de créer des stratégiesQoS de stockage sur un serveurCSV et de les affecter à un ou plusieurs disques virtuels sur des machines virtuellesHyper-V. Les performances du stockage sont automatiquement réajustées pour répondre aux besoins des stratégies lorsque les charges de travail et de stockage varient.  

* Chaque stratégie peut spécifier une réserve (minimum) et/ou une limite (maximum) à appliquer à une collection de flux de données, comme un disque dur virtuel, une machine virtuelle unique ou un groupe de machines virtuelles, ou encore un service ou un locataire.  
* À l’aide de Windows PowerShell ou WMI, vous pouvez effectuer les tâches suivantes:  
    * créer des stratégies sur un clusterCSV;
    * énumérer les stratégies disponibles sur un clusterCSV;
    * affecter une stratégie à un disque dur virtuel sur une machine virtuelleHyper-V. 
    * Analyser les performances de chaque flux et l’état de la stratégie.  
* Si plusieurs disques durs virtuels partagent la même stratégie, les performances sont réparties équitablement afin de répondre à la demande entre les valeurs minimale et maximale de la stratégie. Par conséquent, une stratégie peut servir à gérer un disque dur virtuel, une machine virtuelle, plusieurs machines virtuelles comprenant un service ou toutes les machines virtuelles détenues par un locataire.  

**En quoi le fonctionnement est-il différent ?**  
Cette fonctionnalité est une nouveauté de Windows Server2016. Auparavant, WindowsServer ne proposait aucune fonctionnalité d’administration des réserves minimales, de surveillance des flux de l’ensemble des disques virtuels sur le cluster via une commande unique ou de gestion centralisée basée sur des stratégies.  

Pour plus d’informations, voir [Qualité de service de stockage](storage-qos/storage-qos-overview.md)

## <a name="dedup"></a>Déduplication des données  
| Fonctionnalités | Nouveauté ou mise à jour | Description |
|---------------|----------------|-------------|
| [Prise en charge de volumes importants](data-deduplication/whats-new.md#large-volume-support) | Mis à jour | Avant Windows Server2016, les volumes devaient être dimensionnés de façon spécifique pour l’activité attendue, avec des tailles de volume supérieures à 10To qui ne convenaient pas à la déduplication. Dans Windows Server2016, la déduplication des données prend en charge des tailles de volume **pouvant atteindre 64To**. |
| [Prise en charge de fichiers volumineux](data-deduplication/whats-new.md#large-file-support) | Mis à jour | Avant Windows Server2016, les fichiers dont la taille était proche de 1To ne convenaient pas à la déduplication. Dans Windows Server2016, les fichiers dont la taille peut atteindre **jusqu’à 1To** sont entièrement pris en charge. |
| [Prise en charge de Nano Server](data-deduplication/whats-new.md#nano-server-support) | Nouveau | La déduplication des données est disponible et entièrement prise en charge dans la nouvelle option de déploiement de Nano Server pour Windows Server2016. |
| [Prise en charge de sauvegarde simplifiée](data-deduplication/whats-new.md#simple-backup-support) | Nouveau | Dans Windows Server2012R2, les applications de sauvegarde virtualisées, telles que [Data Protection Manager](https://technet.microsoft.com/en-us/library/hh758173.aspx) de Microsoft, étaient prises en charge via une série d’étapes de configuration manuelles. Dans Windows Server2016, un nouveau type d’utilisation «Sauvegarde» par défaut a été ajouté pour un déploiement transparent de la déduplication des données pour les applications de sauvegarde virtualisées. |
| [Prise en charge des mises à niveau propagées de système d’exploitation de cluster](data-deduplication/whats-new.md#cluster-upgrade-support) | Nouveau | La déduplication des données prend entièrement en charge la nouvelle fonctionnalité de [mise à niveau propagée de système d’exploitation de cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) de Windows Server2016. |

## <a name="smb-hardening-improvements"></a>Améliorations de la sécurisation renforcée SMB pour les connexions SYSVOL et NETLOGON  
Dans Windows10 et Windows Server2016, les connexions clientes aux partages SYSVOL et NETLOGON par défaut des services de domaine Active Directory sur les contrôleurs de domaine nécessitent maintenant une signature SMB et une authentification mutuelle (par exemple, Kerberos).   

**Quels avantages cette modification procure-t-elle?**  
Cette modification réduit la probabilité d’attaques de l’intercepteur.   

**En quoi le fonctionnement est-il différent?**  
Si la signature SMB et l’authentification mutuelle ne sont pas disponibles, un ordinateur Windows10 ou Windows Server2016 ne peut pas traiter les scripts ni la stratégie de groupe basés sur un domaine.  

> [!NOTE]  
> Les valeurs de Registre pour ces paramètres ne sont pas présentes par défaut, mais les règles de sécurisation renforcée s’appliquent jusqu’à ce qu’elles soient remplacées par une stratégie de groupe ou d’autres valeurs de Registre.  

Pour plus d’informations sur ces améliorations de la sécurité (ou sécurisation renforcée UNC), voir l’article de la Base de connaissances Microsoft [3000483](http://support.microsoft.com/kb/3000483) et [MS15-011 & MS15-014: Hardening Group Policy](http://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy).  

## <a name="work-folders"></a>Dossiers de travail
Les notifications de modification émises sont améliorées lorsque le serveurDossiers de travail exécute WindowsServer2016 et que le clientDossiers de travail est doté de Windows10.

**Quels avantages cette modification procure-t-elle?**<br>
Pour Windows Server2012R2, lorsque les modifications apportées aux fichiers sont synchronisées avec le serveurDossiers de travail, les clients ne sont pas informés des modifications et attendent jusqu’à 10minutes pour obtenir la mise à jour.  Lorsque vous utilisez WindowsServer2016, le serveurDossiers de travail informe immédiatement les clients Windows10 et les modifications apportées aux fichiers sont tout de suite synchronisées.

**En quoi le fonctionnement est-il différent ?**<br>
Cette fonctionnalité est une nouveauté de Windows Server2016. Pour que vous puissiez l’utiliser, le serveurDossiers de travail doit être doté de WindowsServer2016 et le client doit hébergerWindows10.

Si vous utilisez un client plus ancien ou si le serveurDossiers de travail exécute WindowsServer2012R2, le client continue à interroger le système toutes les 10minutes, afin d’obtenir les modifications.

## <a name="refs"></a>ReFS 
La nouvelle version de ReFS permet de réaliser des déploiements de stockage à grande échelle avec diverses charges de travail, offrant ainsi plus de fiabilité, de résilience et d'extensibilité pour vos données.     

**Quels avantages cette modification procure-t-elle?**<br>
ReFS offre les améliorations suivantes:

* ReFS offre une nouvelle fonctionnalité de hiérarchisation du stockage, permettant d'accélérer les performances et d'augmenter la capacité de stockage. Cette nouvelle fonctionnalité offre les avantages suivants:
    * Plusieurs types de résilience sur le même lecteur virtuel (utilisation de la mise en miroir pour les performances et de la parité pour la capacité, par exemple).
    * Meilleure réactivité pour les dérives de plages de travail. 
    * Prise en charge du support SMR (Shingled Magnetic Recording). 
* L'introduction du clonage de bloc améliore considérablement les performances des opérations de machine virtuelle, comme les opérations de fusion de point de contrôle .vhdx.
* Le nouvel outil d'analyse ReFS permet de réparer les fuites de stockage et de récupérer les données en cas de corruption critique. 

**En quoi le fonctionnement est-il différent?**<br>
Ces fonctionnalités sont nouvelles dans WindowsServer2016. 

## <a name="see-also"></a>Voir aussi  
* [Nouveautés de WindowsServer2016](../get-started/what-s-new-in-windows-server-2016.md)  
