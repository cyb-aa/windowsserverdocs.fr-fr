---
title: Vue d’ensemble du système de fichiers résilient (ReFS)
ms.prod: windows-server
ms.author: gawatu
ms.manager: mchad
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 06/17/2019
ms.openlocfilehash: 91fdd5aa696c170cacc8903a65e996beb71c4b8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403014"
---
# <a name="resilient-file-system-refs-overview"></a>Vue d’ensemble du système de fichiers résilient (ReFS)

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semi-annuel)

ReFS (Resilient File System) est le dernier système de fichiers de Microsoft. Il a été conçu pour optimiser la disponibilité des données, s’adapter efficacement aux jeux de données volumineux sur diverses charges de travail et garantir l’intégrité des données grâce à sa capacité de résilience face aux endommagements. Ce système a vocation à prendre en charge davantage de scénarios de stockage et à constituer une base pour les innovations futures.

## <a name="key-benefits"></a>Principaux avantages

### <a name="resiliency"></a>Résilience

ReFS fournit de nouvelles fonctionnalités qui permettent de détecter avec précision les endommagements, mais aussi de les corriger en ligne, ce qui contribue à améliorer l’intégrité et la disponibilité de vos données : 

- **Flux d’intégrité**. ReFS utilise des sommes de contrôle pour les métadonnées et, éventuellement, pour les données de fichier, ce qui lui permet de détecter les endommagements de manière fiable. 
- **Intégration aux espaces de stockage**. Quand ReFS est utilisé en association avec un espace en miroir ou de parité, il peut automatiquement réparer les endommagements détectés à l’aide de la copie des données fournie par les espaces de stockage. Les processus de réparation sont effectués en ligne, sur la zone endommagée, et n’entraînent aucune inactivité du volume.
- **Récupération des données**. Si un volume est endommagé et qu’il n’existe pas de copie des données endommagées, ReFS supprime les données endommagées dans l’espace de noms. ReFS peut traiter la plupart des endommagements non réparables en maintenant le volume en ligne, mais il doit mettre le volume hors connexion dans de rares cas.
- **Correction proactive des erreurs**. En plus de valider les données avant les écritures et les lectures, ReFS utilise maintenant un scanneur d’intégrité des données, appelé <i>programme de nettoyage</i>. Ce programme de nettoyage analyse régulièrement le volume pour identifier les endommagements latents et déclencher de manière proactive la réparation des données endommagées. 

### <a name="performance"></a>Performances

En plus des améliorations apportées à la résilience, ReFS propose de nouvelles fonctionnalités pour les charges de travail virtualisées et dépendantes des performances. L’optimisation de niveau en temps réel, le clonage de bloc et le VDL fragmenté sont des exemples de nouvelles fonctionnalités du système ReFS conçues pour prendre en charge diverses charges de travail dynamiques :

- La parité à accélération **[miroir](./mirror-accelerated-parity.md)** avec mise en miroir à accélération miroir offre des performances élevées et un stockage à capacité efficace pour vos données. 

    - Pour offrir des performances élevées ainsi qu’un stockage de haute capacité, ReFS divise un volume en deux groupes de stockage logiques appelés niveaux. Ces niveaux peuvent avoir des types de disque et de résilience qui leur sont propres, ce qui permet à chaque niveau d’optimiser les performances ou la capacité. Voici des exemples de configurations : 
    
      | Niveau de performances | Niveau de capacité |
      | ---------------- | ----------------- |
      | SSD en miroir | HDD en miroir |
      | SSD en miroir | SSD de parité |
      | SSD en miroir | HDD de parité |
            
    - Une fois que ces niveaux sont configurés, ReFS les utilise pour fournir un stockage rapide pour les données actives (« hot ») et un stockage de haute capacité pour les données passives (« cold ») :
        - Toutes les écritures sont effectuées dans le niveau des performances. Les blocs de données volumineux qui restent dans ce niveau sont transférés en temps réel dans le niveau de capacité.
        - Si vous utilisez un déploiement hybride (mélange de lecteurs de disque dur et de mémoire flash), [le cache de espaces de stockage direct](../storage-spaces/understand-the-cache.md) permet d’accélérer les lectures, réduisant ainsi l’effet de la caractéristique de fragmentation des données des charges de travail virtualisées. Dans le cas contraire, si vous utilisez un déploiement tout-Flash, les lectures se produisent également dans le niveau de performance.

