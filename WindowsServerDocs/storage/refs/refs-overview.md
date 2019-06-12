---
title: Vue d’ensemble du système de fichiers résilient (ReFS)
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: mchad
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 06/07/2019
ms.openlocfilehash: fed23c999c67ba81b3bbb821170a748ed5eaa7b8
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812016"
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

- **[Parité avec accélération miroir](./mirror-accelerated-parity.md)**  -parité avec accélération miroir offre à la fois des performances élevées et un stockage efficace pour vos données de capacité. 

    - Pour offrir des performances élevées ainsi qu’un stockage de haute capacité, ReFS divise un volume en deux groupes de stockage logiques appelés niveaux. Ces niveaux peuvent avoir des types de disque et de résilience qui leur sont propres, ce qui permet à chaque niveau d’optimiser les performances ou la capacité. Voici des exemples de configurations : 
    
      | Niveau de performances | Niveau de capacité |
      | ---------------- | ----------------- |
      | SSD en miroir | HDD en miroir |
      | SSD en miroir | SSD de parité |
      | SSD en miroir | HDD de parité |
            
    - Une fois que ces niveaux sont configurés, ReFS les utilise pour fournir un stockage rapide pour les données actives (« hot ») et un stockage de haute capacité pour les données passives (« cold ») :
        - Toutes les écritures sont effectuées dans le niveau des performances. Les blocs de données volumineux qui restent dans ce niveau sont transférés en temps réel dans le niveau de capacité.
        - Si vous utilisez un déploiement hybride (mélange flash et des lecteurs de disque dur), [le cache dans les espaces de stockage Direct](../storage-spaces/understand-the-cache.md) permet d’accélérer les lectures, de réduire l’effet de données fragmentation caractéristique des charges de travail virtualisées. Sinon, si vous utilisez un déploiement exclusivement flash, lectures également se produisent dans le niveau de performances.

> [!NOTE]
> Pour les déploiements de serveur, la parité accélérée grâce à la mise en miroir est uniquement prise en charge par les [espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md). Nous recommandons à l’aide de la parité avec accélération miroir avec l’archivage et sauvegarde des charges de travail uniquement. Pour charges de travail virtualisées et autres hautes performances aléatoires, nous vous recommandons d’utiliser des miroirs triples pour de meilleures performances.

- **Opérations de machine virtuelle plus rapides**. ReFS introduit de nouvelles fonctionnalités dont le principal objectif est d’améliorer les performances des charges de travail virtualisées :
    - [Clonage de bloc](./block-cloning.md) : cette fonctionnalité rend les opérations de copie plus rapides, en permettant d’effectuer rapidement et avec peu d’impact les opérations de fusion des points de contrôle de machine virtuelle.
    - VDL fragmenté : cette fonctionnalité permet à ReFS de remettre rapidement à zéro les fichiers, réduisant ainsi le temps nécessaire pour créer des disques durs virtuels de taille fixe de quelques dizaines de minutes à quelques secondes seulement.

- **Tailles de cluster variables** : ReFS prend en charge les tailles de cluster de 4 Ko et 64 Ko. Pour la plupart des déploiements, nous vous recommandons d’utiliser une taille de cluster de 4 Ko. Les clusters de 64 Ko sont appropriés pour les charges de travail d’E/S séquentielles volumineuses.

### <a name="scalability"></a>Extensibilité

ReFS est conçu pour prendre en charge des jeux de données extrêmement volumineux (millions de téraoctets) sans baisse des performances, offrant ainsi une plus grande extensibilité que les systèmes de fichiers antérieurs.

## <a name="supported-deployments"></a>Déploiements pris en charge

Microsoft a développé NTFS spécifiquement pour une utilisation à usage général avec un large éventail de configurations et les charges de travail, mais pour les clients spécialement nécessitant la disponibilité, la résilience, et/ou mise à l’échelle qui fournit des références, Microsoft prend en charge les références pour une utilisation sous les configurations et scénarios suivants. 

