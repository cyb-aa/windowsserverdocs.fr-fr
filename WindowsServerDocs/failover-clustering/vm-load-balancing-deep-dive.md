---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: Présentation approfondie l’équilibrage de la Machine virtuelle
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 50213cf47c2c59f1775ae704e82ed51794715ac0
ms.sourcegitcommit: 276a480b470482cba4682caa3df4cd07ba5b7801
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66198551"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>Présentation approfondie l’équilibrage de la Machine virtuelle

> S’applique à : Windows Server 2019, Windows Server 2016

Le [fonctionnalité d’équilibrage de charge de Machine virtuelle](vm-load-balancing-overview.md) optimise l’utilisation de nœuds dans un Cluster de basculement. Ce document décrit comment configurer et contrôler l’équilibrage de la machine virtuelle. 

## <a id="heuristics-for-balancing"></a>Méthode heuristique pour équilibrage de charge
Équilibrage de charge de Machine virtuelle prend la valeur de charge d’un nœud en fonction de l’heuristique suivante :
1. Actuel **sollicitation de la mémoire**: La mémoire est la contrainte de ressource plus courantes sur un ordinateur hôte Hyper-V
2. Processeur **utilisation** du nœud moyenne calculée sur une fenêtre de 5 minutes : Réduit un nœud du cluster devient trop sollicités

## <a id="controlling-aggressiveness-of-balancing"></a>Contrôle l’intensité de l’équilibrage
L’intensité de l’équilibrage selon l’heuristique de la mémoire et le processeur peut être configurée à l’aide de la par la propriété commune de cluster « AutoBalancerLevel ». Pour contrôler l’intensité si nécessaire, exécutez la commande suivante dans PowerShell :

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Intensité | Comportement |
|-------------------|----------------|----------|
| 1 (par défaut) | Faible | Déplacer lorsque l’hôte est chargé plus de 80 % |
| 2 | Moyen | Déplacer lorsque l’hôte est chargé plus de 70 % |
| 3 | Élevée | Nombre moyen de nœuds et déplacer lorsque l’hôte est de plus de 5 % au-dessus de la moyenne | 

![Graphique d’un PowerShell de configuration de l’intensité de l’équilibrage](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>Contrôle de l’équilibrage de charge de machine virtuelle
Équilibrage de charge de machine virtuelle est activée par défaut et lorsque l’équilibrage de charge se produit peut être configuré par la propriété commune de cluster « AutoBalancerMode ». Pour contrôler quand des soldes d’équité de nœud du cluster :

### <a name="using-failover-cluster-manager"></a>À l’aide du Gestionnaire du Cluster de basculement :
1. Avec le bouton droit sur le nom de votre cluster et sélectionnez l’option « Propriétés »  
    ![Graphique de la sélection de propriété pour le cluster via le Gestionnaire de Cluster de basculement](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Sélectionnez le volet « Équilibreur de charge »  
    ![Graphique de l’option équilibreur via le Gestionnaire de Cluster de basculement](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>À l’aide de PowerShell :
Exécutez la commande suivante :
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportement| 
|:----------------:|:----------:|
|0| Désactivée| 
|1| Équilibrer la charge sur la jointure de nœud| 
|2 (valeur par défaut)| Équilibrer sur la jonction de nœud et toutes les 30 minutes |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>Visual Studio l’équilibrage de la machine virtuelle. Optimisation dynamique de Virtual Machine Manager de System Center
La fonctionnalité d’équité de nœud, fournit des fonctionnalités de l’emploi, qui sont destinée aux déploiements sans System Center Virtual Machine Manager (SCVMM). L’optimisation dynamique SCVMM est le mécanisme recommandé pour l’équilibrage de charge des machines virtuelles dans votre cluster pour les déploiements de SCVMM. SCVMM désactive automatiquement l’équilibrage de charge Windows Server machine virtuelle lors de l’optimisation dynamique est activée.

## <a name="see-also"></a>Voir aussi
* [Présentation de l’équilibrage de charge Machine virtuelle](vm-load-balancing-overview.md)
* [Clustering de basculement](failover-clustering-overview.md)
* [Vue d’ensemble d’Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
