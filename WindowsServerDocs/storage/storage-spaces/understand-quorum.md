---
title: Fonctionnement du quorum de cluster et de pool
description: Comprendre le quorum de cluster et de pool, avec des exemples spécifiques pour passer en revue les subtilités.
keywords: Espaces de stockage direct, quorum, témoin, S2D, quorum de cluster, quorum de pool, cluster, pool
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 962a4edc1a171167a6af336d4fb32188a526f455
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872133"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Fonctionnement du quorum de cluster et de pool

>S’applique à : Windows Server 2019, Windows Server 2016

Le [clustering de basculement Windows Server](../../failover-clustering/failover-clustering-overview.md) offre une haute disponibilité pour les charges de travail. Ces ressources sont considérées comme hautement disponibles si les nœuds hébergeant les ressources sont disponibles ; Toutefois, le cluster nécessite généralement plus de la moitié des nœuds qui sont en cours d’exécution, ce qui est connu sous le nom de *quorum*.

Le quorum est conçu pour empêcher les scénarios de *fractionnement,* qui peuvent se produire lorsqu’il existe une partition dans le réseau et que les sous-ensembles de nœuds ne peuvent pas communiquer entre eux. Cela peut amener les deux sous-ensembles de nœuds à essayer de posséder la charge de travail et à écrire sur le même disque, ce qui peut entraîner de nombreux problèmes. Toutefois, cela n’est pas possible avec le concept de quorum du clustering de basculement qui force l’exécution d’un seul de ces groupes de nœuds pour continuer à s’exécuter. par conséquent, un seul de ces groupes restera en ligne.

Le quorum détermine le nombre de défaillances que le cluster peut supporter tout en restant en ligne. Le quorum est conçu pour gérer le scénario lorsqu’il y a un problème de communication entre les sous-ensembles de nœuds de cluster, de sorte que plusieurs serveurs ne tentent pas d’héberger simultanément un groupe de ressources et d’écrire sur le même disque en même temps. Grâce à ce concept de quorum, le cluster force le service de cluster à s’arrêter dans l’un des sous-ensembles de nœuds pour s’assurer qu’il n’existe qu’un seul propriétaire d’un groupe de ressources particulier. Une fois que les nœuds qui ont été arrêtés peuvent une fois de plus communiquer avec le groupe de nœuds principal, ils sont automatiquement rejoints au cluster et démarrent leur service de cluster.

Dans Windows Server 2019 et Windows Server 2016, deux composants du système disposent de leurs propres mécanismes de quorum :

- **Quorum du cluster**: Cela fonctionne au niveau du cluster (par exemple, vous pouvez perdre des nœuds et faire en sorte que le cluster reste opérationnel).
- **Quorum du pool**: Cela fonctionne au niveau du pool lorsque espaces de stockage direct est activé (par exemple, vous pouvez perdre des nœuds et des lecteurs et faire en sorte que le pool reste actif). Les pools de stockage ont été conçus pour être utilisés dans des scénarios en cluster et non cluster, ce qui explique pourquoi ils disposent d’un mécanisme de quorum différent.

## <a name="cluster-quorum-overview"></a>Vue d’ensemble du quorum de cluster

Le tableau ci-dessous donne une vue d’ensemble des résultats du quorum de cluster par scénario :

| Nœuds de serveur | Peut survivre à une défaillance de nœud de serveur | Peut survivre à un échec de nœud de serveur, puis à un autre | Peut survivre à deux échecs de nœuds de serveur simultanés |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | Non                                                | Non                                                 |
| 2 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 3            | Oui                                 | 50/50                                             | Non                                                 |
| témoin 3 +  | Oui                                 | Oui                                               | Non                                                 |
| 4            | Oui                                 | Oui                                               | 50/50                                              |
| 4 + témoin  | Oui                                 | Oui                                               | Oui                                                |
| 5 et versions ultérieures  | Oui                                 | Oui                                               | Oui                                                |

