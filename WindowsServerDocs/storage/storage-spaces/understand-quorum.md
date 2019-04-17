---
title: Quorum de cluster et le pool de présentation
description: Présentation de Cluster et Pool de Quorum, avec des exemples spécifiques à emprunter les subtilités.
keywords: Espaces de stockage Direct, de Quorum, témoin, S2D, Cluster Quorum, Pool Quorum, Cluster, Pool
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024080"
---
# Quorum de cluster et le pool de présentation

>S’applique à: Windows Server 2019, Windows Server 2016

[Le Clustering de basculement Windows Server](../../failover-clustering/failover-clustering-overview.md) fournit une haute disponibilité pour les charges de travail. Ces ressources sont considérées comme hautement disponibles si les nœuds qui hébergent les ressources sont Toutefois, le cluster requiert généralement plus de la moitié des nœuds de s’exécuter, ce qui est connu comme possédant un *quorum*.

Quorum est conçu pour empêcher *split-brain* scénarios qui peuvent se produire lorsqu’il existe une partition dans le réseau et des sous-ensembles de nœuds ne peuvent pas communiquer entre eux. Cela peut entraîner à la fois des sous-ensembles de nœuds pour tenter de propriétaire de la charge de travail et d’écriture sur le même disque, ce qui peut entraîner des problèmes de nombreuses. Toutefois, cela permet d’éviter concept du Clustering de basculement de quorum qui force uniquement un de ces groupes de nœuds pour continuer à exécuter, afin qu’un seul de ces groupes restent en ligne.

Quorum détermine le nombre d’échecs que le cluster peut supporter tout en restant en ligne. Quorum est conçu pour gérer le scénario lorsqu’il existe un problème de communication entre des sous-ensembles de nœuds de cluster, afin que plusieurs serveurs n’essayez pas d’héberger un groupe de ressources simultanément et en écriture sur le même disque en même temps. Grâce à ce concept de quorum, le cluster force le service de cluster s’arrête à un des sous-ensembles de nœuds pour vous assurer qu’il n'existe qu’un seul véritable propriétaire d’un groupe de ressources particulier. Une fois que les nœuds qui ont été arrêtées peuvent là encore communiquer avec le groupe principal de nœuds, ils seront automatiquement rejoindre le cluster et démarrer leur service de cluster.

Dans Windows Server 2016, il existe deux composants du système qui ont leurs propres mécanismes de quorum:

- <strong>Quorum du cluster</strong>: cela fonctionne au niveau du cluster (autrement dit, vous pouvez perdre les nœuds et le cluster rester haut)
- <strong>Pool de Quorum</strong>: cela fonctionne au niveau du pool lorsque les espaces de stockage Direct est activé (autrement dit, vous pouvez perdre les nœuds et les lecteurs et le pool de rester haut). Les pools de stockage ont été conçus pour être utilisé dans les scénarios à la fois en cluster et non cluster, c’est pourquoi ils disposent d’un mécanisme de quorum différent.

## Vue d’ensemble de quorum de cluster

Le tableau ci-dessous donne une vue d’ensemble des résultats Quorum du Cluster par scénario:

| Nœuds de serveur | Peut après une panne de nœud un serveur | Peut résister défaillance de nœud d’un serveur, puis un autre | Peut résister à deux défaillances de nœud serveur simultanées |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | Non                                                | Non                                                 |
| 2 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 3            | Oui                                 | 50/50                                             | Non                                                 |
| 3 + témoin  | Oui                                 | Oui                                               | Non                                                 |
| 4            | Oui                                 | Oui                                               | 50/50                                              |
| 4 + témoin  | Oui                                 | Oui                                               | Oui                                                |
| 5 et versions ultérieures  | Oui                                 | Oui                                               | Oui                                                |

### Recommandations en matière de quorum de cluster

