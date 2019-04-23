---
title: Quorum de cluster et pool de présentation
description: Présentation de Cluster et le Quorum de Pool, avec des exemples spécifiques pour accéder via les complexités.
keywords: Espaces de stockage Direct, Quorum, témoin, S2D, Cluster Quorum, le Quorum de Pool, Cluster, Pool
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879970"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Quorum de cluster et pool de présentation

>S’applique à : Windows Server 2019, Windows Server 2016

[Clustering de basculement Windows Server](../../failover-clustering/failover-clustering-overview.md) offre une haute disponibilité pour les charges de travail. Ces ressources sont considérés comme hautement disponibles si les nœuds qui hébergent les ressources sont Toutefois, le cluster requiert généralement plus de la moitié des nœuds pour être en cours d’exécution, ce qui est connu comme ayant *quorum*.

Quorum est conçu pour éviter *« split brain »* des scénarios qui peuvent se produire lorsqu’il existe une partition dans le réseau et des sous-ensembles de nœuds ne peuvent pas communiquer entre eux. Cela peut entraîner les deux sous-ensembles de nœuds pour essayer de propriétaire de la charge de travail et d’écrire dans le même disque, ce qui peut entraîner de nombreux problèmes. Toutefois, cette fonction est bloquée avec un concept du Clustering de basculement de quorum qui force uniquement un de ces groupes de nœuds pour continuer l’exécution, afin que seul l’un des ces groupes est restent en ligne.

Quorum détermine le nombre d’échecs que le cluster peut soutenir tout en restant en ligne. Quorum est conçu pour gérer le scénario lorsqu’il existe un problème de communication entre des sous-ensembles de nœuds de cluster, afin que plusieurs serveurs n’essayez pas d’héberger un groupe de ressources simultanément et d’écriture sur le même disque en même temps. En ayant ce concept de quorum, le cluster force le service de cluster à arrêter dans un des sous-ensembles de nœuds pour vous assurer qu’il n'existe qu’un seul véritable propriétaire d’un groupe de ressources particulier. Une fois que les nœuds qui ont été arrêtés peuvent là aussi communiquer avec le groupe principal de nœuds, ils seront automatiquement rejoindre le cluster de leur service de cluster.

Dans Windows Server 2016, il existe deux composants du système qui possèdent leurs propres mécanismes de quorum :

- <strong>Quorum du cluster</strong>: Cela fonctionne au niveau du cluster (par exemple, vous pouvez perdre des nœuds et le cluster reste opérationnelle)
- <strong>Pool Quorum</strong>: Cela fonctionne au niveau du pool lorsque les espaces de stockage Direct est activé (par exemple, vous pouvez perdre des nœuds et des disques et le pool de rester opérationnel). Pools de stockage ont été conçus pour être utilisé dans les scénarios de cluster et non-cluster, c’est pourquoi ils ont un mécanisme de quorum différents.

## <a name="cluster-quorum-overview"></a>Vue d’ensemble du quorum de cluster

Le tableau ci-dessous donne une vue d’ensemble de résultats de Quorum du Cluster par scénario :

| Nœuds de serveur | Peut survivre à une défaillance de nœud de serveur | Peut survivre à défaillance de nœud d’un serveur, puis un autre | Peut survivre à deux défaillances de nœud de serveur simultanées |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | Non                                                | Non                                                 |
| 2 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 3            | Oui                                 | 50/50                                             | Non                                                 |
| 3 + témoin  | Oui                                 | Oui                                               | Non                                                 |
| 4            | Oui                                 | Oui                                               | 50/50                                              |
| 4 + témoin  | Oui                                 | Oui                                               | Oui                                                |
| 5 et versions ultérieures  | Oui                                 | Oui                                               | Oui                                                |

### <a name="cluster-quorum-recommendations"></a>Recommandations de quorum de cluster

