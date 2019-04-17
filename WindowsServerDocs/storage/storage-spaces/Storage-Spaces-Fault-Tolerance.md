---
title: Tolérance de panne et efficacité du stockage dans les espaces de stockage direct
ms.prod: windows-server-threshold
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 10/11/2017
ms.assetid: 5e1d7ecc-e22e-467f-8142-bad6d82fc5d0
description: Description des options de résilience dans les espaces de stockage direct, y compris la mise en miroir et la parité.
ms.localizationpriority: medium
ms.openlocfilehash: 4e6a29e82a85ec9570cda827060dfe1cdf192c53
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1284976"
---
# <a name="fault-tolerance-and-storage-efficiency-in-storage-spaces-direct"></a>Tolérance de panne et efficacité du stockage dans les espaces de stockage direct

>S’applique à Windows Server2016

Cette rubrique présente les options de résilience disponibles dans les [espaces de stockage direct](storage-spaces-direct-overview.md) et décrit les exigences de mise à l’échelle, l’efficacité du stockage, ainsi que les avantages et inconvénients de chacun de ces aspects. Elle fournit également des instructions de prise en main, ainsi que des références (articles, blogs et autres contenus) que vous pouvez consulter pour en savoir plus.

Si vous connaissez les espaces de stockage, vous pouvez passer la section [Résumé](#summary).

## <a name="overview"></a>Vue d’ensemble

Le rôle des espaces de stockage est d’assurer à vos données une tolérance de pannes, souvent appelée «résilience». Leur implémentation est similaire à RAID, à ceci près qu’ils sont distribués sur plusieurs serveurs et mis en œuvre par voie logicielle.

Comme c’est le cas avec RAID, les espaces de stockage peuvent assurer cette fonction de différentes manières. Il faut donc effectuer des compromis entre tolérance de pannes, efficacité du stockage et complexité de calcul. Les possibilités sont au nombre de deux: la «mise en miroir» et la «parité» (parfois appelée «codage d’effacement»).

## <a name="mirroring"></a>Mise en miroir

La mise en miroir assure une tolérance de pannes en conservant plusieurs copies de toutes les données. Cela ressemble de près à RAID-1. La répartition et le positionnement des données ne sont pas secondaires (voir [ce blog](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/) pour en savoir plus), mais il faut reconnaître que les données stockées avec la mise en miroir sont écrites plusieurs fois en intégralité. Chaque copie est écrite sur différents matériels physiques (différents disques sur différents serveurs) qui sont censés ne pas tomber en panne simultanément.

Dans WindowsServer2016, les espaces de stockage proposent deuxtypes de mise en miroir: «double» et «triple».

### <a name="two-way-mirror"></a>Miroir double

La mise en miroir double écrit deuxcopies de toutes les données. Son efficacité de stockage est de 50%. Pour écrire 1To de données, vous devez donc disposer d’au moins 2To de capacité de stockage physique. De même, vous devez avoir au moins deux [domaines d’erreur matériels](../../failover-clustering/fault-domains.md). Avec des espaces de stockage direct, cela signifie deuxserveurs.

![Miroir double](media/Storage-Spaces-Fault-Tolerance/two-way-mirror-180px.png)

   >[!WARNING]
   > Si vous possédez plus de deuxserveurs, nous vous recommandons plutôt la mise en miroir 3voies.

### <a name="three-way-mirror"></a>Miroir triple

La mise en miroir triple écrit troiscopies de toutes les données. Son efficacité de stockage est de 33,3%. Pour écrire 1To de données, vous devez donc disposer d’au moins 3To de capacité de stockage physique. De même, il vous faut au moins troisdomaines d’erreur matériels. Avec des espaces de stockage direct, cela signifie troisserveurs.

La mise en miroir triple tolère [au moins deuxproblèmes matériels (disque ou serveur) à la fois](#examples). Par exemple, si vous redémarrez un serveur au moment où un autre disque ou serveur tombe soudainement en panne, toutes les données restent en sécurité et demeurent accessibles en continu.

![Miroir triple](media/Storage-Spaces-Fault-Tolerance/three-way-mirror-180px.png)

## <a name="parity"></a>Parité

Le codage de parité (souvent appelé «codage d’effacement») assure la tolérance de pannes à l’aide d’une opération arithmétique au niveau des bits, ce qui peut devenir [remarquablement compliqué](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/LRC12-cheng20webpage.pdf). Cette solution est plus complexe que la mise en miroir, et de nombreuses ressources en ligne (comme le document [Dummies Guide to Erasure Coding](http://smahesh.com/blog/2012/07/01/dummies-guide-to-erasure-coding/)) peuvent vous aider à vous en faire une idée plus précise. Il suffit de dire qu’elle offre un stockage plus efficace sans compromettre la tolérance de pannes.

Dans WindowsServer2016, les espaces de stockage proposent deuxtypes de parité: «simple» et «double». La parité double emploie une technique de pointe, baptisée «codes de reconstruction locale» ou LRC (pour Local Reconstruction Codes), à plus grande échelle.

> [!IMPORTANT]
> Nous vous recommandons d’utiliser la mise en miroir pour la plupart des charges de travail dépendantes des performances. Pour plus d’informations sur la façon de trouver le juste équilibre entre performances et capacité en fonction de votre charge de travail, voir [Planifier des volumes](plan-volumes.md#choosing-the-resiliency-type).

### <a name="single-parity"></a>Parité simple
La parité simple, qui ne conserve qu’un symbole de parité de bit, assure une tolérance d’une seule panne à la fois. Cela ressemble de près à RAID-5. Pour utiliser la parité simple, il vous faut au moins troisdomaines d’erreur matériels. Avec des espaces de stockage direct, cela signifie troisserveurs. Comme la mise en miroir triple fournit une plus grande tolérance de pannes à la même échelle, nous déconseillons l’utilisation de la parité simple. Toutefois, cette solution a le mérite d’exister et d’être totalement prise en charge.

   >[!WARNING]
   > Nous déconseillons l’utilisation de la parité simple, car elle ne tolère qu’une défaillance matérielle à la fois: si vous redémarrez un serveur au moment où un autre disque ou serveur tombe soudainement en panne, vous risquez de rencontrer des temps d’arrêt. Si vous n’avez que troisserveurs, nous vous recommandons d’utiliser la mise en miroir triple. Si vous en avez quatre ou plus, consultez la section suivante.

### <a name="dual-parity"></a>Parité double

La parité double met en œuvre les codes de correction d’erreur Reed-Solomon pour conserver deux symboles de parité de bit, offrant ainsi la même tolérance de panne que la mise en miroir triple (c’est-à-dire jusqu’à deux pannes en même temps), mais avec un stockage plus efficace. Cela ressemble de près à RAID-6. Pour utiliser la parité double, il vous faut au moins quatredomaines d’erreur matériels. Avec des espaces de stockage direct, cela signifie quatreserveurs. À cette échelle, son efficacité de stockage est de 50%. Pour écrire 2To de données, vous devez donc disposer d’au moins 4To de capacité de stockage physique.

![dual-parity](media/Storage-Spaces-Fault-Tolerance/dual-parity-180px.png)

L’efficacité de stockage de la parité double passe de 50 à 80% lorsque vous augmentez votre nombre de domaines d’erreur matériels. Par exemple, à sept (avec les espaces de stockage direct, cela signifie septserveurs) l’efficacité atteint 66,7%. Pour stocker 4To de données, vous devez donc disposer de 6To de capacité de stockage physique.

![dual-parity-wide](media/Storage-Spaces-Fault-Tolerance/dual-parity-wide-180px.png)

Consultez la section [Résumé](#summary) pour connaître l’efficacité de la parité double et des codes de reconstruction locale à chaque échelle.

### <a name="local-reconstruction-codes"></a>Codes de reconstruction locale (LRC)

Les espaces de stockage de WindowsServer2016 mettent en œuvre une technique de pointe développée par Microsoft Research, baptisée «codes de reconstruction locale» (ou LRC). À grande échelle, la parité double utilise LRC pour fractionner son codage/décodage en groupes plus petits afin d’accélérer les écritures ou la reprise après incident.

Avec des disques durs classiques, la taille des groupes est de quatresymboles; avec des disquesSSD, elle est de sixsymboles. Pour illustrer ce propos, voici un exemple avec des disques durs et 12domaines d’erreur matériels (donc 12serveurs). Il y a deuxgroupes de quatresymboles de données. Son stockage atteint 72,7% d’efficacité.

![local-reconstruction-codes](media/Storage-Spaces-Fault-Tolerance/local-reconstruction-codes-180px.png)

Nous recommandons cette procédure de [Claus Joergensen](https://twitter.com/clausjor), qui explique en détail mais de manière très compréhensible [comment les codes LRC gèrent différents scénarios de panne et en quoi ils sont intéressants](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/).

## <a name="mirror-accelerated-parity"></a>Parité accélérée grâce à la mise en miroir

Depuis Windows Server2016, un volume des espaces de stockage direct peut utiliser à la fois la mise en miroir et la parité. Les écritures sont dans un premier temps hébergées sur la partie en miroir et sont progressivement déplacées, plus tard, dans la partie dédiée à la parité. En effet, il s’agit [d'utiliser la mise en miroir pour accélérer le codage](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/).

Pour combiner la mise en miroir triple et la parité double, il faut au moins quatredomaines d’erreur, soit quatreserveurs.

L’efficacité de stockage de la parité accélérée grâce à la mise en miroir varie entre le maximum d’une utilisation 100% miroir et d’une utilisation 100% parité double, et dépend des proportions que vous choisissez. Par exemple, la démonstration débutant à la 37eminute de cette présentation montre [plusieurs combinaisons atteignant une efficacité de 46, 54 et 65%](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) avec 12serveurs.

> [!IMPORTANT]
> Nous vous recommandons d’utiliser la mise en miroir pour la plupart des charges de travail dépendantes des performances. Pour plus d’informations sur la façon de trouver le juste équilibre entre performances et capacité en fonction de votre charge de travail, voir [Planifier des volumes](plan-volumes.md#choosing-the-resiliency-type).

## <a name="summary"></a>Résumé

Cette section résume les types de résilience disponibles dans les espaces de stockage direct, les échelles minimum pour utiliser chaque type, le nombre de pannes tolérées par chaque type et l’efficacité de stockage correspondante.

### <a name="resiliency-types"></a>Types de résilience

|    Résilience          |    Tolérance de pannes       |    Efficacité du stockage      |
|------------------------|----------------------------|----------------------------|
|    Miroir double      |    1                       |    50%                   |
|    Miroir triple    |    2                       |    33,3%                   |
|    Parité double         |    2                       |    50 à 80%           |
|    Mixte               |    2                       |    33,3 à 80%           |

### <a name="minimum-scale-requirements"></a>Échelle minimale requise

|    Résilience          |    Nombre de domaines minimum requis par défaut   |
|------------------------|-------------------------------------|
|    Miroir double      |    2                                |
|    Miroir triple    |    3                                |
|    Parité double         |    4                                |
|    Mixte               |    4                                |

   >[!TIP]
   > Sauf si vous utilisez la [tolérance de pannes des châssis ou des racks](../../failover-clustering/fault-domains.md), le nombre de domaines d’erreur correspond au nombre de serveurs. Le nombre de disques dans chaque serveur est sans effet sur les types de résilience utilisables, tant que vous respectez les exigences minimales applicables aux espaces de stockage direct. 

### <a name="dual-parity-efficiency-for-hybrid-deployments"></a>Efficacité de la parité double pour les déploiements hybrides

Ce tableau montre l’efficacité de stockage de la parité double et des codesLRC à chaque échelle pour des déploiements hybrides combinant des disques durs classiques et des disquesSSD.

|    Domaines d’erreur      |    Disposition           |    Efficacité   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50%        |
|    5                  |    RS 2+2           |    50%        |
|    6                  |    RS 2+2           |    50%        |
|    7                  |    RS 4+2           |    66,7%        |
|    8                  |    RS 4+2           |    66,7%        |
|    9                  |    RS 4+2           |    66,7%        |
|    10                 |    RS 4+2           |    66,7%        |
|    11                 |    RS 4+2           |    66,7%        |
|    12                 |    LRC (8, 2, 1)    |    72.7%        |
|    13                 |    LRC (8, 2, 1)    |    72.7%        |
|    14                 |    LRC (8, 2, 1)    |    72.7%        |
|    15                 |    LRC (8, 2, 1)    |    72.7%        |
|    16                 |    LRC (8, 2, 1)    |    72.7%        |

### <a name="dual-parity-efficiency-for-all-flash-deployments"></a>Efficacité de la parité double pour les déploiements 100% flash

Ce tableau montre l’efficacité de stockage de la parité double et des codesLRC à chaque échelle pour des déploiements 100% flash ne contenant que des disquesSSD. La parité peut utiliser des groupes plus volumineux et offrir un stockage plus efficace dans une configuration 100% flash.

|    Domaines d’erreur      |    Disposition           |    Efficacité   |
|-----------------------|---------------------|-----------------|
|    2                  |    –                |    –            |
|    3                  |    –                |    –            |
|    4                  |    RS 2+2           |    50%        |
|    5                  |    RS 2+2           |    50%        |
|    6                  |    RS 2+2           |    50%        |
|    7                  |    RS 4+2           |    66,7%        |
|    8                  |    RS 4+2           |    66,7%        |
|    9                  |    RS 6+2           |    75%        |
|    10                 |    RS 6+2           |    75%        |
|    11                 |    RS 6+2           |    75%        |
|    12                 |    RS 6+2           |    75%        |
|    13                 |    RS 6+2           |    75%        |
|    14                 |    RS 6+2           |    75%        |
|    15                 |    RS 6+2           |    75%        |
|    16                 |    LRC (12, 2, 1)   |    80%        |

## <a name="examples"></a>Exemples

Sauf si vous n’avez que deuxserveurs, nous recommandons d’utiliser la mise en miroir triple et/ou la parité double, car ce modèle offre une meilleure tolérance de pannes. Plus précisément, il garantit la sécurité et l’accessibilité de toutes les données en permanence, même en cas de défaillance simultanée des deuxdomaines d’erreur (avec les espaces de stockage direct, cela signifie deuxserveurs).

### <a name="examples-where-everything-stays-online"></a>Exemples où tout reste en ligne

Ces sixexemples montrent ce que la mise en miroir triple et/ou la double parité **peuvent** tolérer.

- **1.**    Perte d’un disque (disques cache compris)
- **2.**    Perte d’un serveur

![fault-tolerance-examples-1-and-2](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-12.png)

- **3.**    Perte d’un serveur et d’un disque
- **4.**    Perte de deuxdisques de différents serveurs

![fault-tolerance-examples-3-and-4](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-34.png)

- **5.**    Perte de plus de deuxdisques, tant que deuxserveurs au maximum sont affectés
- **6.**    Perte de deuxserveurs

![fault-tolerance-examples-5-and-6](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-56.png)

...Dans tous les cas, tous les volumes restent en ligne. (Vérifiez que votre cluster conserve le quorum).

### <a name="examples-where-everything-goes-offline"></a>Exemples où tout passe hors connexion

Pendant leur durée de vie, les espaces de stockage tolèrent un nombre illimité de pannes, car ils restaurent une résilience totale après chaque incident, pourvu qu’ils en aient le temps. Toutefois, le nombre maximum de domaines qui peuvent être affectés par des pannes à un moment donné est dedeux. Les exemples suivants montrent ce que la mise en miroir triple et/ou la parité double **ne peuvent pas** tolérer.

- **7.**    Perte de disques dans troisserveurs au moins à la fois
- **8.** Perte d’au moins troisserveurs à la fois

![fault-tolerance-examples-7-and-8](media/Storage-Spaces-Fault-Tolerance/Fault-Tolerance-Example-78.png)

## <a name="usage"></a>Utilisation

Consultez [Création de volumes dans les espaces de stockage direct](create-volumes.md).

## <a name="see-also"></a>Voir également

Chaque lien ci-dessous figure déjà dans le corps de cette rubrique.

- [Espaces de stockage direct dans WindowsServer2016](storage-spaces-direct-overview.md)
- [Reconnaissance des domaines d’erreur dans Windows Server2016](../../failover-clustering/fault-domains.md)
- [Codage d’effacement dans Azure par Microsoft Research](https://www.microsoft.com/en-us/research/publication/erasure-coding-in-windows-azure-storage/)
- [Codes de reconstruction locale et volumes à parité plus rapide](https://blogs.technet.microsoft.com/filecab/2016/09/06/volume-resiliency-and-efficiency-in-storage-spaces-direct/)
- [Volumes dans l’API de gestion de stockage](https://blogs.technet.microsoft.com/filecab/2016/08/29/deep-dive-volumes-in-spaces-direct/)
- [Démonstration de l’efficacité de stockage à Microsoft Ignite2016](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s)
- [Version préliminaire de la calculatrice de capacité dédiée aux espaces de stockage direct](http://aka.ms/s2dcalc)
