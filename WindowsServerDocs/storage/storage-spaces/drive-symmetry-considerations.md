---
title: Considérations relatives à la symétrie lecteur pour les espaces de stockage Direct
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/08/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 629e49a0c1919286d8e4f418b3e99d69e720f4fd
ms.sourcegitcommit: f2ef58003da6de049c7c4b578f789a97e0a0f512
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5591845"
---
# Considérations relatives à la symétrie lecteur pour les espaces de stockage Direct 

> S’applique à: Windows Server 2019, Windows Server 2016

[Espaces de stockage Direct](storage-spaces-direct-overview.md) fonctionne de manière optimale lorsque chaque serveur possède exactement les mêmes lecteurs.

En réalité, nous savons cela n’est pas toujours pratique: espaces de stockage Direct est conçu pour exécuter des années et à l’échelle en les besoins de votre organisation. Aujourd'hui, vous pouvez acheter spacieux 3 To des disques durs; année suivante, il peut s’avérer impossible, recherchez celles petit. Par conséquent, un certain mixage-et-mise en correspondance est pris en charge.

Cette rubrique explique les contraintes et fournit des exemples de configurations prises en charge et non pris en charge.

## Contraintes

### Type

Tous les serveurs doivent avoir les mêmes [types de disques](choosing-drives.md#drive-types).

Par exemple, si un serveur dispose de disques NVMe, elles doivent *toutes* avoir NVMe.

### Nombre

Tous les serveurs doivent avoir le même nombre de disques de chaque type.

Par exemple, si un serveur dispose de six SSD, elles doivent *toutes* avoir six SSD.

   > [!NOTE]
   > Il est OK pour le nombre de disques de différer temporairement pendant les défaillances ou lors de l’ajout ou la suppression des lecteurs.

### Modèle

Nous recommandons l’utilisation de disques du même modèle et la version du microprogramme autant que possible. Si vous ne pouvez pas, sélectionnez avec soin les disques qui sont aussi proches que possible. Nous déconseillons mixage-et-mise en correspondance des disques du même type présentant des caractéristiques de performances ou endurance diminue brutalement différentes (sauf si un est le cache et l’autre capacité), car les espaces de stockage Direct répartit uniformément les e/s et n’a pas distinguer en fonction de modèle .

   > [!NOTE]
   > Il est OK et combinaisons les disques SAS et SATA similaires.

### Taille

Nous recommandons l’utilisation de disques de la même taille autant que possible. À l’aide de lecteurs de capacité de tailles différentes peut entraîner une capacité inutilisable et à l’aide de lecteurs de cache de tailles différentes pas peut améliorer les performances du cache. Consultez la section suivante pour plus d’informations.

   > [!WARNING]
   > Différentes tailles de lecteurs de capacité entre les serveurs peuvent entraîner de capacités inutilisées.

## Comprendre: déséquilibre de capacité

Espaces de stockage Direct est robuste déséquilibre de capacité sur les lecteurs et les serveurs. Même si le déséquilibre est grave, tous les éléments continueront à fonctionner. Toutefois, en fonction de plusieurs facteurs, la capacité n’est pas disponible dans tous les serveurs est peut-être pas utilisable.

Pour connaître la raison pour laquelle cela se produit, pensez à l’illustration simplifiée ci-dessous. Chaque zone colorée représente une copie des données en miroir. Par exemple, les zones marqué A A» et un compte personnel sont trois copies des mêmes données. Respecter la tolérance de panne de serveur, ces copies *doivent* être stockées sur différents serveurs.

### Capacités inutilisées

Lors de son dessin, Server 1 (10 To) et Server 2 (10 To) sont complet. Serveur 3 dispose de disques plus grands, donc sa capacité totale est plus grande (15 To). Toutefois, pour stocker les données de mise en miroir triple plus sur le serveur 3 nécessiterait copies sur Server 1 et 2 serveur trop, qui sont déjà complet. La capacité de 5 to restante sur Server 3 ne peuvent pas être utilisée; c’est *«inutilisés»* de capacité.

![Miroir triple, trois serveurs, inutilisés capacité](media/drive-symmetry-considerations/Size-Asymmetry-3N-Stranded.png)

### Positionnement optimal

À l’inverse, avec quatre serveurs de 10 To, 10 To, 10 To et 15 To de capacité et la résilience en miroir triple, il *est* possible de légitimement placez des copies d’une façon qui utilise toute la capacité disponible, comme dessinée. Chaque fois que cela est possible, l’espaces de stockage Direct d’allocation sera trouver et utiliser le placement optimal, en laissant aucune capacités inutilisées.

![Miroir triple, quatre serveurs, aucune capacité bloquée](media/drive-symmetry-considerations/Size-Asymmetry-4N-No-Stranded.png)

Le nombre de serveurs, la résilience, la gravité de le déséquilibre de capacité et d’autres facteurs affecte s’il y a des capacités inutilisées. **La règle générale plus prudente consiste à supposer que la capacité uniquement disponible dans tous les serveurs est garantie utilisable.**

## Comprendre: déséquilibre de cache

Espaces de stockage Direct est robuste déséquilibre de cache sur les lecteurs et les serveurs. Même si le déséquilibre est grave, tous les éléments continueront à fonctionner. En outre, les espaces de stockage Direct utilise toujours tous les cache disponible de manière optimale.

Toutefois, à l’aide de lecteurs de cache de tailles différentes ne peut pas améliorer les performances de cache uniformément ou de manière prévisible: seules les e/s pour [les mappages de lecteurs](understand-the-cache.md#server-side-architecture) avec des lecteurs de cache plus grandes peuvent voir une amélioration des performances. Espaces de stockage Direct répartit des e/s de manière égale dans les liaisons et n’a pas distinguer en fonction de coefficient de capacité de cache.

![Déséquilibre de cache](media/drive-symmetry-considerations/Cache-Asymmetry.png)

   > [!TIP]
   > Consultez le [fonctionnement du cache](understand-the-cache.md) en savoir plus sur les liaisons de cache.

## Exemples de configurations

Voici certaines configurations prises en charge et non pris en charge:

### ![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge: les modèles différents entre les serveurs

Les deux premiers serveurs utilisent NVMe modèle «X», mais le troisième serveur utilise un modèle de NVMe «Z», qui est très similaire.

| Serveur 1                    | Serveur 2                    | Serveur 3                    |
|-----------------------------|-----------------------------|-----------------------------|
| 2 x NVMe modèle X (cache)    | 2 x NVMe modèle X (cache)    | 2 x NVMe modèle Z (cache)    |
| 10 x SSD modèle Y (capacité) | 10 x SSD modèle Y (capacité) | 10 x SSD modèle Y (capacité) |

Cela est pris en charge.

### ![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge: les modèles différents au sein de serveur

Chaque serveur utilise certains différents modèles de disque dur «Y» et «Z», qui sont très similaires. Chaque serveur possède 10 HDD total.

| Serveur 1                   | Serveur 2                   | Serveur 3                   |
|----------------------------|----------------------------|----------------------------|
| 2 x SSD modèle X (cache)    | 2 x SSD modèle X (cache)    | 2 x SSD modèle X (cache)    |
| 7 x HDD modèle Y (capacité) | 5 x HDD modèle Y (capacité) | 1 x HDD modèle Y (capacité) |
| 3 x HDD modèle Z (capacité) | 5 x HDD modèle Z (capacité) | 9 x HDD modèle Z (capacité) |

Cela est pris en charge.

### ![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge: différentes tailles entre les serveurs

Les deux premiers serveurs utilisent HDD de 4 To, mais le troisième serveur utilise très similaire 6 To HDD.

| Serveur 1                | Serveur 2                | Serveur 3                |
|-------------------------|-------------------------|-------------------------|
| 2 x 800 Go NVMe (cache) | 2 x 800 Go NVMe (cache) | 2 x 800 Go NVMe (cache) |
| 4 x 4 To HDD (capacité) | 4 x 4 To HDD (capacité) | 4 x 6 To HDD (capacité) |

Cela est pris en charge, même si elle se traduit par des capacités inutilisées.

### ![pris en charge](media/drive-symmetry-considerations/supported.png) Pris en charge: les tailles différentes au sein de serveur

Chaque serveur utilise certains différents 1,2 To et très similaire 1,6 To SSD. Chaque serveur a 4 SSD total.

| Serveur 1                 | Serveur 2                 | Serveur 3                 |
|--------------------------|--------------------------|--------------------------|
| 3 x 1.2 To SSD (cache)   | 2 x 1.2 To SSD (cache)   | 4 x 1.2 To SSD (cache)   |
| 1 x 1,6 To SSD (cache)   | 2 x 1,6 To SSD (cache)   | -                        |
| 20 x 4 To HDD (capacité) | 20 x 4 To HDD (capacité) | 20 x 4 To HDD (capacité) |

Cela est pris en charge.

### ![absence de prise en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge: différents types de disques entre les serveurs

Le serveur 1 a NVMe, mais les autres ne.

| Serveur 1            | Serveur 2            | Serveur 3            |
|---------------------|---------------------|---------------------|
| 6 x NVMe (cache)    | -                   | -                   |
| -                   | 6 x SSD (cache)     | 6 x SSD (cache)     |
| 18 x HDD (capacité) | 18 x HDD (capacité) | 18 x HDD (capacité) |

Cela n’est pas pris en charge. Les types de disques doivent être identique dans tous les serveurs.

### ![absence de prise en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge: nombre de chaque type entre les serveurs différent

Serveur 3 dispose de plusieurs lecteurs que les autres.

| Serveur 1            | Serveur 2            | Serveur 3            |
|---------------------|---------------------|---------------------|
| 2 x NVMe (cache)    | 2 x NVMe (cache)    | 4 x NVMe (cache)    |
| 10 x HDD (capacité) | 10 x HDD (capacité) | 20 x HDD (capacité) |

Cela n’est pas pris en charge. Le nombre de disques de chaque type doit être identique dans tous les serveurs.

### ![absence de prise en charge](media/drive-symmetry-considerations/unsupported.png) Non pris en charge: seuls les lecteurs de disque dur

Tous les serveurs ont uniquement des disques HDD connectés.

|Serveur 1|Serveur 2|Serveur 3|
|-|-|-| 
|18 x HDD (capacité) |18 x HDD (capacité)|18 x HDD (capacité)|

Cela n’est pas pris en charge. Vous devez ajouter au moins deux lecteurs de cache (NvME ou SSD) associées à chacun des serveurs.

## Résumé

Pour résumer, tous les serveurs du cluster doivent avoir les mêmes types de disques et le même nombre de chaque type. Il est pris en charge pour les modèles de disques et combinaisons et tailles de disques en fonction des besoins, avec les considérations ci-dessus.

| Contrainte                               |               |
|------------------------------------------|---------------|
| Mêmes types de disques de chaque serveur     | **Requis**  |
| Même nombre de chaque type de chaque serveur | **Requis**  |
| Modèles de disques même dans tous les serveurs        | Nos recommandations   |
| Même des tailles de disques dans chaque serveur         | Nos recommandations   |

## Voir aussi

- [Configuration matérielle requise pour les espaces de stockage direct](storage-spaces-direct-hardware-requirements.md)
- [Présentation des espaces de stockage direct](storage-spaces-direct-overview.md)