> [!NOTE]
> Pour les déploiements de serveur, la parité accélérée grâce à la mise en miroir est uniquement prise en charge par les [espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md). Nous vous recommandons d’utiliser la parité avec accélération miroir uniquement avec les charges de travail d’archivage et de sauvegarde. Pour les charges de travail virtualisées et d’autres charges de travail aléatoires hautes performances, nous vous recommandons d’utiliser des miroirs triples pour de meilleures performances.

- **Opérations de machine virtuelle plus rapides**. ReFS introduit de nouvelles fonctionnalités dont le principal objectif est d’améliorer les performances des charges de travail virtualisées :
    - [Clonage de bloc](./block-cloning.md) : cette fonctionnalité rend les opérations de copie plus rapides, en permettant d’effectuer rapidement et avec peu d’impact les opérations de fusion des points de contrôle de machine virtuelle.
    - VDL fragmenté : cette fonctionnalité permet à ReFS de remettre rapidement à zéro les fichiers, réduisant ainsi le temps nécessaire pour créer des disques durs virtuels de taille fixe de quelques dizaines de minutes à quelques secondes seulement.

- **Tailles de cluster variables** : ReFS prend en charge les tailles de cluster de 4 Ko et 64 Ko. Pour la plupart des déploiements, nous vous recommandons d’utiliser une taille de cluster de 4 Ko. Les clusters de 64 Ko sont appropriés pour les charges de travail d’E/S séquentielles volumineuses.

### <a name="scalability"></a>Extensibilité

ReFS est conçu pour prendre en charge des jeux de données extrêmement volumineux (millions de téraoctets) sans baisse des performances, offrant ainsi une plus grande extensibilité que les systèmes de fichiers antérieurs.

## <a name="supported-deployments"></a>Déploiements pris en charge

Microsoft a développé NTFS spécifiquement pour une utilisation à usage général avec un large éventail de configurations et de charges de travail. Toutefois, pour les clients qui requièrent expressément la disponibilité, la résilience et/ou la mise à l’échelle fournis par ReFS, Microsoft prend en charge les ReFS en vue de leur utilisation sous les configurations et les scénarios suivants. 