### <a name="cluster-quorum-recommendations"></a>Recommandations en matière de quorum de cluster

- Si vous avez deux nœuds, un témoin est **nécessaire**.
- Si vous disposez de trois ou quatre nœuds, le témoin est **fortement recommandé**.
- Si vous avez accès à Internet, utilisez un  **[témoin Cloud](../../failover-clustering/deploy-cloud-witness.md)**
- Si vous êtes dans un environnement informatique avec d’autres machines et partages de fichiers, utilisez un témoin de partage de fichiers

## <a name="how-cluster-quorum-works"></a>Fonctionnement du quorum de cluster

Lorsque les nœuds échouent, ou lorsque certains sous-ensembles de nœuds perdent leur contact avec un autre sous-ensemble, les nœuds survivants doivent vérifier qu’ils constituent la *majorité* du cluster pour rester en ligne. S’ils ne peuvent pas le vérifier, ils se déplaceront en mode hors connexion.

Toutefois, le concept de *majoritaire* ne fonctionne correctement que si le nombre total de nœuds dans le cluster est impair (par exemple, trois nœuds dans un cluster à cinq nœuds). Alors, qu’en est-il des clusters avec un nombre pair de nœuds (par exemple, un cluster à quatre nœuds) ?

Il existe deux façons pour le cluster de rendre le *nombre total de votes* impair :

1. Tout d’abord, vous pouvez en *Ajouter un en* ajoutant un *témoin* avec un vote supplémentaire. Cela nécessite la configuration de l’utilisateur.
2.  Ou bien, elle peut *descendre* d’une unité en n’ayant aucun vote d’un nœud Unlucky (se produit automatiquement en fonction des besoins).

Chaque fois que les nœuds survivants vérifient qu’ils sont la *majorité*, la définition de la *majorité* est mise à jour de manière à ne se trouver qu’entre les survivants. Cela permet au cluster de perdre un nœud, puis un autre, puis un autre, et ainsi de suite. Ce concept du *nombre total de votes qui* s’adaptent après des échecs successifs est appelé ***quorum dynamique***.  

### <a name="dynamic-witness"></a>Témoin dynamique

Le témoin dynamique active le vote du témoin pour s’assurer que le *nombre total de votes* est impair. S’il existe un nombre impair de votes, le témoin n’a pas de vote. S’il existe un nombre pair de votes, le témoin a un vote. Le témoin dynamique réduit considérablement le risque que le cluster ne soit interrompu en raison d’un échec de témoin. Le cluster décide s’il faut utiliser le vote témoin en fonction du nombre de nœuds votants disponibles dans le cluster.

Le quorum dynamique fonctionne avec un témoin dynamique comme décrit ci-dessous.

### <a name="dynamic-quorum-behavior"></a>Comportement du quorum dynamique

- Si vous avez un nombre **pair** de nœuds et pas de témoin, *un nœud obtient son vote zéro*. Par exemple, seuls trois des quatre nœuds sont votes, le *nombre total de votes* est donc de trois et deux survivants avec votes sont considérés comme une majorité.
- Si vous avez un nombre **impair** de nœuds et aucun témoin, *ils sont tous des votes*.
- Si vous avez un nombre **pair** de nœuds et de témoins, *le témoin vote*, de sorte que le total est impair.
- Si vous avez un nombre **impair** de nœuds et de témoins, *le témoin ne vote pas*.

Le quorum dynamique permet d’attribuer dynamiquement un vote à un nœud afin d’éviter de perdre la majorité des votes et de permettre au cluster de s’exécuter avec un seul nœud (appelé dernier homme debout). Prenons un exemple de cluster à quatre nœuds. Supposons que le quorum requiert 3 votes. 

Dans ce cas, le cluster s’est arrêté si vous avez perdu deux nœuds.

![Diagramme montrant quatre nœuds de cluster, chacun d’entre eux ayant un vote](media/understand-quorum/dynamic-quorum-base.png)