- Si vous avez deux nœuds, un témoin est <strong>requis</strong>.
- Si vous disposez de trois ou quatre nœuds, témoin est <strong>fortement recommandé</strong>.
- Si vous avez accès à Internet, utilisez un <strong> [témoin cloud](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Si vous êtes dans un environnement informatique avec d’autres ordinateurs et les partages de fichiers, utilisez un témoin de partage de fichiers

## Fonctionne de quorum du cluster

Lors de l’échouent des nœuds, ou quand un sous-ensemble des nœuds perd le contact avec un autre sous-ensemble, les nœuds restants ont besoin vérifier qu’elle constitue la *majorité* du cluster restent en ligne. Si elles ne peut pas vérifier qui, elles vous passez en mode hors connexion.

Mais le concept de *majorité* ne fonctionne correctement lorsque le nombre total de nœuds du cluster est dimension non standard (par exemple, trois nœuds dans un cluster à cinq nœuds). Par conséquent, qu’en est-il des clusters avec un nombre pair de nœuds (par exemple, un cluster de quatre nœuds)?

Il existe deux façons que le cluster peut rendre le *nombre total de voix* dimension non standard:

1. Tout d’abord, il peut passer ** d’une en ajoutant un *témoin* avec un vote supplémentaire. Cette opération nécessite une configuration utilisateur.
2.  Sinon, il peut aller *vers le bas* en mettant à zéro voix d’un nœud dans d’autres (se produit automatiquement en fonction des besoins).

Chaque fois que les nœuds restants vérifier qu’ils représentent la *majorité*, la définition de la *majorité* est mis à jour pour être entre simplement les survivants. Cela permet au cluster de perdre un nœud, puis un autre, puis un autre et ainsi de suite. Ce concept le *nombre total de voix* adapter une fois que les échecs successifs est appelé <strong> *quorum dynamique*</strong>.  

### Témoin dynamique

Témoin dynamique active ou désactive la voix de témoin pour vous assurer que le *nombre total de voix* est irrégulière. S’il existe un nombre de voix dimension non standard, le rappel ne dispose d’une voix. S’il existe un nombre pair de voix, le rappel a un vote. Témoin dynamique réduit considérablement le risque que le cluster est soumis vers le bas en raison de l’échec de témoin. Le cluster détermine s’il convient d’utiliser le vote témoin en fonction du nombre de nœuds votants qui sont disponibles dans le cluster.

Quorum dynamique fonctionne avec témoin dynamique de la manière décrite ci-dessous.

### Comportement de quorum dynamique

- Si vous disposez d’une <strong>même</strong> nombre de nœuds et aucune témoin, *un nœud obtient son vote à zéro*. Par exemple, seuls trois des quatre nœuds obtiennent voix, afin que le *nombre total de voix* est trois, et deux survivants avec voix sont pris en compte la majorité.
- Si vous disposez d’une <strong>étrange</strong> nombre de nœuds et aucun témoin, *ils toutes les voix*.
- Si vous disposez d’une <strong>même</strong> nombre de nœuds, ainsi que témoin, *le rappel votes*, afin que le total est irrégulière.
- Si vous disposez d’une <strong>étrange</strong> nombre de nœuds, ainsi que témoin, *le rappel n’a pas voter*.

Quorum dynamique permet la possibilité d’attribuer un vote à un nœud dynamiquement pour éviter de perdre la majorité des voix et pour permettre le cluster pour s’exécuter avec un nœud (on parle dernier-man permanent). Nous allons prendre un cluster à quatre nœuds comme exemple. Partons du principe que quorum nécessite 3 votes. 

Dans ce cas, le cluster serait ait été arrêté si vous avez perdu deux nœuds.

![Diagramme montrant quatre nœuds de cluster, chacun d'entre eux Obtient un vote](media/understand-quorum/dynamic-quorum-base.png)

Toutefois, quorum dynamique empêche cela se produise. Le *nombre total de voix* requis pour le quorum est maintenant déterminé en fonction du nombre de nœuds disponibles. Par conséquent, avec quorum dynamique, le cluster restent au même si vous perdez trois nœuds.

![Diagramme montrant les quatre nœuds de cluster, avec des nœuds un à la fois et le nombre de voix requis postérieurs à chaque échec échouent.](media/understand-quorum/dynamic-quorum-step-through.png)

Le scénario ci-dessus s’applique à un cluster général qui n’a pas espaces de stockage Direct est activé. Toutefois, les espaces de stockage Direct est activé, le cluster peut uniquement prendre en charge deux défaillances de nœud. Cela est expliqué plus dans la [section de quorum pool](#poolQuorum).

### Exemples

#### Deux nœuds sans un témoin. 
Voix d’un nœud est mis à zéro, afin que la *majorité* des voix sont déterminé en dehors d’un total de <strong>1 vote</strong>. Si le nœud non vote inattendue tombe en panne, la survie a 1/1 et survit au cluster. Si le nœud vote inattendue tombe en panne, la survie a 0/1 et le cluster tombe en panne. Si le nœud vote est hors tension normalement, le vote est transféré à l’autre nœud et survit au cluster. *<strong>C’est pourquoi il est essentiel de configurer un témoin.</strong>*

![Expliqué dans le cas avec deux nœuds sans un témoin de quorum](media/understand-quorum/2-node-no-witness.png)

- Peut résister à une défaillance du serveur: <strong>chance de 50 %</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>non</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>non</strong>. 

#### Deux nœuds d’un témoin. 
Les deux nœuds votent, ainsi que la voix de témoin, afin que la *majorité* est déterminé en dehors d’un total de <strong>3 votes</strong>. Si des nœuds tombe en panne, la survie a 2/3 et survit au cluster.

![Expliqué dans le cas avec deux nœuds d’un témoin de quorum](media/understand-quorum/2-node-witness.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>non</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>non</strong>. 

#### Trois nœuds sans un témoin.
Tous les nœuds votent, afin que la *majorité* est déterminé en dehors d’un total de <strong>3 votes</strong>. Si n’importe quel nœud tombe en panne, les survivants sont 2/3 et survit au cluster. Le cluster devient deux nœuds sans un témoin: à ce stade, vous êtes dans le scénario 1.

![Expliqué dans le cas avec trois nœuds sans un témoin de quorum](media/understand-quorum/3-node-no-witness.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>chance de 50 %</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>non</strong>. 

#### Trois nœuds d’un témoin.
Tous les nœuds votent, afin que le rappel n’a pas voter initialement. La *majorité* est déterminé en dehors d’un total de <strong>3 votes</strong>. Après un échec, le cluster possède deux nœuds d’un témoin – qui correspond au scénario 2. Par conséquent, désormais les deux nœuds et le témoin votent.

![Expliqué dans le cas avec trois nœuds d’un témoin de quorum](media/understand-quorum/3-node-witness.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>Oui</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>non</strong>. 

#### Quatre nœuds sans un témoin
Voix d’un nœud est mis à zéro, afin que la *majorité* est déterminé en dehors d’un total de <strong>3 votes</strong>. Après un échec, le cluster devient trois nœuds, et que vous êtes dans le scénario 3.

![Expliqué dans le cas avec les quatre nœuds sans un témoin de quorum](media/understand-quorum/4-node-no-witness.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>Oui</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>chance de 50 %</strong>. 

#### Quatre nœuds d’un témoin.
Toutes les voix de nœuds et les votes témoin, afin que la *majorité* est déterminé en dehors d’un total de <strong>5 votes</strong>. Après un échec, vous êtes dans le scénario 4. Après avoir deux défaillances simultanées, vous ignorez jusqu'à 2 de scénario.

![Expliqué dans le cas avec les quatre nœuds d’un témoin de quorum](media/understand-quorum/4-node-witness.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>Oui</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>Oui</strong>. 

#### Cinq nœuds et au-delà.
Tous les nœuds votent, ou toutes les mais une seule voix, quelle que soit rend le total dimension non standard. Espaces de stockage Direct ne peut pas gérer plus de deux nœuds vers le bas malgré tout, à ce stade, aucun témoin n’est nécessaire ou utile.

![Quorum expliqué dans le cas avec cinq nœuds et au-delà](media/understand-quorum/5-nodes.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>Oui</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>Oui</strong>. 

Maintenant que nous avons comprendre le fonctionne de quorum, examinons les types de témoins de quorum.

### Types de témoin de quorum

Le Clustering de basculement prend en charge trois types de témoins de Quorum:

- <strong>[Témoin de cloud](../../failover-clustering\deploy-cloud-witness.md) </strong> -Blob de stockage dans Azure accessible à tous les nœuds du cluster. Il conserve les informations de cluster dans un fichier witness.log, mais n’a pas de stocker une copie de la base de données du cluster.
- <strong>Témoin de partage de fichiers</strong> – partage de fichiers SMB A configuré sur un serveur de fichiers exécutant Windows Server. Il conserve les informations de cluster dans un fichier witness.log, mais n’a pas de stocker une copie de la base de données du cluster.
- <strong>Disque témoin</strong> -un petit disque en cluster qui se trouve dans le groupe de Cluster de stockage disponible. Ce disque est hautement disponible et peut basculer entre les nœuds. Elle contient une copie de la base de données du cluster.  <strong>*Un témoin de disque n’est pas pris en charge avec les espaces de stockage Direct*</strong>.

## <a id="poolQuorum"></a>Vue d’ensemble de quorum pool

Nous venons de parler de Quorum du Cluster, qui fonctionne au niveau du cluster. Maintenant, nous allons pencher sur le Pool de Quorum, qui opère sur le niveau de pool (autrement dit, vous pouvez perdre des nœuds et des lecteurs et laisser le pool de haut). Les pools de stockage ont été conçus pour être utilisé dans les scénarios à la fois en cluster et non cluster, c’est pourquoi ils disposent d’un mécanisme de quorum différent.

Le tableau ci-dessous donne une vue d’ensemble des résultats Quorum Pool par scénario:

| Nœuds de serveur | Peut après une panne de nœud un serveur | Peut résister défaillance de nœud d’un serveur, puis un autre | Peut résister à deux défaillances de nœud serveur simultanées |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | Non                                  | Non                                                | Non                                                 |
| 2 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 3            | Oui                                 | Non                                                | Non                                                 |
| 3 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 4            | Oui                                 | Non                                                | Non                                                 |
| 4 + témoin  | Oui                                 | Oui                                               | Oui                                                |
| 5 et versions ultérieures  | Oui                                 | Oui                                               | Oui                                                |

## Fonctionne de quorum pool

Lors de la panne de disques, ou quand un sous-ensemble des lecteurs perd le contact avec un autre sous-ensemble, les disques restants ont besoin vérifier qu’elle constitue la *majorité* du pool de rester en ligne. Si elles ne peut pas vérifier qui, elles vous passez en mode hors connexion. Le pool est l’entité qui est hors connexion ou reste en ligne selon qu’il dispose de suffisamment de disques pour le quorum (50 % + 1). Le propriétaire de la ressource pool (nœud du cluster actif) peut être + 1.

Mais quorum pool fonctionne différemment de quorum du cluster sur les points suivants:

- le pool utilise un nœud du cluster en tant que témoin comme un séparateur de lier pour résister la moitié des lecteurs disparus (ce nœud qui est le propriétaire de la ressource pool)
- le pool n’a pas de quorum dynamique
- le pool n’implémente pas sa propre version de la suppression d’un vote

### Exemples

#### Quatre nœuds avec une disposition symétrique. 
Chacun des 16 disques dispose d’une voix et nœud deux possède également une voix (dans la mesure où il est propriétaire de la ressource pool). La *majorité* est déterminé en dehors d’un total de <strong>16 voix</strong>. Si les nœuds 3 et 4 go vers le bas, le sous-ensemble survivant a 8 disques et le propriétaire de la ressource pool, c'est-à-dire la voix de 9/16. Par conséquent, le pool survit à.

![Pool Quorum 1](media/understand-quorum/pool-1.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>Oui</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>Oui</strong>. 

#### Quatre nœuds avec un échec de mise en page et lecteur symétrique. 
Chacun des 16 disques dispose d’une voix et le nœud 2 a également une voix (dans la mesure où il est propriétaire de la ressource pool). La *majorité* est déterminé en dehors d’un total de <strong>16 voix</strong>. Tout d’abord, lecteur 7 tombe en panne. Si les nœuds 3 et 4 go vers le bas, le sous-ensemble survivant a 7 disques et le propriétaire de la ressource pool, c'est-à-dire la voix de 8/16. Par conséquent, le pool n’a pas majorité et tombe en panne.

![Pool Quorum 2](media/understand-quorum/pool-2.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>non</strong>.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>non</strong>. 

#### Quatre nœuds avec une disposition non symétrique. 
Chacun des 24 disques dispose d’une voix et nœud deux possède également une voix (dans la mesure où il est propriétaire de la ressource pool). La *majorité* est déterminé en dehors d’un total de <strong>24 voix</strong>. Si les nœuds 3 et 4 go vers le bas, le sous-ensemble survivant a 8 disques et le propriétaire de la ressource pool, c'est-à-dire la voix de 9/24. Par conséquent, le pool n’a pas majorité et tombe en panne.

![Pool Quorum 3](media/understand-quorum/pool-3.png)

- Peut résister à une défaillance du serveur: <strong>Oui</strong>.
- Peut résister Échec d’un serveur, puis un autre: <strong>finalité </strong>(ne peut pas résister si les deux nœuds 3 et 4 vers le bas, mais nous allons peuvent résister tous les autres scénarios.
- Peut faire face aux deux défaillances de serveur à la fois: <strong>finalité </strong>(ne peut pas résister si les deux nœuds 3 et 4 vers le bas, mais nous allons peuvent résister tous les autres scénarios.

### Recommandations en matière de quorum pool

- Assurez-vous que chaque nœud dans votre cluster est symétrique (chaque nœud a le même nombre de disques)
- Activer la mise en miroir triple ou la parité double afin que vous pouvez tolérer un défaillances de nœud et conserver les disques virtuels en ligne. Consultez notre [page de volume Guide](plan-volumes.md) pour plus d’informations.
- Si un disque sur un autre nœud et plus de deux nœuds sont vers le bas, ou deux nœuds sont arrêtés, volumes n’aient pas accès à toutes les trois copies de leurs données et donc disponible hors connexion et ne pas être disponibles. Il est recommandé de ramener les serveurs ou remplacer les disques rapidement pour garantir la résilience de la plupart pour toutes les données du volume.

## Informations supplémentaires

- [Configurer et gérer le quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Déployer un témoin de cloud](../../failover-clustering/deploy-cloud-witness.md)
