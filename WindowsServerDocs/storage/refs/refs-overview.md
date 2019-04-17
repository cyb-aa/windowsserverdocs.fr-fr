---
title: "Vue d’ensemble du système de fichiers résilient (ReFS)"
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: dmoss
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 1/3/2016
ms.assetid: 
ms.openlocfilehash: 4460df978f2a3d5e30e43b82cf952ed37f3d91a9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="resilient-file-system-refs-overview"></a>Vue d’ensemble du système de fichiers résilient (ReFS)
>S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

ReFS (Resilient File System) est le dernier système de fichiers de Microsoft. Il a été conçu pour optimiser la disponibilité des données, s’adapter efficacement aux jeux de données volumineux sur diverses charges de travail et garantir l’intégrité des données grâce à sa capacité de résilience face aux endommagements. Ce système a vocation à prendre en charge davantage de scénarios de stockage et à constituer une base pour les innovations futures. 

## <a name="key-benefits"></a>Principaux avantages

### <a name="resiliency"></a>Résilience 
ReFS fournit de nouvelles fonctionnalités qui permettent de détecter avec précision les endommagements, mais aussi de les corriger en ligne, ce qui contribue à améliorer l’intégrité et la disponibilité de vos données: 

- **Flux d’intégrité**. ReFS utilise des sommes de contrôle pour les métadonnées et, éventuellement, pour les données de fichier, ce qui lui permet de détecter les endommagements de manière fiable. 
- **Intégration aux espaces de stockage**. Quand ReFS est utilisé en association avec un espace en miroir ou de parité, il peut automatiquement réparer les endommagements détectés à l’aide de la copie des données fournie par les espaces de stockage. Les processus de réparation sont effectués en ligne, sur la zone endommagée, et n’entraînent aucune inactivité du volume.
- **Récupération des données**. Si un volume est endommagé et qu’il n’existe pas de copie des données endommagées, ReFS supprime les données endommagées dans l’espace de noms. ReFS peut traiter la plupart des endommagements non réparables en maintenant le volume en ligne, mais il doit mettre le volume hors connexion dans de rares cas.
- **Correction proactive des erreurs**. En plus de valider les données avant les écritures et les lectures, ReFS utilise maintenant un scanneur d’intégrité des données, appelé <i>programme de nettoyage</i>. Ce programme de nettoyage analyse régulièrement le volume pour identifier les endommagements latents et déclencher de manière proactive la réparation des données endommagées. 


### <a name="performance"></a>Performances
En plus des améliorations apportées à la résilience, ReFS propose de nouvelles fonctionnalités pour les charges de travail virtualisées et dépendantes des performances. L’optimisation de niveau en temps réel, le clonage de bloc et le VDL fragmenté sont des exemples de nouvelles fonctionnalités du système ReFS conçues pour prendre en charge diverses charges de travail dynamiques:

- **[Parité accélérée grâce à la mise en miroir](./mirror-accelerated-parity.md)**. La parité accélérée grâce à la mise en miroir offre des performances élevées ainsi qu’un stockage de haute capacité pour vos données. 

    - Pour offrir des performances élevées ainsi qu’un stockage de haute capacité, ReFS divise un volume en deux groupes de stockage logiques appelés niveaux. Ces niveaux peuvent avoir des types de disque et de résilience qui leur sont propres, ce qui permet à chaque niveau d’optimiser les performances ou la capacité. Voici des exemples de configurations: 
    
    
    | Niveau de performances | Niveau de capacité |
    |----------------|-----------------|
     SSD en miroir | HDD en miroir |
     SSD en miroir | SSD de parité |
     SSD en miroir | HDD de parité |   
            
    - Une fois que ces niveaux sont configurés, ReFS les utilise pour fournir un stockage rapide pour les données actives («hot») et un stockage de haute capacité pour les données passives («cold»):
        - Toutes les écritures sont effectuées dans le niveau des performances. Les blocs de données volumineux qui restent dans ce niveau sont transférés en temps réel dans le niveau de capacité.
        - Dans un [déploiement hybride](../storage-spaces/choosing-drives.md) (combinant des lecteurs flash et des disques HDD), [le cache dans les espaces de stockage direct](../storage-spaces/understand-the-cache.md) contribue à accélérer les lectures, en limitant l’impact de la fragmentation des données propre aux charges de travail virtualisées. Dans un déploiement avec uniquement des lecteurs flash, les lectures s’effectuent également dans le niveau de performances. 

>[!NOTE]
>Pour les déploiements de serveur, la parité accélérée grâce à la mise en miroir est uniquement prise en charge par les [espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md).

- **Opérations d’ordinateurs virtuels plus rapides**. ReFS introduit de nouvelles fonctionnalités dont le principal objectif est d’améliorer les performances des charges de travail virtualisées:
    - [Clonage de bloc](./block-cloning.md): cette fonctionnalité rend les opérations de copie plus rapides, en permettant d’effectuer rapidement et avec peu d’impact les opérations de fusion des points de contrôle de machine virtuelle. 
    - VDL fragmenté: cette fonctionnalité permet à ReFS de remettre rapidement à zéro les fichiers, réduisant ainsi le temps nécessaire pour créer des disques durs virtuels de taille fixe de quelques dizaines de minutes à quelques secondes seulement.