- Si vous avez deux nœuds, un témoin est <strong>requis</strong>.
- Si vous avez trois ou quatre nœuds, le témoin est <strong>fortement recommandé</strong>.
- Si vous avez accès à Internet, utilisez un  <strong>[cloud témoin](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Si vous êtes dans un environnement informatique avec d’autres ordinateurs et les partages de fichiers, utiliser un témoin de partage de fichiers

## <a name="how-cluster-quorum-works"></a>Le fonctionnement du quorum de cluster

Lorsque les nœuds échouent, ou lorsqu’un sous-ensemble de nœuds perd le contact avec un autre sous-ensemble, nœuds survivants doivent vérifier qu’ils constituent le *majorité* du cluster de rester en ligne. Si elles ne peuvent pas vérifier que, rétorquent hors connexion.

Mais le concept de *majorité* uniquement fonctionne correctement lorsque le nombre total de nœuds du cluster est impair (par exemple, trois nœuds dans un cluster à cinq nœuds). Par conséquent, qu’en est-il des clusters avec un nombre pair de nœuds (par exemple, un cluster à quatre nœuds) ?

Il existe deux façons le cluster peut rendre le *nombre total de votes* impair :

1. Tout d’abord, il peut aller *des* une en ajoutant un *témoin* avec un vote supplémentaire. Cela nécessite la configuration de l’utilisateur.
2.  Vous pouvez aussi, il peut *vers le bas* un en mettant à zéro le vote d’un nœud dans d’autres (se produit automatiquement en fonction des besoins).

Chaque fois que les nœuds survivants correctement vérifier qu’ils sont le *majorité*, la définition de *majorité* est mis à jour pour être entre simplement les survivants. Cela permet au cluster à perdre un nœud, puis un autre, puis un autre et ainsi de suite. Ce concept de la *nombre total de votes* adaptation après des échecs consécutifs est appelé  <strong>*quorum dynamique*</strong>.  

### <a name="dynamic-witness"></a>Témoin dynamique

Témoin dynamique active ou désactive le vote du témoin pour vous assurer que le *nombre total de votes* est impair. S’il existe un nombre impair de voix, le témoin n’a pas un vote. En l’absence d’un nombre pair de votes, le témoin dispose d’un vote. Témoin dynamique réduit considérablement le risque que le cluster va être arrêté en raison de l’échec de témoin. Le cluster décide s’il faut utiliser le vote témoin en fonction du nombre de nœuds votants qui sont disponibles dans le cluster.

Quorum dynamique fonctionne avec témoin dynamique de la façon décrite ci-dessous.

### <a name="dynamic-quorum-behavior"></a>Comportement du quorum dynamique

- Si vous avez un <strong>même</strong> nombre de nœuds et aucun témoin, *un seul nœud obtient son vote remis à zéro*. Par exemple, seulement trois des quatre nœuds obtiennent voix, donc la *nombre total de votes* est trois, et deux survivants avec les votes sont considérés comme une majorité.
- Si vous avez un <strong>impair</strong> nombre de nœuds et aucun témoin, *pouvoir tous les votes*.
- Si vous avez un <strong>même</strong> nombre de nœuds ainsi que le témoin, *le témoin votes*, de sorte que le total est impair.
- Si vous avez un <strong>impair</strong> nombre de nœuds ainsi que le témoin, *le témoin ne vote*.

Quorum dynamique vous permettent d’assigner un vote à un nœud dynamiquement pour éviter de perdre la majorité des votes et permettre au cluster de s’exécuter avec un seul nœud (appelé permanent de la dernière-man). Prenons un cluster à quatre nœuds comme un exemple. Supposons que quorum nécessite 3 votes. 

Dans ce cas, le cluster auraient vers le bas si vous avez perdu à deux nœuds.

![Diagramme montrant quatre nœuds de cluster, chacun d’eux Obtient un vote](media/understand-quorum/dynamic-quorum-base.png)

Toutefois, quorum dynamique permet d’éviter ce problème. Le *nombre total de votes* requis pour le quorum est maintenant déterminé en fonction du nombre de nœuds disponibles. Par conséquent, avec quorum dynamique, le cluster continuera même si vous perdez des trois nœuds.

![Diagramme montrant quatre nœuds de cluster, avec des nœuds échouent une à la fois et le nombre de votes requis ajustant après chaque échec.](media/understand-quorum/dynamic-quorum-step-through.png)

Le scénario ci-dessus s’applique à un cluster général dépourvu d’espaces de stockage Direct est activé. Toutefois, espaces de stockage Direct est activé, le cluster peut uniquement prendre en charge deux défaillances de nœud. Cela est expliqué plus dans le [section de quorum de pool](#poolQuorum).

### <a name="examples"></a>Exemples

#### <a name="two-nodes-without-a-witness"></a>Deux nœuds sans témoin. 
Vote d’un nœud est remis à zéro, afin que la *majorité* vote est déterminé sur un total de <strong>1 vote</strong>. Si le nœud non-votant s’arrête inopinément, survivant a 1/1 et le cluster survit. Si le vote du nœud tombe en panne inattendue, survivant a 0/1 et le cluster tombe en panne. Si le nœud de vote est mis hors tension normalement, le vote est transféré vers l’autre nœud et le cluster survit. *<strong>C’est pourquoi il est essentiel pour configurer un témoin.</strong>*

![Expliqué dans le cas avec deux nœuds sans témoin de quorum](media/understand-quorum/2-node-no-witness.png)

- Peut survivre à une défaillance du serveur : <strong>50 % de chance</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>No</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>No</strong>. 

#### <a name="two-nodes-with-a-witness"></a>Deux nœuds avec un témoin. 
Les deux nœuds votants, ainsi que les votes le témoin, la *majorité* est déterminée sur un total de <strong>3 votes</strong>. Si des nœuds tombe en panne, survivant a 2/3 et le cluster survit.

![Expliqué dans le cas avec deux nœuds avec un témoin de quorum](media/understand-quorum/2-node-witness.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>No</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>No</strong>. 

#### <a name="three-nodes-without-a-witness"></a>Trois nœuds sans témoin.
Tous les nœuds de votent, afin que la *majorité* est déterminée sur un total de <strong>3 votes</strong>. Si n’importe quel nœud tombe en panne, les survivants sont 2/3 et le cluster survit. Le cluster présente deux nœuds sans témoin : à ce stade, vous êtes dans le scénario 1.

![Expliqué dans le cas avec trois nœuds sans témoin de quorum](media/understand-quorum/3-node-no-witness.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>50 % de chance</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>No</strong>. 

#### <a name="three-nodes-with-a-witness"></a>Trois nœuds avec un témoin.
Tous les nœuds de votent, donc le témoin ne vote initialement. Le *majorité* est déterminée sur un total de <strong>3 votes</strong>. Après un échec, le cluster a deux nœuds avec un témoin – ce qui correspond au scénario 2. Donc, maintenant les deux nœuds et le témoin votent.

![Expliqué dans le cas avec trois nœuds avec un témoin de quorum](media/understand-quorum/3-node-witness.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>Oui</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>No</strong>. 

#### <a name="four-nodes-without-a-witness"></a>Quatre nœuds sans témoin
Vote d’un nœud est remis à zéro, afin que la *majorité* est déterminée sur un total de <strong>3 votes</strong>. Après un échec, le cluster présente trois nœuds, et vous êtes dans le scénario 3.

![Expliqué dans le cas avec quatre nœuds sans témoin de quorum](media/understand-quorum/4-node-no-witness.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>Oui</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>50 % de chance</strong>. 

#### <a name="four-nodes-with-a-witness"></a>Quatre nœuds avec un témoin.
Tous les votes de nœuds et les votes de témoin, donc la *majorité* est déterminée sur un total de <strong>5 votes</strong>. Après un échec, vous êtes dans le scénario 4. Après deux défaillances simultanées, vous passez à scénario 2.

![Expliqué dans le cas avec quatre nœuds avec un témoin de quorum](media/understand-quorum/4-node-witness.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>Oui</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>Oui</strong>. 

#### <a name="five-nodes-and-beyond"></a>Cinq nœuds et au-delà.
Tous les nœuds votants, ou un seul vote, quelle que soit, le total impair. Espaces de stockage Direct ne peut pas gérer plus de deux nœuds vers le bas en tout cas, par conséquent, à ce stade, aucun témoin n’est nécessaire ou utile.

![Quorum expliqué dans le cas de cinq nœuds et au-delà](media/understand-quorum/5-nodes.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>Oui</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>Oui</strong>. 

Maintenant que nous comprenons le fonctionnement du quorum, examinons les types de témoins de quorum.

### <a name="quorum-witness-types"></a>Types de témoin de quorum

Le Clustering de basculement prend en charge trois types de témoins de Quorum :

- <strong>[Témoin de cloud](../../failover-clustering\deploy-cloud-witness.md)</strong>  -le stockage Blob en Azure accessible par tous les nœuds du cluster. Il conserve les informations de mise en cluster dans un fichier witness.log, mais ne stocke pas une copie de la base de données de cluster.
- <strong>Le témoin de partage de fichiers</strong> – partage de fichiers SMB de A qui est configuré sur un serveur de fichiers exécutant Windows Server. Il conserve les informations de mise en cluster dans un fichier witness.log, mais ne stocke pas une copie de la base de données de cluster.
- <strong>Disque témoin</strong> -un petit disque en cluster qui se trouve dans le groupe de stockage disponible du Cluster. Ce disque est hautement disponible et peut basculer entre les nœuds. Il contient une copie de la base de données de cluster.  <strong>*Un témoin de disque n’est pas pris en charge avec les espaces de stockage Direct*</strong>.

## <a id="poolQuorum"></a>Vue d’ensemble du quorum de pool

Nous venons de parler de Quorum du Cluster, qui fonctionne au niveau du cluster. À présent, examinons le Quorum de Pool, qui fonctionne au niveau du pool (par exemple, vous pouvez perdre des nœuds et lecteurs et avez le pool de rester opérationnel). Pools de stockage ont été conçus pour être utilisé dans les scénarios de cluster et non-cluster, c’est pourquoi ils ont un mécanisme de quorum différents.

Le tableau ci-dessous donne une vue d’ensemble de résultats de Quorum de Pool par scénario :

| Nœuds de serveur | Peut survivre à une défaillance de nœud de serveur | Peut survivre à défaillance de nœud d’un serveur, puis un autre | Peut survivre à deux défaillances de nœud de serveur simultanées |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | Non                                  | Non                                                | Non                                                 |
| 2 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 3            | Oui                                 | Non                                                | Non                                                 |
| 3 + témoin  | Oui                                 | Non                                                | Non                                                 |
| 4            | Oui                                 | Non                                                | Non                                                 |
| 4 + témoin  | Oui                                 | Oui                                               | Oui                                                |
| 5 et versions ultérieures  | Oui                                 | Oui                                               | Oui                                                |

## <a name="how-pool-quorum-works"></a>Le fonctionnement du quorum de pool

Cas d’échouent des lecteurs, ou quand un sous-ensemble des lecteurs perd le contact avec un autre sous-ensemble, lecteurs survivants doivent avoir vérifier qu’ils constituent le *majorité* du pool de rester en ligne. Si elles ne peuvent pas vérifier que, rétorquent hors connexion. Le pool est l’entité qui est mis hors connexion ou reste en ligne selon qu’il dispose de suffisamment de disques pour le quorum (50 % + 1). Le propriétaire de ressource du pool (nœud de cluster actif) peut être le + 1.

Mais le quorum de pool fonctionne différemment de quorum du cluster comme suit :

- le pool utilise un seul nœud dans le cluster en tant que témoin comme décisive de résister à la moitié des lecteurs disparus (ce nœud qui est le propriétaire de ressource de pool)
- le pool n’a pas de quorum dynamique
- le pool n’implémente pas sa propre version de la suppression d’un vote

### <a name="examples"></a>Exemples

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Quatre nœuds avec une disposition symétrique. 
Chacun des 16 disques a un vote et nœud deux a également un vote (dans la mesure où il est propriétaire de la ressource de pool). Le *majorité* est déterminée sur un total de <strong>16 votes</strong>. Si les nœuds trois et quatre tombent en panne, le sous-ensemble survivant a 8 disques et le propriétaire de la ressource de pool, est 9/16 votes. Par conséquent, le pool survit.

![Pool Quorum 1](media/understand-quorum/pool-1.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>Oui</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>Oui</strong>. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Quatre nœuds avec un échec de mise en page et lecteur symétrique. 
Chacun des 16 disques a un vote et nœud 2 a également un vote (dans la mesure où il est propriétaire de la ressource de pool). Le *majorité* est déterminée sur un total de <strong>16 votes</strong>. Tout d’abord, le lecteur 7 tombe en panne. Si les nœuds trois et quatre tombent en panne, le sous-ensemble survivant a 7 disques et le propriétaire de la ressource de pool, est de 8/16 votes. Par conséquent, le pool n’a pas majorité et tombe en panne.

![Pool Quorum 2](media/understand-quorum/pool-2.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>No</strong>.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>No</strong>. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Quatre nœuds avec une disposition non symétrique. 
Chacun des 24 disques a un vote et nœud deux a également un vote (dans la mesure où il est propriétaire de la ressource de pool). Le *majorité* est déterminée sur un total de <strong>24 votes</strong>. Si les nœuds trois et quatre tombent en panne, le sous-ensemble survivant a 8 disques et le propriétaire de la ressource de pool, est 9/24 votes. Par conséquent, le pool n’a pas majorité et tombe en panne.

![Pool Quorum 3](media/understand-quorum/pool-3.png)

- Peut survivre à une défaillance du serveur : <strong>Oui</strong>.
- Peut survivre à la défaillance d’un serveur, puis une autre : <strong>Varie</strong> (ne peut pas survivre à si les deux nœuds, trois et quatre tombent en panne, mais peuvent survivre à tous les autres scénarios.
- Peut surmonter les défaillances de serveur deux à la fois : <strong>Varie</strong> (ne peut pas survivre à si les deux nœuds, trois et quatre tombent en panne, mais peuvent survivre à tous les autres scénarios.

### <a name="pool-quorum-recommendations"></a>Recommandations de quorum de pool

- Assurez-vous que chaque nœud dans votre cluster est symétrique (chaque nœud a le même nombre de disques)
- Activer le miroir trois voies ou double parité afin que vous pouvez tolérer un défaillances de nœud et conserver les disques virtuels en ligne. Consultez notre [page du Guide de volume](plan-volumes.md) pour plus d’informations.
- Si plus de deux nœuds sont arrêtés ou deux nœuds et un disque sur un autre nœud sont arrêtés, volumes n’aient pas accès à toutes les trois copies de leurs données et par conséquent être mis hors connexion et ne pas être disponibles. Il est recommandé pour ramener les serveurs ou de remplacer les disques rapidement pour garantir la résilience de la plupart pour toutes les données dans le volume.

## <a name="more-information"></a>Informations supplémentaires

- [Configurer et gérer le quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Déployer un témoin cloud](../../failover-clustering/deploy-cloud-witness.md)
