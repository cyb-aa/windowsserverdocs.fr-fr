---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Exploration approfondie de l’équilibrage de charge des machines virtuelles
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 972e86d5f49f92d090eed1d4130544d0269c1309
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392220"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Exploration approfondie de l’équilibrage de charge des machines virtuelles

> S’applique à : Windows Server 2019, Windows Server 2016

La [fonctionnalité d’équilibrage de charge des machines virtuelles](vm-load-balancing-overview.md) optimise l’utilisation des nœuds dans un cluster de basculement. Ce document décrit comment configurer et contrôler l’équilibrage de charge de la machine virtuelle. 

## <a id="heuristics-for-balancing"></a>Heuristiques pour l’équilibrage
L’équilibrage de charge de la machine virtuelle évalue la charge d’un nœud en fonction de l’heuristique suivante :
1. **Sollicitation**de la mémoire actuelle : La mémoire est la contrainte de ressource la plus courante sur un hôte Hyper-V
2. **Utilisation** de l’UC du nœud moyenne sur une fenêtre de 5 minutes : Atténue un nœud dans le cluster qui devient trop-validé

## <a id="controlling-aggressiveness-of-balancing"></a>Contrôle de l’intensité de l’équilibrage
L’intensité de l’équilibrage basée sur les heuristiques de mémoire et de processeur peut être configurée à l’aide de la propriété commune de cluster « AutoBalancerLevel ». Pour contrôler l’intensité, exécutez la commande suivante dans PowerShell :

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Intensité | Comportement |
|-------------------|----------------|----------|
| 1 (par défaut) | Faible | Déplacer lorsque l’ordinateur hôte est plus de 80% chargé |
| 2 | Moyenne | Déplacer lorsque l’ordinateur hôte est plus de 70% chargé |
| 3 | Élevé | Nombre moyen de nœuds et déplacement lorsque l’hôte est supérieur à 5% au-dessus de la moyenne | 

![Graphique d’un PowerShell de la configuration de l’intensité de l’équilibrage](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Contrôle de l’équilibrage de charge des machines virtuelles
L’équilibrage de charge de la machine virtuelle est activé par défaut et lorsque l’équilibrage de charge se produit peut être configuré par la propriété commune de cluster « AutoBalancerMode ». Pour contrôler le moment où l’équité du nœud équilibre le cluster :

### <a name="using-failover-cluster-manager"></a>Utilisation de Gestionnaire du cluster de basculement :
1. Cliquez avec le bouton droit sur le nom de votre cluster et sélectionnez l’option « Propriétés ».  
    ![Graphique de sélection de la propriété pour le cluster à l’aide de Gestionnaire du cluster de basculement](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Sélectionner le volet « équilibreur »  
    ![Graphique de la sélection de l’option d’équilibreur via Gestionnaire du cluster de basculement](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>À l’aide de PowerShell :
Exécutez la commande suivante :
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportement| 
|:----------------:|:----------:|
|0| Désactivé| 
|1| Équilibrer la charge sur la jointure de nœud| 
|2 (par défaut)| Équilibrer la charge sur la jonction de nœud et toutes les 30 minutes |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Équilibrage de charge des machines virtuelles et Optimisation dynamique System Center Virtual Machine Manager
La fonctionnalité d’équité du nœud fournit une fonctionnalité intégrée, qui est ciblée vers les déploiements sans System Center Virtual Machine Manager (SCVMM). L’optimisation dynamique SCVMM est le mécanisme recommandé pour équilibrer la charge des machines virtuelles dans votre cluster pour les déploiements SCVMM. SCVMM désactive automatiquement l’équilibrage de charge de la machine virtuelle Windows Server lorsque l’optimisation dynamique est activée.

## <a name="see-also"></a>Voir aussi
* [Vue d’ensemble de l’équilibrage de charge des machines virtuelles](vm-load-balancing-overview.md)
* [Clustering de basculement](failover-clustering-overview.md)
* [Vue d’ensemble d’Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
