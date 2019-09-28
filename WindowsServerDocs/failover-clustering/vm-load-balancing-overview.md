---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Vue d’ensemble de l’équilibrage de charge des machines virtuelles
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 1fea9e6297399a5081eb8fef1876f1b11d23c745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360981"
---
# <a name="virtual-machine-load-balancing-overview"></a>Vue d’ensemble de l’équilibrage de charge des machines virtuelles

> S’applique à : Windows Server 2019, Windows Server 2016

Une préoccupation essentielle pour les déploiements de clouds privés est la dépense en capital (<abbr title="dépenses d’investissement">Investissement</abbr>) requis pour passer en production. Il est très courant d’ajouter une redondance aux déploiements de cloud privé afin d’éviter une sous-capacité pendant le trafic de pointe en production, mais cela augmente <abbr title="dépenses d’investissement">Investissement</abbr>. Le besoin de redondance est piloté par des clouds privés déséquilibrés, où certains nœuds hébergent davantage de machines virtuelles (<abbr title="ordinateurs virtuels">Ordinateurs virtuels</abbr>) et d’autres sont sous-exploités (par exemple, un serveur redémarré récemment).

<strong>Vue d’ensemble de la vidéo rapide</strong><br>(6 minutes)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>Qu’est-ce que l’équilibrage de charge des machines virtuelles ?
<abbr title="Ordinateur virtuel">VM</abbr> L’équilibrage de charge est une fonctionnalité intégrée de Windows Server 2019 et Windows Server 2016 qui vous permet d’optimiser l’utilisation des nœuds dans un cluster de basculement. Il identifie les nœuds survalidés et les redistributions <abbr title="ordinateurs virtuels">Ordinateurs virtuels</abbr> à partir de ces nœuds vers des nœuds sous-validés. Voici quelques-uns des aspects les plus saillants de cette fonctionnalité :

* *Il s’agit d’une solution sans interruption de service*: <abbr title="Ordinateurs virtuels">Ordinateurs virtuels</abbr> sont migrés en direct vers des nœuds inactifs.
* *Intégration transparente à votre environnement de cluster existant*: Les stratégies d’échec, telles que l’anti-affinité, les domaines d’erreur et les propriétaires possibles, sont honorées.
* *Heuristiques pour l’équilibrage*: <abbr title="Ordinateur virtuel">VM</abbr> sollicitation de la mémoire et utilisation de l’UC du nœud.
* *Contrôle granulaire*: Activé par défaut. Peut être activé à la demande ou à intervalles réguliers.
* *Seuils d’intensité*: Trois seuils disponibles en fonction des caractéristiques de votre déploiement.

## <a id="feature-in-action"></a>Fonctionnalité en action
### <a id="new-node-added"></a>Un nouveau nœud est ajouté à votre cluster de basculement
![Graphique d’un nouveau nœud ajouté à votre cluster de basculement](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Lorsque vous ajoutez une nouvelle capacité à votre cluster de basculement, le <abbr title="ordinateur virtuel">VM</abbr> La fonctionnalité d’équilibrage de charge équilibre automatiquement la capacité des nœuds existants vers le nœud que vous venez d’ajouter dans l’ordre suivant :

1. La pression est évaluée sur les nœuds existants dans le cluster de basculement.
2. Tous les nœuds dépassant le seuil sont identifiés.
3. Les nœuds avec la pression la plus élevée sont identifiés pour déterminer la priorité de l’équilibrage.
4. <abbr title="Ordinateurs virtuels">Ordinateurs virtuels</abbr> sont migrés dynamiquement (sans temps d’inactivité) à partir d’un nœud dépassant le seuil vers un nœud nouvellement ajouté dans le cluster de basculement.

### <a id="recurring-load-balancing"></a>Équilibrage de charge récurrent
![Graphique d’un équilibrage de charge de machine virtuelle périodique](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Lorsqu’elle est configurée pour un équilibrage régulier, la pression sur les nœuds du cluster est évaluée pour l’équilibrage toutes les 30 minutes. Vous pouvez également évaluer la pression à la demande. Voici le déroulement des étapes :

1. La pression est évaluée sur tous les nœuds du Cloud privé.
2. Tous les nœuds dépassant le seuil et les seuils inférieurs sont identifiés.
3. Les nœuds avec la pression la plus élevée sont identifiés pour déterminer la priorité de l’équilibrage.
4. <abbr title="Ordinateurs virtuels">Ordinateurs virtuels</abbr> sont migrés dynamiquement (sans temps d’inactivité) à partir d’un nœud qui dépasse le seuil vers le nœud sous le seuil minimal.

## <a name="see-also"></a>Voir aussi
* [Exploration approfondie de l’équilibrage de charge des machines virtuelles](vm-load-balancing-deep-dive.md)
* [Clustering de basculement](failover-clustering-overview.md)
* [Vue d’ensemble d’Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
