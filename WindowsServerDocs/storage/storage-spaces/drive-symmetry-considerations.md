---
title: Considérations relatives à la symétrie lecteur pour les espaces de stockage Direct
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866880"
---
# <a name="drive-symmetry-considerations-for-storage-spaces-direct"></a>Considérations relatives à la symétrie lecteur pour les espaces de stockage Direct 

> S’applique à : Windows Server 2019, Windows Server 2016

[Espaces de stockage Direct](storage-spaces-direct-overview.md) fonctionne mieux lorsque chaque serveur possède exactement les mêmes lecteurs.

En réalité, nous reconnaissons que cela n’est pas toujours pratique : Espaces de stockage Direct est conçu pour s’exécuter pendant des années et à l’échelle selon l’évoluent des besoins de votre organisation. Aujourd'hui, vous pourrez acheter 3 To spacieux des disques durs ; année suivante, il peut devenir impossible de trouver celles qui petit. Par conséquent, une certaine quantité d’et associations est pris en charge.

Cette rubrique explique les contraintes et fournit des exemples de configurations prises en charge et non pris en charge.

## <a name="constraints"></a>Contraintes

### <a name="type"></a>Type

Tous les serveurs doivent avoir le même [types de lecteurs](choosing-drives.md#drive-types).

Par exemple, si un serveur a NVMe, ils le devraient *tous les* ont NVMe.

### <a name="number"></a>Numéro

Tous les serveurs doivent avoir le même nombre de lecteurs de chaque type.

Par exemple, si un serveur a six SSD, ils le devraient *tous les* avoir six SSD.

   > [!NOTE]
   > Il est OK pour le nombre de lecteurs pour différer temporairement lors de pannes ou tout ajout ou suppression de lecteurs.

### <a name="model"></a>Modèle

Nous vous recommandons d’utiliser des lecteurs du même modèle et la version de microprogramme autant que possible. Si vous ne pouvez pas, sélectionnez avec soin les lecteurs qui sont aussi semblables que possible. Nous déconseillons les lecteurs et associations du même type avec des caractéristiques de performances ou de résistance nettement différentes (sauf si un est le cache et l’autre est la capacité), car les espaces de stockage Direct distribue uniformément les e/s et ne celle-ci de discriminer selon de modèle .

   > [!NOTE]
   > Il est OK pour mélanger et les disques SATA et SAS similaire.

### <a name="size"></a>Size

Nous recommandons d’utiliser des disques de la même taille de chaque fois que possible. À l’aide de disques de la capacité de différentes tailles peut-être entraîner dans une certaine mesure inutilisable, et à l’aide de différentes tailles, les lecteurs de cache n’améliore pas les performances du cache. Consultez la section suivante pour plus d’informations.

   > [!WARNING]
   > Différentes tailles de disques de capacité entre les serveurs peuvent entraîner des capacités inutilisées.

## <a name="understand-capacity-imbalance"></a>Comprendre : déséquilibre de capacité

Espaces de stockage Direct est robuste déséquilibre de la capacité sur les lecteurs et les serveurs. Même si le déséquilibre est grave, tout continue de fonctionner. Toutefois, selon plusieurs facteurs, capacité qui n’est pas disponible dans chaque serveur peut-être pas utilisable.

Pour voir pourquoi cela se produit, examinez l’illustration simplifiée. Chaque zone colorée représente une seule copie de données mise en miroir. Par exemple, les zones marquée A, A » et un '' sont trois copies des mêmes données. Pour respecter la tolérance de panne du serveur, ces copies *doit* être stockées dans des serveurs différents.

### <a name="stranded-capacity"></a>Capacités inutilisées

Lors de son dessin, le serveur 1 (10 To) et 2 (10 To) sont saturées. Serveur 3 dispose de disques plus volumineux, par conséquent sa capacité totale est supérieure (15 To). Toutefois, pour stocker davantage de données en miroir triple sur serveur 3 nécessiterait des copies sur le serveur 1 et 2 du serveur trop, qui sont déjà complet. Impossible d’utiliser la capacité de 5 to restante sur le serveur 3 – il s’agit *inutilisés » »* capacité.

![Miroir triple, trois serveurs, en échec la capacité](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### <a name="optimal-placement"></a>Placement optimal

À l’inverse, avec quatre serveurs de 10 To, 10 To, 10 To et 15 To de capacité et la résilience en miroir triple, il *est* possible de placer correctement les copies d’une manière qui utilise toute la capacité disponible, comme dessiné. Chaque fois que cela soit possible, l’allocateur d’espaces de stockage Direct recherchera et utiliser le placement optimal, en laissant aucune capacité inutilisée.

![Miroir triple, quatre serveurs, aucune capacité inutilisée](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

Le nombre de serveurs, la résilience, la gravité de déséquilibre de la capacité et d’autres facteurs affecte s’il y a des capacités inutilisées. **La règle générale plus prudente consiste à supposer que seule la capacité disponible dans chaque serveur est garantie pour être utilisable.**

## <a name="understand-cache-imbalance"></a>Comprendre : déséquilibre de cache

Espaces de stockage Direct est robuste déséquilibre de cache sur les lecteurs et les serveurs. Même si le déséquilibre est grave, tout continue de fonctionner. En outre, espaces de stockage Direct utilise toujours tous les disponible dans le cache de manière optimale.

Toutefois, à l’aide de différentes tailles, les lecteurs de cache n’améliore pas les performances du cache uniformément ou de manière prévisible : uniquement les e/s à [lecteur liaisons](understand-the-cache.md#server-side-architecture) lecteurs peuvent constater de meilleures performances avec la taille du cache. Espaces de stockage Direct distribue uniformément les e/s entre les liaisons et ne celle-ci de discriminer selon les taux de capacité maximale du cache.

![Déséquilibre du cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Consultez [présentation du cache](understand-the-cache.md) pour en savoir plus sur les liaisons de cache.

## <a name="example-configurations"></a>Exemples de configuration

Voici certaines configurations prises en charge et non pris en charge :

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-between-servers"></a>![prise en charge](media/drive-symmetry-considerations/supported.png) Prise en charge : des modèles différents entre les serveurs

Les deux premiers serveurs utilisent NVMe modèle « X », mais le troisième serveur utilise le modèle NVMe « Z », qui est très similaire.

| Serveur 1                    | Serveur 2                    | Serveur 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modèle X (cache)    | 2 x NVMe modèle X (cache)    | 2 x NVMe modèle Z (cache)    |
| 10 x SSD modèle Y (capacité) | 10 x SSD modèle Y (capacité) | 10 x SSD modèle Y (capacité) |

Cela est pris en charge.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-models-within-server"></a>![prise en charge](media/drive-symmetry-considerations/supported.png) Prise en charge : des modèles différents au sein du serveur

Chaque serveur utilise une combinaison différente de modèles HDD « Y » et « Z », qui sont très similaires. Chaque serveur a 10 HDD total.

| Serveur 1                   | Serveur 2                   | Serveur 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modèle X (cache)    | 2 x SSD modèle X (cache)    | 2 x SSD modèle X (cache)    |
| 7 x HDD modèle Y (capacité) | 5 x HDD modèle Y (capacité) | 1 x HDD modèle Y (capacité) |
| 3 x HDD modèle Z (capacité) | 5 x HDD modèle Z (capacité) | 9 x HDD modèle Z (capacité) |

Cela est pris en charge.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-across-servers"></a>![prise en charge](media/drive-symmetry-considerations/supported.png) Prise en charge : des tailles différentes entre les serveurs

Les deux premiers serveurs utilisent des disques durs de 4 To, mais le troisième serveur utilise très similaire 6 To du disque dur.

| Serveur 1                | Serveur 2                | Serveur 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 Go NVMe (cache) | 2 x 800 Go NVMe (cache) | 2 x 800 Go NVMe (cache) |
| 4 x 4 To HDD (capacité) | 4 x 4 To HDD (capacité) | 4 x 6 To HDD (capacité) |

Cela est pris en charge, bien que cela entraînerait des capacités inutilisées.

### <a name="supportedmediadrive-symmetry-considerationssupportedpng-supported-different-sizes-within-server"></a>![prise en charge](media/drive-symmetry-considerations/supported.png) Prise en charge : des tailles différentes au sein du serveur

Chaque serveur utilise une combinaison différente de 1,2 To et très similaire 1,6 To SSD. Chaque serveur a 4 SSD total.

| Serveur 1                 | Serveur 2                 | Serveur 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1,2 To SSD (cache)   | 2 x 1,2 To SSD (cache)   | 4 x 1,2 To SSD (cache)   |
| 1 1,6 To SSD (cache)   | 2 x 1.6 To SSD (cache)   | -                        |
| 20 x 4 To HDD (capacité) | 20 x 4 To HDD (capacité) | 20 x 4 To HDD (capacité) |

Cela est pris en charge.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-types-of-drives-across-servers"></a>![absence de prise en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge : différents types de disques entre serveurs

Le serveur 1 a NVMe, mais les autres ne.

| Serveur 1            | Serveur 2            | Serveur 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| 18 x HDD (capacité) | 18 x HDD (capacité) | 18 x HDD (capacité) |

Cela n’est pas pris en charge. Les types de lecteurs doivent être identiques dans chaque serveur.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-different-number-of-each-type-across-servers"></a>![absence de prise en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge : un nombre différent de chaque type entre les serveurs

Serveur 3 a davantage de lecteurs que les autres.

| Serveur 1            | Serveur 2            | Serveur 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 x HDD (capacité) | 10 x HDD (capacité) | 20 x HDD (capacité) |

Cela n’est pas pris en charge. Le nombre de lecteurs de chaque type doit être identique dans chaque serveur.

### <a name="unsupportedmediadrive-symmetry-considerationsunsupportedpng-not-supported-only-hdd-drives"></a>![absence de prise en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge : seuls les lecteurs de disque dur

Tous les serveurs possèdent uniquement les lecteurs de disques durs connectés.

|Serveur 1|Serveur 2|Serveur 3|
|-|-|-| 
|18 x HDD (capacité) |18 x HDD (capacité)|18 x HDD (capacité)|

Cela n’est pas pris en charge. Vous devez ajouter un minimum de deux lecteurs de cache (NvME ou SSD) attachés à chacun des serveurs.

## <a name="summary"></a>Récapitulatif

Pour résumer, chaque serveur dans le cluster doit avoir les mêmes types de disques et le même nombre de chaque type. Il est pris en charge pour les modèles de lecteur de mélanger et et tailles de lecteur en fonction des besoins, avec les considérations ci-dessus.

| contrainte                               |               |
|------------------------------------------|---------------|
| Mêmes types de disques dans chaque serveur     | **Obligatoire**  |
| Même nombre de chaque type dans chaque serveur | **Obligatoire**  |
| Modèles de lecteur même dans chaque serveur        | Recommandé   |
| Tailles de lecteur même dans chaque serveur         | Recommandé   |

## <a name="see-also"></a>Voir aussi

- [Configuration matérielle directe des espaces de stockage](storage-spaces-direct-hardware-requirements.md)
- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
