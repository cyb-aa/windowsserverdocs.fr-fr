---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: "L’équilibrage de la Machine virtuelle en profondeur au"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 6ee092a2e51e2181a2203bc263377c4a8ec60246
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>L’équilibrage de la Machine virtuelle en profondeur au

> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Windows Server 2016 introduit le [fonctionnalité d’équilibrage de charge d’ordinateur virtuel](vm-load-balancing-overview.md) pour optimiser l’utilisation de nœuds dans un Cluster de basculement. Ce document décrit comment configurer et contrôler <abbr title="machine virtuelle">Machine virtuelle</abbr> l’équilibrage de charge. 

## <a id="heuristics-for-balancing"></a>Méthode heuristique pour équilibrage de charge
<abbr title="Ordinateur virtuel">Machine virtuelle</abbr> l’équilibrage de la Machine virtuelle évalue charge d’un nœud basé sur l’heuristique suivante:
1. En cours **pression de mémoire**: mémoire est la contrainte de ressources les plus courants sur un ordinateur hôte Hyper-V
2. <abbr title="Unité centrale">Processeur</abbr> **utilisation** du nœud était en moyenne sur une fenêtre de 5 minutes: réduit un nœud du cluster devient trop sollicité

## <a id="controlling-aggressiveness-of-balancing"></a>Contrôle l’intensité de l’équilibrage de
L’intensité de l’équilibrage de la fonction l’heuristique de la mémoire et du processeur peut être configurée à l’aide de la par la propriété commune de cluster «AutoBalancerLevel». Pour contrôler l’intensité, exécutez la commande suivante dans PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | Intensité | Comportement |
|-------------------|----------------|----------|
| 1 (par défaut) | Faible | Déplacer lorsque l’hôte est chargé à plus de 80 % |
| 2 | Moyenne | Déplacer lorsque l’hôte est chargé à plus de 70 % |
| 3 | Élevé | Déplacer lorsque l’ordinateur hôte est chargé à plus de 60 % | 

![Graphique d’une PowerShell de configuration de l’intensité de l’équilibrage de](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>Contrôle <abbr title="Machine virtuelle">Machine virtuelle</abbr> l’équilibrage de charge
<abbr title="Ordinateur virtuel">Machine virtuelle</abbr> l’équilibrage de charge est activée par défaut et lors de l’équilibrage de charge peut être configuré par la propriété commune de cluster «AutoBalancerMode». Pour contrôler quand des soldes d’équité des nœuds du cluster:

### <a name="using-failover-cluster-manager"></a>À l’aide du Gestionnaire du Cluster de basculement:
1. Avec le bouton droit sur le nom du cluster et sélectionnez l’option «Propriétés»  
    ![Graphique de la sélection de propriété pour un cluster par le biais de gestionnaire du Cluster de basculement](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  Sélectionnez le volet d’équilibrage de la «»  
    ![Graphique de l’option équilibrage par le biais de gestionnaire du Cluster de basculement](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>À l’aide de PowerShell:
Exécutez la commande suivante:
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |Comportement| 
|:----------------:|:----------:|
|0| Désactivé| 
|1| Équilibrage de la charge sur la jonction de nœud| 
|2 (par défaut)| Équilibrer sur la jonction de nœud et toutes les 30 minutes |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="Ordinateur virtuel">Machine virtuelle</abbr> et l’optimisation de System Center Virtual Machine Manager dynamique équilibrage de la charge
La fonctionnalité équité des nœuds, fournit des fonctionnalités, qui sont ciblée vers des déploiements sans System Center Virtual Machine Manager (<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>). <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> l’optimisation dynamique est la méthode recommandée pour l’équilibrage de charge des machines virtuelles dans votre cluster pour <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> déploiements. <abbr title="System Center Virtual Machine Manager">SCVMM</abbr> désactive automatiquement le serveur Windows <abbr title="machine virtuelle">Machine virtuelle</abbr> l’équilibrage de charge lors de l’optimisation dynamique est activée.

## <a name="see-also"></a>Voir aussi
* [Vue d’ensemble de Machine virtuelle charge équilibrage](vm-load-balancing-overview.md)
* [Clustering de basculement](failover-clustering-overview.md)
* [Vue d’ensemble d’Hyper-V](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