Toutefois, le quorum dynamique empêche ce problème de se produire. Le *nombre total de votes* requis pour le quorum est maintenant déterminé en fonction du nombre de nœuds disponibles. Ainsi, avec le quorum dynamique, le cluster restera opérationnel même si vous perdez trois nœuds.

![Diagramme montrant quatre nœuds de cluster, avec des nœuds qui échouent un par un, et le nombre de votes requis en ajustant après chaque défaillance.](media/understand-quorum/dynamic-quorum-step-through.png)

Le scénario ci-dessus s’applique à un cluster général qui n’a pas espaces de stockage direct activé. Toutefois, lorsque espaces de stockage direct est activé, le cluster peut uniquement prendre en charge deux défaillances de nœud. Pour plus d’informations, voir la [section quorum du pool](#pool-quorum-overview).

### <a name="examples"></a>Exemples

#### <a name="two-nodes-without-a-witness"></a>Deux nœuds sans témoin. 
Le vote d’un nœud étant mis à zéro, le vote *majoritaire* est déterminé par un **vote**au total. Si le nœud de non-vote ne se déconnecte pas de façon inattendue, le survivant a 1/1 et le cluster a survécu. Si le nœud de vote tombe en panne de façon inattendue, le survivant a 0/1 et le cluster tombe en panne. Si le nœud de vote est mis hors tension en douceur, le vote est transféré vers l’autre nœud, et le cluster survivent. ***C’est la raison pour laquelle il est essentiel de configurer un témoin.***

![Explication du quorum dans le cas de deux nœuds sans témoin](media/understand-quorum/2-node-no-witness.png)

- Peut survivre à une défaillance de serveur : **50% de chance**.
- Peut survivre à une défaillance du serveur, puis à un autre : **No**.
- Peut survivre à deux défaillances de serveur à la fois : **No**. 

#### <a name="two-nodes-with-a-witness"></a>Deux nœuds avec un témoin. 
Les deux nœuds votent, plus les votes de témoin, la *majorité* est déterminée par un total de **3 votes**. Si l’un des nœuds tombe en panne, le survivant a 2/3 et le cluster a survécu.

![Explication du quorum dans le cas de deux nœuds avec témoin](media/understand-quorum/2-node-witness.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **No**.
- Peut survivre à deux défaillances de serveur à la fois : **No**. 

#### <a name="three-nodes-without-a-witness"></a>Trois nœuds sans témoin.
Comme tous les nœuds votent, la *majorité* est déterminée à partir de **3 votes**au total. Si un nœud tombe en panne, les survivants sont 2/3 et le cluster survivent. Le cluster devient deux nœuds sans témoin : à ce stade, vous êtes dans le scénario 1.

![Explication du quorum dans le cas avec trois nœuds sans témoin](media/understand-quorum/3-node-no-witness.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **50% de chance**.
- Peut survivre à deux défaillances de serveur à la fois : **No**. 

#### <a name="three-nodes-with-a-witness"></a>Trois nœuds avec un témoin.
Tous les nœuds votent, donc le témoin ne vote pas initialement. La *majorité* est déterminée à partir de **3 votes**au total. Après une défaillance, le cluster a deux nœuds avec un témoin, qui revient au scénario 2. À présent, les deux nœuds et le vote témoin.

![Explication du quorum dans le cas avec trois nœuds avec témoin](media/understand-quorum/3-node-witness.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **Oui**.
- Peut survivre à deux défaillances de serveur à la fois : **No**. 

#### <a name="four-nodes-without-a-witness"></a>Quatre nœuds sans témoin
Le vote d’un nœud est mis à zéro, donc la *majorité* est déterminée à partir de **3 votes**au total. Après une défaillance, le cluster devient trois nœuds, et vous êtes dans le scénario 3.

![Explication du quorum dans le cas de quatre nœuds sans témoin](media/understand-quorum/4-node-no-witness.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **Oui**.
- Peut survivre à deux défaillances de serveur à la fois : **50% de chance**. 

#### <a name="four-nodes-with-a-witness"></a>Quatre nœuds avec un témoin.
Tous les nœuds vote et les votes de témoin, donc la *majorité* est déterminée à partir de **5 votes**au total. Après une défaillance, vous êtes dans le scénario 4. Après deux échecs simultanés, vous passez au scénario 2.

![Explication du quorum dans le cas de quatre nœuds avec témoin](media/understand-quorum/4-node-witness.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **Oui**.
- Peut survivre à deux défaillances de serveur à la fois : **Oui**. 

#### <a name="five-nodes-and-beyond"></a>Cinq nœuds et au-delà.
Tous les nœuds votent, ou tous sauf un vote, quel que soit le total impair. Espaces de stockage direct ne peut pas gérer plus de deux nœuds de toute manière, à ce stade, aucun témoin n’est nécessaire ou utile.

![Explication du quorum dans le cas de cinq nœuds et au-delà](media/understand-quorum/5-nodes.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **Oui**.
- Peut survivre à deux défaillances de serveur à la fois : **Oui**. 

Maintenant que nous comprenons le fonctionnement du quorum, examinons les types de témoins de quorum.

### <a name="quorum-witness-types"></a>Types de témoin de quorum

Le clustering de basculement prend en charge trois types de témoin de quorum :

- **[Témoin Cloud](../../failover-clustering/deploy-cloud-witness.md)** -stockage BLOB dans Azure accessible par tous les nœuds du cluster. Il conserve les informations de clustering dans un fichier témoin. log, mais ne stocke pas de copie de la base de données de clusters.
- **Partage de fichiers témoin** : partage de fichiers SMB configuré sur un serveur de fichiers exécutant Windows Server. Il conserve les informations de clustering dans un fichier témoin. log, mais ne stocke pas de copie de la base de données de clusters.
- **Disque témoin** : un petit disque en cluster qui se trouve dans le groupe de stockage disponible du cluster. Ce disque est hautement disponible et peut basculer entre les nœuds. Il contient une copie de la base de données de clusters.  ***Un témoin de disque n’est pas pris en charge avec espaces de stockage direct***.

## <a name="pool-quorum-overview"></a>Vue d’ensemble du quorum de pool

Nous venons de parler du quorum de cluster, qui fonctionne au niveau du cluster. À présent, examinons le quorum de pool, qui fonctionne au niveau du pool (c’est-à-dire que vous pouvez perdre des nœuds et des lecteurs et faire en sorte que le pool reste opérationnel). Les pools de stockage ont été conçus pour être utilisés dans des scénarios en cluster et non cluster, ce qui explique pourquoi ils disposent d’un mécanisme de quorum différent.

Le tableau ci-dessous donne une vue d’ensemble des résultats du quorum de pool par scénario :

| Nœuds de serveur | Peut survivre à une défaillance de nœud de serveur | Peut survivre à un échec de nœud de serveur, puis à un autre | Peut survivre à deux échecs de nœuds de serveur simultanés |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | Non                                  | Non                                                | Non                                                 |
| 2 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 3            | Oui                                 | Non                                                | Non                                                 |
| témoin 3 +  | Oui                                 | Non                                                | Non                                                 |
| 4            | Oui                                 | Non                                                | Non                                                 |
| 4 + témoin  | Oui                                 | Oui                                               | Oui                                                |
| 5 et versions ultérieures  | Oui                                 | Oui                                               | Oui                                                |

## <a name="how-pool-quorum-works"></a>Fonctionnement du quorum de pool

En cas de défaillance des lecteurs ou lorsque certains sous-ensembles de lecteurs perdent leur contact avec un autre sous-ensemble, les lecteurs survivants doivent vérifier qu’ils constituent la *majorité* du pool pour rester en ligne. S’ils ne peuvent pas le vérifier, ils se déplaceront en mode hors connexion. Le pool est l’entité qui est hors connexion ou reste en ligne selon qu’elle dispose de suffisamment de disques pour le quorum (50% + 1). Le propriétaire de la ressource du pool (nœud de cluster actif) peut être le + 1.

Toutefois, le quorum de pool fonctionne différemment du quorum de cluster de la façon suivante :

- le pool utilise un nœud du cluster en tant que témoin pour résister à la moitié des lecteurs (ce nœud qui est le propriétaire de la ressource du pool).
- le pool n’a pas de quorum dynamique
- le pool n’implémente pas sa propre version de suppression d’un vote

### <a name="examples"></a>Exemples

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Quatre nœuds avec une disposition symétrique. 
Chacun des 16 lecteurs a un vote et le nœud 2 a également un vote (puisqu’il s’agit du propriétaire de la ressource du pool). La *majorité* est déterminée à partir de **16 votes**au total. Si les nœuds trois et quatre s’affichent, le sous-ensemble survivant a 8 lecteurs et le propriétaire de la ressource du pool, soit 9/16 votes. Par conséquent, le pool subsiste.

![Quorum de pool 1](media/understand-quorum/pool-1.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **Oui**.
- Peut survivre à deux défaillances de serveur à la fois : **Oui**. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Quatre nœuds avec une disposition symétrique et une défaillance de disque. 
Chacun des 16 lecteurs a un vote et le nœud 2 a également un vote (puisqu’il s’agit du propriétaire de la ressource du pool). La *majorité* est déterminée à partir de **16 votes**au total. Tout d’abord, le lecteur 7 tombe en panne. Si les nœuds trois et quatre s’affichent, le sous-ensemble survivant a 7 lecteurs et le propriétaire de la ressource du pool, soit 8/16 votes. Par conséquent, le pool n’a pas la majorité et tombe en panne.

![Quorum de pool 2](media/understand-quorum/pool-2.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance du serveur, puis à un autre : **No**.
- Peut survivre à deux défaillances de serveur à la fois : **No**. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Quatre nœuds avec une disposition non symétrique. 
Chacun des 24 disques dispose d’un vote et le nœud 2 a également un vote (puisqu’il s’agit du propriétaire de la ressource du pool). La *majorité* est déterminée à partir de **24 votes**au total. Si les nœuds trois et quatre s’affichent, le sous-ensemble survivant a 8 lecteurs et le propriétaire de la ressource du pool, soit 9/24 votes. Par conséquent, le pool n’a pas la majorité et tombe en panne.

![Quorum de pool 3](media/understand-quorum/pool-3.png)

- Peut survivre à une défaillance de serveur : **Oui**.
- Peut survivre à une défaillance de serveur, alors qu’un autre : * * dépend de * * (ne peut pas survivre si les deux nœuds, trois et quatre s’affichent, mais peut survivre à tous les autres scénarios.
- Peut survivre simultanément à deux défaillances de serveur : * * Depends * * (ne peut pas survivre si les deux nœuds sont en panne, mais peut survivre à tous les autres scénarios.

### <a name="pool-quorum-recommendations"></a>Recommandations de quorum de pool

- Assurez-vous que chaque nœud de votre cluster est symétrique (chaque nœud a le même nombre de lecteurs)
- Activez le miroir triple ou la parité double pour pouvoir tolérer les défaillances de nœud et garder les disques virtuels en ligne. Pour plus d’informations, consultez notre [page de conseils](plan-volumes.md) sur les volumes.
- Si plus de deux nœuds sont en baisse, ou si deux nœuds et un disque sur un autre nœud sont en état de défaillance, les volumes peuvent ne pas avoir accès aux trois copies de leurs données et donc être mis hors connexion et indisponibles. Il est recommandé de rétablir les serveurs ou de les remplacer rapidement pour garantir une résilience optimale pour toutes les données du volume.

## <a name="more-information"></a>Plus d’informations

- [Configurer et gérer le quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Déployer un témoin cloud](../../failover-clustering/deploy-cloud-witness.md)