> [!NOTE]
> Toutes les configurations prises en charge de ReFS doivent utiliser [catalogue Windows Server](https://www.WindowsServerCatalog.com) certifié de matériel et répondre aux exigences des applications.

### <a name="storage-spaces-direct"></a>Espaces de stockage directs

Le déploiement de ReFS sur des espaces de stockage direct est recommandé pour les charges de travail virtualisées ou le stockage connecté au réseau (NAS) : 
- La parité accélérée grâce à la mise en miroir et [le cache dans les espaces de stockage direct](../storage-spaces/understand-the-cache.md) offrent des performances élevées ainsi qu’un stockage de haute capacité. 
- Le clonage de bloc et le VDL fragmenté accélèrent considérablement les opérations sur les fichiers .vhdx, telles que la création, la fusion et l’extension.
- Flux d’intégrité, de réparation en ligne et de copies de données alternatif activent ReFS et espaces de stockage Direct à conjointement pour détecter et corriger le contrôleur de stockage et les altérations de support de stockage dans les métadonnées et données. 
- ReFS fournit les fonctionnalités nécessaires pour s’adapter et prendre en charge les jeux de données volumineux. 

### <a name="storage-spaces"></a>Espaces de stockage

- Flux d’intégrité, de réparation en ligne et de copies de données alternatif activent ReFS et [espaces de stockage](../storage-spaces/overview.md) à conjointement pour détecter et corriger le contrôleur de stockage et les altérations de support de stockage dans les métadonnées et données.
- Les déploiements d’espaces de stockage peuvent également utiliser le clonage de bloc et l’évolutivité proposés dans ReFS.
- Le déploiement de références sur les espaces de stockage avec des boîtiers SAS partagés est approprié pour l’hébergement des données d’archivage et de stocker des documents de l’utilisateur.

> [!NOTE]
> Prend en charge de stockage espaces local non amovible en attachée direct via BusTypes SATA, SAS, NVME ou rattachés au moyen de HBA (également appelé contrôleur RAID en mode Pass-Through).

### <a name="basic-disks"></a>Disques de base

Déploiement de références sur les disques de base convient le mieux pour les applications qui implémentent leurs propres solutions de résilience et disponibilité des logiciels. 
- Les applications qui introduisent leurs propres solutions logicielles de résilience et de disponibilité peuvent exploiter les flux d’intégrité, le clonage de bloc et la capacité de dimensionner et prendre en charge les jeux de données volumineux. 

> [!NOTE]
> Les disques de base incluent local non amovible en attachement direct via BusTypes SATA, SAS, NVME ou RAID. 

### <a name="backup-target"></a>Cible de sauvegarde

Déploiement ReFS comme cible de sauvegarde est meilleure adapté aux applications et le matériel qui implémentent leurs propres solutions de résilience et de disponibilité.
- Les applications qui introduisent leurs propres solutions logicielles de résilience et de disponibilité peuvent exploiter les flux d’intégrité, le clonage de bloc et la capacité de dimensionner et prendre en charge les jeux de données volumineux.

> [!NOTE]
> Cibles de sauvegarde incluent les configurations prises en charge ci-dessus. Veuillez contacter les fournisseurs de baies de stockage et d’application pour plus d’informations de prise en charge sur Fibre Channel et SAN iSCSI. Pour les réseaux SAN, si les fonctionnalités telles que mince approvisionnement, TRIM/UNMAP ou transfert de données déchargées (ODX) sont requises, NTFS doit être utilisé.   

## <a name="feature-comparison"></a>Comparaison des fonctionnalités

### <a name="limits"></a>Limites

| Fonctionnalité       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Longueur maximale des noms de fichier | 255 caractères Unicode  | 255 caractères Unicode               |
| Longueur maximale des chemins |32 000 caractères Unicode | 32 000 caractères Unicode                |
| Taille de fichier maximale | 35 Po (pétaoctets)  | 256 To               |
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
| / Annuler le mappage de Trim | Oui<sup>3</sup> | Oui |
1. Disponible sur Windows Server, version 1709 et versions ultérieure.
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
| Prise en charge des fichiers de page | Non | Oui |
| Prise en charge sur les médias amovibles | Non | Oui |

## <a name="see-also"></a>Voir aussi

-   [Vue d’ensemble Direct des espaces de stockage](../storage-spaces/storage-spaces-direct-overview.md)
-   [Clonage de bloc reFS](block-cloning.md)
-   [Flux d’intégrité (Refs)](integrity-streams.md)