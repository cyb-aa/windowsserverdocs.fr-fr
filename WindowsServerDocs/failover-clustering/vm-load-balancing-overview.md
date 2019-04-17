---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: "Vue d’ensemble de l’équilibrage de la Machine virtuelle"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 0a106db25d476088898b914481e6041f20ce2e9e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-overview"></a>Vue d’ensemble de l’équilibrage de la Machine virtuelle

> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Une préoccupation pour les déploiements de cloud privé est les dépenses de capital (<abbr title="de dépenses">CapEx</abbr>) obligé de passer en production. Il est très courant pour ajouter la redondance pour les déploiements de cloud privé afin d’éviter le dépassement de capacité lors des pics de trafic en production, mais cela augmente <abbr title="de dépenses">CapEx</abbr>. La nécessité pour assurer la redondance est pilotée par des clouds privés déséquilibrées où certains nœuds hébergent plus de Machines virtuelles (<abbr title="virtuels">Machines virtuelles</abbr>) et d’autres sont sous-utilisés (par exemple, un serveur fraîchement redémarrez).

## <a id="what-is-vm-load-balancing"></a>Quelle est l’équilibrage de la Machine virtuelle?
<abbr title="Ordinateur virtuel">Machine virtuelle</abbr> l’équilibrage de charge est une nouvelle fonctionnalité de boîte aux lettres dans Windows Server 2016 qui vous permet d’optimiser l’utilisation de nœuds dans un Cluster de basculement. Il identifie les nœuds trop sollicités et distribue nouveau <abbr title="virtuels">Machines virtuelles</abbr> à partir de ces nœuds aux nœuds sous dédiés. Parmi les aspects marquants de cette fonctionnalité sont les suivantes:

* *Il s’agit d’une solution sans interruption de service*: <abbr title="virtuels">Machines virtuelles</abbr> sont migrés vers des nœuds inactifs.
* *Intégration transparente avec votre environnement de cluster existant*: stratégies d’échec tels qu’anti-affinité, les domaines d’erreur et les propriétaires possibles sont respectées.
* *Méthode heuristique pour l’équilibrage de la*: <abbr title="machine virtuelle">Machine virtuelle</abbr> pression de mémoire et l’utilisation du processeur du nœud.
* *Contrôle granulaire*: activée par défaut. Peut être activé à la demande ou à un intervalle périodique.
* *Seuils d’agressivité*: trois seuils disponibles selon les caractéristiques de votre déploiement.

## <a id="feature-in-action"></a>La fonctionnalité en action
### <a id="new-node-added"></a>Un nouveau nœud est ajouté à votre Cluster de basculement
![Graphique d’un nouveau nœud ajouté à votre Cluster de basculement](media/vm-load-balancing/overview-VM-load-balancing-1.png)

Lorsque vous ajoutez de nouvelles capacités pour votre Cluster de basculement, le <abbr title="machine virtuelle">Machine virtuelle</abbr> fonctionnalité d’équilibrage de charge équilibre automatiquement la capacité à partir des nœuds existants, pour le nœud qui vient d’être ajouté dans l’ordre suivant:

1. La pression est évaluée sur les nœuds du Cluster de basculement existants.
2. Tous les nœuds qui dépasse le seuil sont identifiés.
3. Les nœuds avec la pression la plus élevée sont identifiés pour déterminer la priorité de l’équilibrage.
4. <abbr title="Les ordinateurs virtuels">Machines virtuelles</abbr> sont Live migration (aucun temps d’arrêt) à partir d’un nœud qui dépasse le seuil à un nœud du Cluster de basculement qui vient d’être ajouté.

### <a id="recurring-load-balancing"></a>Équilibrage de charge périodique
![Graphique d’un ordinateur virtuel périodique l’équilibrage de charge](media/vm-load-balancing/overview-VM-load-balancing-2.png)

Lorsque configuré pour l’équilibrage de la périodique, la pression sur les nœuds de cluster est évaluée pour l’équilibrage de toutes les 30 minutes. Sinon, la pression peut être évaluée sur demande. Voici le flux des étapes:

1. La pression est évaluée sur tous les nœuds dans le cloud privé.
2. Tous les nœuds qui dépasse le seuil et ceux dessous du seuil sont identifiés.
3. Les nœuds avec la pression la plus élevée sont identifiés pour déterminer la priorité de l’équilibrage.
4. <abbr title="Les ordinateurs virtuels">Machines virtuelles</abbr> sont Live migration (aucun temps d’arrêt) à partir d’un nœud qui dépasse le seuil de nœud sous le seuil minimal.

## <a name="see-also"></a>Voir aussi
* [Étudier l’équilibrage de la charge des machines virtuelles](vm-load-balancing-deep-dive.md)
* [Clustering de basculement](failover-clustering-overview.md)
* [Vue d’ensemble d’Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