> [!NOTE]
> Toutes les configurations prises en charge par ReFS doivent utiliser le matériel certifié du [Catalogue Windows Server](https://www.WindowsServerCatalog.com) et répondre aux exigences de l’application.

### <a name="storage-spaces-direct"></a>Espaces de stockage directs

Le déploiement de ReFS sur des espaces de stockage direct est recommandé pour les charges de travail virtualisées ou le stockage connecté au réseau (NAS) : 
- La parité accélérée grâce à la mise en miroir et [le cache dans les espaces de stockage direct](../storage-spaces/understand-the-cache.md) offrent des performances élevées ainsi qu’un stockage de haute capacité. 
- Le clonage de bloc et le VDL fragmenté accélèrent considérablement les opérations sur les fichiers .vhdx, telles que la création, la fusion et l’extension.
- Intégrité : les flux de données, la réparation en ligne et les autres copies de données permettent à ReFS et espaces de stockage direct de détecter et de corriger les altérations du contrôleur de stockage et du support de stockage dans les métadonnées et les données. 
- ReFS fournit les fonctionnalités nécessaires pour s’adapter et prendre en charge les jeux de données volumineux. 

### <a name="storage-spaces"></a>Espaces de stockage

- Intégrité : les flux de données, la réparation en ligne et les copies de données secondaires permettent aux [espaces de stockage](../storage-spaces/overview.md) et ReFS de détecter et de corriger les altérations du contrôleur de stockage et du support de stockage dans les métadonnées et les données.
- Les déploiements d’espaces de stockage peuvent également utiliser le clonage de bloc et l’évolutivité proposés dans ReFS.
- Le déploiement de références sur les espaces de stockage avec des boîtiers SAS partagés est adapté à l’hébergement des données d’archivage et au stockage des documents utilisateur.

> [!NOTE]
> Les espaces de stockage prennent en charge l’attachement direct non amovible local via BusTypes SATA, SAS, NVME ou attaché via HBA (également appelé contrôleur RAID en mode pass-through).

### <a name="basic-disks"></a>Disques de base

Le déploiement de références sur des disques de base est mieux adapté aux applications qui implémentent leurs propres solutions de résilience et de disponibilité logicielles. 
- Les applications qui introduisent leurs propres solutions logicielles de résilience et de disponibilité peuvent exploiter les flux d’intégrité, le clonage de bloc et la capacité de dimensionner et prendre en charge les jeux de données volumineux. 

> [!NOTE]
> Les disques de base incluent une connexion directe non amovible locale via BusTypes SATA, SAS, NVME ou RAID. 

### <a name="backup-target"></a>Cible de sauvegarde

Le déploiement de ReFS en tant que cible de sauvegarde est idéal pour les applications et le matériel qui implémentent leurs propres solutions de résilience et de disponibilité.
- Les applications qui introduisent leurs propres solutions logicielles de résilience et de disponibilité peuvent exploiter les flux d’intégrité, le clonage de bloc et la capacité de dimensionner et prendre en charge les jeux de données volumineux.

> [!NOTE]
> Les cibles de sauvegarde incluent les configurations prises en charge ci-dessus. Pour plus d’informations sur la prise en charge de Fibre Channel et des réseaux SAN iSCSI, contactez les fournisseurs d’applications et de groupes de stockage. Pour les réseaux SAN, si des fonctionnalités telles que l’allocation dynamique, le démappage/le démappage ou le Transfert de données déchargée (ODX) sont nécessaires, NTFS doit être utilisé.   

## <a name="feature-comparison"></a>Comparaison des fonctionnalités

### <a name="limits"></a>Limites

| Fonctionnalité       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Longueur maximale des noms de fichier | 255 caractères Unicode  | 255 caractères Unicode               |
| Longueur maximale des chemins |32 000 caractères Unicode | 32 000 caractères Unicode                |
| Taille de fichier maximale | 35 po (pétaoctets)  | 256 To               |
| Taille de volume maximale | 35 PO                           | 256 To                |

### <a name="functionality"></a>Fonctionnalité

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>Les fonctionnalités suivantes sont disponibles sur ReFS et NTFS :

| Fonctionnalité       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Chiffrement BitLocker | Oui | Oui |
| Déduplication des données | Oui<sup>1</sup> | Oui |
| Prise en charge des volumes CSV (volumes partagés de cluster) | Oui<sup>2</sup> | Oui |
| Liens virtuels | Oui | Oui |
| Prise en charge des clusters de basculement | Oui | Oui |
| Listes de contrôle d’accès | Oui | Oui |
| Journal USN | Oui | Oui |
| Notifications de modifications | Oui | Oui |
| Points de jonction | Oui | Oui |
| Points de montage | Oui | Oui |
| Points d’analyse | Oui | Oui |
| Clichés instantanés de volume | Oui | Oui |
| ID de fichier | Oui | Oui |
| Verrouillages opportunistes (oplock) | Oui | Oui |
| Fichiers partiellement alloués | Oui | Oui |
| Flux avec nom | Oui | Oui |
| Allocation dynamique | Oui<sup>3</sup> | Oui |
| Supprimer/démapper | Oui<sup>3</sup> | Oui |
1. Disponible sur Windows Server, version 1709 et versions ultérieures.
2. Disponible sur Windows Server 2012 R2 et versions ultérieures.
3. Espaces de stockage uniquement

#### <a name="the-following-features-are-only-available-on-refs"></a>Les fonctionnalités suivantes sont uniquement disponibles sur ReFS :

| Fonctionnalité       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonage de bloc | Oui | Non |
| VDL fragmenté | Oui | Non |
| Parité accélérée grâce à la mise en miroir| Oui (sur les espaces de stockage direct) | Non |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>Les fonctionnalités suivantes ne sont pas actuellement disponibles sur ReFS :

| Fonctionnalité       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compression de système de fichiers | Non | Oui |
| Chiffrement du système de fichiers | Non | Oui |
| Transactions | Non | Oui |
| Liens physiques | Non | Oui |
| ID d’objet | Non | Oui |
| Transfert de données déchargées (ODX) | Non | Oui |
| Noms courts | Non | Oui |
| Attributs étendus | Non | Oui |
| Quotas de disque | Non | Oui |
| Démarrable | Non | Oui |
| Prise en charge des fichiers d’échange | Non | Oui |
| Prise en charge sur les médias amovibles | Non | Oui |

## <a name="see-also"></a>Voir aussi

- [Recommandations en matière de taille de cluster pour ReFS et NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Présentation de espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)
- [Clonage de bloc ReFS](block-cloning.md)
- [Flux d’intégrité ReFS](integrity-streams.md)