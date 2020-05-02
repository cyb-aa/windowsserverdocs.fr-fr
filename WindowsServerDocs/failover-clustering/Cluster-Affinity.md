---
title: Affinité de cluster
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 03/07/2019
description: Cet article décrit l’affinité de cluster de basculement et les niveaux d’antiaffinité
ms.openlocfilehash: b0c2209680f3c34ac8376d5662620595aff92c0b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720613"
---
# <a name="cluster-affinity"></a>Affinité de cluster

> S'applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement peut contenir de nombreux rôles pouvant être déplacés entre les nœuds et exécutés. Dans certains cas, certains rôles (par exemple, les machines virtuelles, les groupes de ressources, etc.) ne doivent pas s’exécuter sur le même nœud.  Cela peut être dû à la consommation des ressources, à l’utilisation de la mémoire, etc.  Par exemple, il existe deux machines virtuelles nécessitant beaucoup de mémoire et de processeur, et si les deux machines virtuelles s’exécutent sur le même nœud, l’un des ordinateurs virtuels ou les deux peut avoir des problèmes d’impact sur les performances.  Cet article explique les niveaux d’antiaffinité du cluster et comment vous pouvez les utiliser.

## <a name="what-is-affinity-and-antiaffinity"></a>Qu’est-ce que l’affinité et l’antiaffinité ?

L’affinité est une règle que vous devez configurer pour établir une relation entre deux rôles (i, e, machines virtuelles, groupes de ressources, etc.) afin de les conserver ensemble.  L’antiaffinité est la même, mais elle est utilisée pour essayer et conserver les rôles spécifiés indépendamment l’un de l’autre. Les clusters de basculement utilisent l’antiaffinité pour ses rôles.  Plus précisément, le paramètre [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) défini sur les rôles pour qu’ils ne s’exécutent pas sur le même nœud.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Lorsque vous examinez les propriétés d’un groupe, il y a le paramètre AntiAffinityClassNames et il est vide par défaut.  Dans les exemples ci-dessous, Group1 et Group2 doivent être séparés de l’exécution sur le même nœud.  Pour afficher la propriété, la commande PowerShell et le résultat sont les suivants :

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Étant donné que AntiAffinityClassNames n’est pas défini comme valeur par défaut, ces rôles peuvent être exécutés ensemble ou indépendamment.  L’objectif est de conserver les séparations.  La valeur de AntiAffinityClassNames peut être la même que celle que vous souhaitez, mais elle doit simplement être la même.  Disons que Group1 et Group2 sont des contrôleurs de domaine exécutés sur des machines virtuelles et qu’ils sont les mieux utilisés sur des nœuds différents.  Étant donné qu’il s’agit de contrôleurs de domaine, je vais utiliser DC comme nom de classe.  Pour définir la valeur, la commande PowerShell et les résultats sont les suivants :

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

À présent qu’ils sont définis, le clustering de basculement tente de les conserver en dehors.  

Le paramètre AntiAffinityClassName est un bloc « soft ».  Cela signifie qu’elle tentera de les séparer, mais si elle ne le peut pas, elle pourra toujours s’exécuter sur le même nœud.  Par exemple, les groupes s’exécutent sur un cluster de basculement à deux nœuds.  Si un nœud doit descendre en mode maintenance, cela signifie que les deux groupes sont opérationnels sur le même nœud.  Dans ce cas, il est possible de le faire.  Il n’est peut-être pas le plus idéal, mais les deux machines virtial s’exécutent toujours dans des limites de performances acceptables.

## <a name="i-need-more"></a>J’ai besoin de plus

Comme mentionné, AntiAffinityClassNames est un bloc souple.  Mais que se passe-t-il si un blocage matériel est nécessaire ?  Les machines virtuelles ne peuvent pas être exécutées sur le même nœud ; dans le cas contraire, l’impact sur les performances se produit et les services peuvent éventuellement descendre.

Dans ce cas, il existe une propriété de cluster supplémentaire de ClusterEnforcedAntiAffinity.  Ce niveau d’antiaffinité empêchera tout coût des mêmes valeurs AntiAffinityClassNames de s’exécuter sur le même nœud.

Pour afficher la propriété et la valeur, la commande PowerShell (et le résultat) sont les suivantes :

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

La valeur « 0 » signifie qu’elle est désactivée et qu’elle ne doit pas être appliquée.  La valeur de « 1 » l’active et est le bloc Hard.  Pour activer ce blocage matériel, la commande (et le résultat) est :

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Quand ces deux paramètres sont définis, le groupe ne peut pas être mis en ligne ensemble.  S’ils se trouvent sur le même nœud, c’est ce que vous pouvez voir dans Gestionnaire du cluster de basculement.

![Affinité de cluster](media/Cluster-Affinity/Cluster-Affinity-1.png)

Dans une liste PowerShell des groupes, vous voyez ceci :

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Commentaires supplémentaires

- Vérifiez que vous utilisez le paramètre d’antiaffinité approprié en fonction des besoins.
- N’oubliez pas que dans un scénario à deux nœuds et ClusterEnforcedAntiAffinity, si un nœud est défaillant, les deux groupes ne s’exécuteront pas.  

- L’utilisation de propriétaires préférés sur des groupes peut être combinée à l’antiaffinité dans un cluster à trois nœuds ou plus.
- Les paramètres AntiAffinityClassNames et ClusterEnforcedAntiAffinity se produisent uniquement après un recyclage des ressources. savoir vous pouvez les définir, mais si les deux groupes sont en ligne sur le même nœud lorsqu’ils sont définis, ils continuent à rester en ligne.
