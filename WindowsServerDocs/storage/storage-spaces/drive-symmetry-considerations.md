---
title: Considérations relatives à la symétrie de lecteur pour espaces de stockage direct
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
ms.localizationpriority: medium
ms.openlocfilehash: b06d69c020ea38a2fb9f23df2cfd9cd4191ae315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857552"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Considérations relatives à la symétrie de lecteur pour espaces de stockage direct 

> S’applique à : Windows Server 2019, Windows Server 2016

[Espaces de stockage direct](storage-spaces-direct-overview.md) fonctionne mieux lorsque chaque serveur a exactement les mêmes lecteurs.

En réalité, nous reconnaissons que cela n’est pas toujours pratique : espaces de stockage direct est conçu pour s’exécuter pendant des années et pour évoluer à mesure que les besoins de votre organisation évoluent. Aujourd’hui, vous pouvez acheter des disques durs spacieux de 3 to. l’année prochaine, il sera peut-être impossible de trouver ceux qui sont de petite taille. Par conséquent, un certain nombre de combinaisons est pris en charge.

Cette rubrique explique les contraintes et fournit des exemples de configurations prises en charge et non prises en charge.

## <a name="constraints"></a>Contraintes

### <a name="type"></a>Type

Tous les serveurs doivent avoir les mêmes [types de lecteurs](choosing-drives.md#drive-types).

Par exemple, si un serveur a NVMe, ils doivent *tous* avoir nVMe.

### <a name="number"></a>Numéro

Tous les serveurs doivent avoir le même nombre de lecteurs de chaque type.

Par exemple, si un serveur possède six disques SSD, ils doivent *tous* avoir six disques SSD.

   > [!NOTE]
   > Il est possible que le nombre de lecteurs diffère temporairement pendant les défaillances ou lors de l’ajout ou de la suppression de lecteurs.

### <a name="model"></a>Modèle

Nous vous recommandons d’utiliser des lecteurs du même modèle et de la même version du microprogramme chaque fois que cela est possible. Si vous ne le pouvez pas, sélectionnez soigneusement les lecteurs qui sont aussi similaires que possible. Nous déconseillons de mélanger et de mettre en correspondance les lecteurs du même type avec des caractéristiques de performances ou d’endurance nettes (sauf si l’une d’elles est mise en cache et l’autre la capacité) car espaces de stockage direct distribue les e/s uniformément et ne fait pas de discrimination basée sur le modèle.

   > [!NOTE]
   > Il est possible de mélanger et de faire correspondre des lecteurs SATA et SAS similaires.

### <a name="size"></a>Taille

Nous vous recommandons d’utiliser des lecteurs de même taille chaque fois que cela est possible. L’utilisation de disques de capacité de différentes tailles peut entraîner une capacité inutilisable et l’utilisation de lecteurs de cache de différentes tailles peut ne pas améliorer les performances du cache. Pour plus d’informations, consultez la section suivante.

   > [!WARNING]
   > Des tailles de lecteurs de capacité différente sur plusieurs serveurs peuvent entraîner une capacité inutilisée.

## <a name="understand-capacity-imbalance"></a>Comprendre : déséquilibre de la capacité

Espaces de stockage direct est robuste pour le déséquilibre de la capacité entre les lecteurs et entre les serveurs. Même si le déséquilibre est grave, tout va continuer à fonctionner. Toutefois, selon plusieurs facteurs, la capacité qui n’est pas disponible dans chaque serveur n’est peut-être pas utilisable.

Pour comprendre pourquoi cela se produit, considérez l’illustration simplifiée ci-dessous. Chaque zone de couleur représente une copie de données mises en miroir. Par exemple, les zones marquées A, A’et' 'sont trois copies des mêmes données. Pour honorer la tolérance aux pannes du serveur, ces copies *doivent* être stockées sur des serveurs différents.

### <a name="stranded-capacity"></a>Capacité bloquée

Comme dessiné, le serveur 1 (10 to) et le serveur 2 (10 to) sont pleins. Le serveur 3 possède des lecteurs plus volumineux, donc sa capacité totale est supérieure (15 to). Toutefois, pour stocker des données miroir triples sur le serveur 3, vous devez également copier sur le serveur 1 et sur le serveur 2, ce qui est déjà plein. La capacité restante de 5 to sur le serveur 3 ne peut pas être utilisée, il s’agit d’une capacité *« bloquée »* .

![Miroir triple, trois serveurs, capacité bloquée](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Positionnement optimal

À l’inverse, avec quatre serveurs de 10 to, 10 to, 10 to, et 15 to et une résilience en miroir triple, il est possible de placer les copies de manière valide en utilisant toute la capacité disponible, telle qu’elle *est* dessinée. Chaque fois que cela est possible, l’allocateur espaces de stockage direct trouvera et utilisera le positionnement optimal, sans aucune capacité inutilisée.

![Miroir triple, quatre serveurs, aucune capacité bloquée](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

Le nombre de serveurs, la résilience, la gravité du déséquilibre de la capacité et d’autres facteurs déterminent s’il existe une capacité bloquée. **La règle générale la plus prudente consiste à supposer que seule la capacité disponible dans chaque serveur est toujours utilisable.**

## <a name="understand-cache-imbalance"></a>Comprendre : déséquilibre du cache

Espaces de stockage direct est robuste pour mettre en cache le déséquilibre entre les lecteurs et entre les serveurs. Même si le déséquilibre est grave, tout va continuer à fonctionner. En outre, espaces de stockage direct utilise toujours tout le cache disponible à la plus complète.

Toutefois, l’utilisation de lecteurs de cache de différentes tailles peut ne pas améliorer les performances de cache de façon uniforme ou prévisible : seules les [liaisons](understand-the-cache.md#server-side-architecture) d’e/s avec des lecteurs de cache plus importants peuvent améliorer les performances. Espaces de stockage direct distribue les e/s uniformément entre les liaisons et ne discrimine pas en fonction du rapport entre le cache et la capacité.

![Déséquilibre du cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Pour en savoir plus sur les liaisons de cache, consultez [Présentation du cache](understand-the-cache.md) .

## <a name="example-configurations"></a>Exemples de configurations

Voici quelques-unes des configurations prises en charge et non prises en charge :

### <a name="supported-supported-different-models-between-servers"></a>![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge : différents modèles entre les serveurs

Les deux premiers serveurs utilisent le modèle NVMe « X », mais le troisième serveur utilise le modèle NVMe « Z », qui est très similaire.

| Serveur 1                    | Serveur 2                    | Serveur 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x modèle NVMe X (cache)    | 2 x modèle NVMe X (cache)    | 2 x modèle NVMe Z (cache)    |
| 10 x modèle SSD Y (capacité) | 10 x modèle SSD Y (capacité) | 10 x modèle SSD Y (capacité) |

Cette topologie est prise en charge.

### <a name="supported-supported-different-models-within-server"></a>![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge : différents modèles au sein du serveur

Chaque serveur utilise une combinaison différente de modèles HDD « Y » et « Z », qui sont très similaires. Chaque serveur dispose de 10 disques durs au total.

| Serveur 1                   | Serveur 2                   | Serveur 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x modèle SSD x (cache)    | 2 x modèle SSD x (cache)    | 2 x modèle SSD x (cache)    |
| 7 x modèle de disque dur Y (capacité) | 5 x modèle de disque dur Y (capacité) | 1 x modèle de disque dur Y (capacité) |
| 3 x modèle de disque dur Z (capacité) | 5 x modèle de disque dur Z (capacité) | 9 x modèle de disque dur Z (capacité) |

Cette topologie est prise en charge.

### <a name="supported-supported-different-sizes-across-servers"></a>![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge : différentes tailles sur plusieurs serveurs

Les deux premiers serveurs utilisent un disque dur de 4 to, mais le troisième serveur utilise un disque dur très similaire de 6 to.

| Serveur 1                | Serveur 2                | Serveur 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 Go NVMe (cache) | 2 x 800 Go NVMe (cache) | 2 x 800 Go NVMe (cache) |
| disques durs 4 x 4 to (capacité) | disques durs 4 x 4 to (capacité) | disque dur de 4 x 6 to (capacité) |

Cela est pris en charge, même si cela se traduira par une capacité inutilisée.

### <a name="supported-supported-different-sizes-within-server"></a>![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge : différentes tailles au sein du serveur

Chaque serveur utilise une combinaison différente de 1,2 to et de disques SSD de 1,6 to similaires. Chaque serveur a 4 disques SSD au total.

| Serveur 1                 | Serveur 2                 | Serveur 3                 |
|--------------------------|--------------------------|--------------------------|
| disque SSD de 3 x 1,2 to (cache)   | 2 x 1,2 to SSD (cache)   | 4 disques SSD 1,2 to (cache)   |
| disques SSD de 1 à 1,6 to (cache)   | 2 x 1,6 to SSD (cache)   | -                        |
| 20 disques durs de 4 to (capacité) | 20 disques durs de 4 to (capacité) | 20 disques durs de 4 to (capacité) |

Cette topologie est prise en charge.

### <a name="unsupported-not-supported-different-types-of-drives-across-servers"></a>![non pris en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge : différents types de lecteurs sur les serveurs

Le serveur 1 a NVMe, mais les autres ne le sont pas.

| Serveur 1            | Serveur 2            | Serveur 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | SSD 6 x (cache)     | SSD 6 x (cache)     |
| 18 x HDD (capacité) | 18 x HDD (capacité) | 18 x HDD (capacité) |

Cela n’est pas pris en charge. Les types de lecteurs doivent être identiques sur chaque serveur.

### <a name="unsupported-not-supported-different-number-of-each-type-across-servers"></a>![non pris en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge : nombre différent de chaque type sur plusieurs serveurs

Le serveur 3 contient plus de lecteurs que les autres.

| Serveur 1            | Serveur 2            | Serveur 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 disques durs (capacité) | 10 disques durs (capacité) | 20 disques durs (capacité) |

Cela n’est pas pris en charge. Le nombre de lecteurs de chaque type doit être le même sur chaque serveur.

### <a name="unsupported-not-supported-only-hdd-drives"></a>![non pris en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge : seuls les lecteurs HDD

Seuls les lecteurs HDD sont connectés à tous les serveurs.

|Serveur 1|Serveur 2|Serveur 3|
|-|-|-| 
|18 x HDD (capacité) |18 x HDD (capacité)|18 x HDD (capacité)|

Cela n’est pas pris en charge. Vous devez ajouter au moins deux lecteurs de cache (NvME ou SSD) attachés à chacun des serveurs.

## <a name="summary"></a>Résumé

Pour résumer, chaque serveur du cluster doit avoir les mêmes types de lecteurs et le même nombre de chaque type. Il est possible de mélanger et de faire correspondre les modèles de lecteur et les tailles de lecteur en fonction des besoins, avec les considérations ci-dessus.

| Restrictions                               |               |
|------------------------------------------|---------------|
| Mêmes types de lecteurs dans chaque serveur     | **Obligatoire**  |
| Même numéro de chaque type dans chaque serveur | **Obligatoire**  |
| Mêmes modèles de lecteur dans chaque serveur        | Recommandé   |
| Mêmes tailles de lecteur sur chaque serveur         | Recommandé   |

## <a name="see-also"></a>Voir aussi

- [espaces de stockage direct configuration matérielle requise](storage-spaces-direct-hardware-requirements.md)
- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