- **Tailles de cluster variables**: ReFS prend en charge les tailles de cluster de 4Ko et 64Ko. Pour la plupart des déploiements, nous vous recommandons d’utiliser une taille de cluster de 4Ko. Les clusters de 64Ko sont appropriés pour les charges de travail d’E/S séquentielles volumineuses.
    
    
### **<a name="scalability"></a>Extensibilité**
ReFS est conçu pour prendre en charge des jeux de données extrêmement volumineux (millions de téraoctets) sans baisse des performances, offrant ainsi une plus grande extensibilité que les systèmes de fichiers antérieurs. 

## <a name="supported-deployments"></a>Déploiements pris en charge

### <a name="storage-spaces-direct"></a>Espaces de stockage direct ###

Le déploiement de ReFS sur des espaces de stockage direct est recommandé pour les charges de travail virtualisées ou le stockage connecté au réseau (NAS): 
- La parité accélérée grâce à la mise en miroir et [le cache dans les espaces de stockage direct](../storage-spaces/understand-the-cache.md) offrent des performances élevées ainsi qu’un stockage de haute capacité. 
- Le clonage de bloc et le VDL fragmenté accélèrent considérablement les opérations sur les fichiers.vhdx, telles que la création, la fusion et l’extension.
- Les flux d’intégrité, les opérations de réparation en ligne et les copies de données permettent à ReFS et aux espaces de stockage direct de détecter et réparer conjointement les métadonnées et les données endommagées. 
- ReFS fournit les fonctionnalités nécessaires pour s’adapter et prendre en charge les jeux de données volumineux. 

### <a name="storage-spaces-with-sas-drive-enclosures"></a>Espaces de stockage avec boîtiers de disque SAS ###
Le déploiement de ReFS sur les espaces de stockage avec des boîtiers SAS partagés permet l’hébergement des données d’archive et le stockage des documents utilisateur:
- Les flux d’intégrité, les opérations de réparation en ligne et les copies de données permettent à ReFS et aux espaces de stockage de détecter et réparer conjointement les métadonnées et les données endommagées. 
- Les déploiements d’espaces de stockage peuvent également utiliser le clonage de bloc et l’évolutivité proposés dans ReFS.

### <a name="basic-disks"></a>Disques de base ###
Le déploiement de ReFS sur des disques de base convient mieux aux applications qui implémentent leurs propres solutions logicielles de résilience et de disponibilité. 
- Les applications qui introduisent leurs propres solutions logicielles de résilience et de disponibilité peuvent exploiter les flux d’intégrité, le clonage de bloc et la capacité de dimensionner et prendre en charge les jeux de données volumineux. 

>[!NOTE]
>ReFS n’est pas pris en charge sur le stockage connecté au réseau (SAN).


## <a name="feature-comparison"></a>Comparaison des fonctionnalités

### <a name="limits"></a>Limites

| Fonctionnalité       | ReFS                                        | NTFS |
|----------------|------------------------------------------------|-----------------------|
| Longueur maximale des noms de fichier | 255caractères Unicode  | 255caractères Unicode               |
| Longueur maximale des chemins |32000caractères Unicode | 32000caractères Unicode                |
| Taille de fichier maximale | 18Eo (exaoctets)  | 18Eo (exaoctets)                |
| Taille de volume maximale | 4,7Zo (zettaoctets)                           | 256To                |


### <a name="functionality"></a>Fonctionnalité

#### <a name="the-following-features-are-available-on-refs-and-ntfs"></a>Les fonctionnalités suivantes sont disponibles sur ReFS et NTFS:

| Fonctionnalité       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Chiffrement BitLocker | Oui | Oui |
| Prise en charge des volumes CSV (volumes partagés de cluster) | Oui | Oui |
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

#### <a name="the-following-features-are-only-available-on-refs"></a>Les fonctionnalités suivantes sont uniquement disponibles sur ReFS:

| Fonctionnalité       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Clonage de bloc | Oui | Non |
| VDL fragmenté | Oui | Non |
| Parité accélérée grâce à la mise en miroir| Oui (sur les espaces de stockage direct) | Non |

#### <a name="the-following-features-are-unavailable-on-refs-at-this-time"></a>Les fonctionnalités suivantes ne sont pas actuellement disponibles sur ReFS:

| Fonctionnalité       | ReFS                                        | NTFS |
|---------------------------|------------------|-----------------------|
| Compression de système de fichiers | Non | Oui |
| Chiffrement du système de fichiers | Non | Oui |
| Déduplication des données | Non | Oui |
| Transactions | Non | Oui |
| Liens physiques | Non | Oui |
| ID d’objet | Non | Oui |
| Noms courts | Non | Oui |
| Attributs étendus | Non | Oui |
| Quotas de disque | Non | Oui |
| Démarrable | Non | Oui |
| Prise en charge sur les médias amovibles | Non | Oui |
| Niveaux de stockage NTFS | Non | Oui |


## <a name="see-also"></a>Voir aussi

-   [Clonage de bloc sur ReFS](block-cloning.md)
-   [Flux d’intégrité ReFS](integrity-streams.md)
-   [Présentation des espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)
