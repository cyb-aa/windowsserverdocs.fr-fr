---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: Vue d’ensemble de l’équilibrage de la Machine virtuelle
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 125dd7421cc1876c07983016498a9689d8a507ac
ms.sourcegitcommit: a3c9a7718502de723e8c156288017de465daaf6b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/19/2019
ms.locfileid: "65475989"
---
# <a name="virtual-machine-load-balancing-overview"></a>Vue d’ensemble de l’équilibrage de la Machine virtuelle

> S’applique à : Windows Server 2019, Windows Server 2016

Un aspect clé pour les déploiements de cloud privé est le (dépenses d’investissement<abbr title="dépenses d’investissement">CapEx</abbr>) obligé de passer en production. Il est très courant pour intégrer la redondance pour les déploiements de cloud privé pour éviter le dépassement de capacité pendant les pics de trafic en production, mais cela augmente <abbr title="dépenses d’investissement">CapEx</abbr>. Le besoin de redondance est piloté par les clouds privés déséquilibrées où certains nœuds hébergent plus (Machines virtuelles<abbr title="ordinateurs virtuels">Ordinateurs virtuels</abbr>) et d’autres sont sous-utilisées (par exemple, un serveur qui vient d’être redémarré).

<strong>Courte vidéo de présentation</strong><br>(6 minutes)<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>Quelle est l’équilibrage de la Machine virtuelle ?
<abbr title="Ordinateur virtuel">VM</abbr> Équilibrage de la charge est une fonctionnalité de l’emploi dans Windows Server 2019 et Windows Server 2016 qui vous permet d’optimiser l’utilisation de nœuds dans un Cluster de basculement. Il identifie les nœuds sollicités et ré-distribue <abbr title="ordinateurs virtuels">Ordinateurs virtuels</abbr> à partir de ces nœuds pour les nœuds sous validée. Certains aspects marquants de cette fonctionnalité sont les suivantes :

* *C’est une solution sans interruption de service*: <abbr title="Ordinateurs virtuels">Ordinateurs virtuels</abbr> sont migrés vers les nœuds inactifs.
* *Intégration transparente avec votre environnement de cluster existant*: Stratégies de défaillance tels qu’anti-affinité, les domaines d’erreur et les propriétaires possibles sont respectées.
* *Méthode heuristique pour l’équilibrage*: <abbr title="Ordinateur virtuel">VM</abbr> sollicitation de la mémoire et du processeur du nœud.
* *Contrôle granulaire*: Activé par défaut. Peut être activé à la demande ou selon un intervalle périodique.
* *Seuils de l’intensité*: Trois seuils disponibles en fonction des caractéristiques de votre déploiement.

## <a id="feature-in-action"></a>La fonctionnalité en action
### <a id="new-node-added"></a>Un nouveau nœud est ajouté à votre Cluster de basculement
![Graphique d’un nouveau nœud ajouté à votre Cluster de basculement](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Lorsque vous ajoutez de nouvelles capacités à votre Cluster de basculement, le <abbr title="ordinateur virtuel">VM</abbr> Fonctionnalité d’équilibrage de charge équilibre automatiquement la capacité des nœuds existants, pour le nœud qui vient d’être ajouté dans l’ordre suivant :

1. La pression est évaluée sur les nœuds existants du Cluster de basculement.
2. Tous les nœuds dépassant le seuil sont identifiés.
3. Les nœuds avec la pression la plus élevée sont identifiés pour déterminer la priorité de l’équilibrage.
4. <abbr title="Ordinateurs virtuels">Ordinateurs virtuels</abbr> sont Live migration (aucun temps d’arrêt) à partir d’un nœud dépassant le seuil à un nœud qui vient d’être ajouté dans le Cluster de basculement.

### <a id="recurring-load-balancing"></a>Équilibrage de charge périodique
![Graphique d’un équilibrage de charge de machine virtuelle récurrents](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Lorsque configuré pour l’équilibrage périodiques, la pression sur les nœuds de cluster est évaluée pour l’équilibrage de toutes les 30 minutes. Ou bien, la pression peut être évaluée à la demande. Voici le flux des étapes :

1. La pression est évaluée sur tous les nœuds dans le cloud privé.
2. Tous les nœuds dépassant le seuil et ceux sous seuil sont identifiés.
3. Les nœuds avec la pression la plus élevée sont identifiés pour déterminer la priorité de l’équilibrage.
4. <abbr title="Ordinateurs virtuels">Ordinateurs virtuels</abbr> sont Live migration (aucun temps d’arrêt) à partir d’un nœud de dépassement du seuil de nœud sous le seuil minimal.

## <a name="see-also"></a>Voir aussi
* [Présentation approfondie de l’équilibrage de la charge des machines virtuelles](vm-load-balancing-deep-dive.md)
* [Clustering de basculement](failover-clustering-overview.md)
* [Vue d’ensemble d’Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
