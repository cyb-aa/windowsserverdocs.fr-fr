---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clustering de basculement
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: high
ms.openlocfilehash: a7f6dd847d2762dbc616189ed729449479788f98
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810904"
---
# <a name="failover-clustering-in-windows-server"></a>Clustering de basculement dans Windows Server

> S’applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement est un groupe d’ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité et l’extensibilité des rôles en cluster (appelés précédemment « applications et services en cluster »). Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. En cas de défaillance d’un ou plusieurs nœuds, d’autres nœuds prennent le relais pour fournir les services requis (processus appelé « basculement »). En outre, les rôles en cluster sont surveillés de manière proactive pour vérifier qu’ils fonctionnent correctement. S’ils ne fonctionnent pas, ils sont redémarrés ou déplacés vers un autre nœud.

Les clusters de basculement offrent également la fonctionnalité de volume partagé de cluster (CSV) qui procure un espace de noms distribué et cohérent que les rôles en cluster peuvent utiliser pour accéder au stockage partagé à partir de tous les nœuds. La fonctionnalité de clustering de basculement garantit ainsi aux utilisateurs des interruptions de service minimales.

Le clustering de basculement a de nombreuses applications pratiques, notamment :

* Stockage de partage de fichiers hautement disponible ou disponible en continu destiné à des applications telles que Microsoft SQL Server et à des ordinateurs virtuels Hyper-V.
* Rôles en cluster hautement disponibles qui sont exécutés sur des serveurs physiques ou des ordinateurs virtuels installés sur des serveurs exécutant Hyper-V

| **Comprendre**                                                               |  **Planification**                          |  **Déploiement**       |
| -------------                                                                |  --------------                        | --------------------- |
| [Nouveautés dans le clustering de basculement](whats-new-in-failover-clustering.md)    | [Planification de la configuration matérielle requise pour le clustering de basculement et options de stockage](clustering-requirements.md)  | [Création d’un cluster de basculement](create-failover-cluster.md) |
| [Serveur de fichiers avec montée en puissance parallèle pour les données d’application](sofs-overview.md)               | [Utiliser des volumes partagés de cluster (CSV)](failover-cluster-csvs.md) | [Déployer un serveur de fichiers à deux nœuds](../storage/storage-spaces/storage-spaces-direct-in-vm.md) |
|  [Quorum du cluster et du pool](../storage/storage-spaces/understand-quorum.md)   |  [Utilisation de clusters de machines virtuelles invitées avec des espaces de stockage direct](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [Préparer les comptes d’ordinateur de cluster dans Active Directory Domain Services](prestage-cluster-adds.md) |
| [Reconnaissance des domaines d'erreur](fault-domains.md)                                 |                                 | [Configurer des comptes de cluster dans Active Directory](configure-ad-accounts.md) |
| [Réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés](smb-multichannel.md) |                       | [Gérer le quorum et les témoins](manage-cluster-quorum.md) |
| [Équilibrage de charge des machines virtuelles](vm-load-balancing-overview.md)                         |                             | [Déployer un témoin cloud](deploy-cloud-witness.md) |
| [Jeux de clusters](../storage/storage-spaces/cluster-sets.md)                  |                             |[Déployer un témoin de partage de fichiers](file-share-witness.md) |
| [Affinité de cluster](cluster-affinity.md)                                     |                            | [Mise à niveau propagée de système d’exploitation de cluster](cluster-operating-system-rolling-upgrade.md) |
|                                                                             |                            | [Mettre à jour un cluster de basculement sur le même matériel](upgrade-option-same-hardware.md) |
|                                                                            |                             | [Déployer un cluster détaché d’Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))

|**Gérer**  |  **Outils et paramètres**  |  **Ressources de la communauté**       |
| ------------- |  -------------- | --------------------- |
| [ Mise à jour adaptée aux clusters](cluster-aware-updating.md)    |   [Applets de commande PowerShell pour le clustering de basculement](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)      |  [Forum sur la haute disponibilité (clustering)](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [Service de contrôle d'intégrité](health-service-overview.md)   |   [Applets de commande PowerShell pour la mise à jour adaptée aux clusters](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)      | [Blog de l’équipe sur le clustering de basculement et l’équilibrage de la charge réseau](http://blogs.msdn.com/b/clustering/)        |
|  [Migration de domaine en cluster](cluster-domain-migration.md)   |         |         |
|  [Résolution des problèmes à l'aide du Rapport d'erreurs Windows](troubleshooting-using-wer-reports.md)   |         |         |
