---
title: Affinité de cluster
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 03/07/2019
description: Cet article décrit les niveaux d’affinité et antiAffinity de cluster de basculement
ms.openlocfilehash: 67929e6d3399633ebfec0b908463131973aecaf7
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453030"
---
# <a name="cluster-affinity"></a>Affinité de cluster

> S’applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement peut contenir de nombreux rôles qui peuvent déplacer entre les nœuds et à exécuter.  Il est parfois quand les certains rôles (par exemple, les machines virtuelles, groupes de ressources, etc.) ne doivent pas s’exécuter sur le même nœud.  Cela peut être dû à la consommation des ressources, utilisation de la mémoire, etc.  Par exemple, il existe deux machines virtuelles qui sont beaucoup de ressources processeur et mémoire et si les deux machines virtuelles sont en cours d’exécution sur le même nœud, moins le des machines virtuelles peut avoir des problèmes d’impact sur les performances.  Cet article explique les niveaux antiaffinity et comment vous pouvez les utiliser de cluster.

## <a name="what-is-affinity-and-antiaffinity"></a>Qu’est l’affinité et AntiAffinity ?

L’affinité est une règle que vous établissez qui établit une relation entre deux ou plusieurs rôles (i, e, machines virtuelles, groupes de ressources, etc.) afin de les regrouper.  AntiAffinity est identique, mais permet de limiter les rôles spécifiés indépendamment les uns des autres.  Clusters de basculement utiliser AntiAffinity pour ses rôles.  Plus précisément, le [AntiAffinityClassNames](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/groups-antiaffinityclassnames) paramètre défini sur les rôles, afin qu’ils ne s’exécutent pas sur le même nœud.  

## <a name="antiaffinityclassnames"></a>AntiAffinityClassnames

Lorsque vous examinez les propriétés d’un groupe, il est le paramètre AntiAffinityClassNames et elle est vide par défaut.  Dans les exemples ci-dessous, le Groupe1 et le groupe2 doivent être séparés de s’exécuter sur le même nœud.  Pour afficher la propriété, la commande PowerShell et le résultat serait :

    PS> Get-ClusterGroup Group1 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

    PS> Get-ClusterGroup Group2 | fl AntiAffinityClassNames
    AntiAffinityClassNames : {}

Dans la mesure où AntiAffinityClassNames ne sont pas définis par défaut, ces peuvent rôles ensemble exécution ou les uns des autres.  L’objectif est de les conserver pour être séparés.  La valeur AntiAffinityClassNames peut être tout ce que vous voulez, il suffit d’être identiques.  Supposons que le Groupe1 et le groupe2 sont des contrôleurs de domaine en cours d’exécution sur des machines virtuelles, et il seraient mieux servis en cours d’exécution sur des nœuds différents.  Dans la mesure où il s’agit de contrôleurs de domaine, je vais utiliser le contrôleur de domaine pour le nom de classe.  Pour définir la valeur, la commande PowerShell et les résultats serait :

    PS> $AntiAffinity = New-Object System.Collections.Specialized.StringCollection
    PS> $AntiAffinity.Add("DC")
    PS> (Get-ClusterGroup -Name "Group1").AntiAffinityClassNames = $AntiAffinity
    PS> (Get-ClusterGroup -Name "Group2").AntiAffinityClassNames = $AntiAffinity

    PS> Get-ClusterGroup "Group1" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

    PS> Get-ClusterGroup "Group2" | fl AntiAffinityClassNames
    AntiAffinityClassNames : {DC}

Maintenant qu’elles sont définies, le clustering de basculement tente de conserver les uns des autres.  

Le paramètre AntiAffinityClassName est un bloc « soft ».  Autrement dit, il tente de conserver les uns des autres, mais s’il ne peut pas, elle permettra toujours s’exécuter sur le même nœud.  Par exemple, les groupes sont en cours d’exécution sur un cluster de basculement à deux nœuds.  Si un nœud doit aller vers le bas pour la maintenance, il signifie que les deux groupes est en cours d’exécution sur le même nœud.  Dans ce cas, il serait OK pour le faire.  Il ne peut pas être plus idéale, mais les deux machines virtial seront toujours en cours d’exécution dans les plages de performances acceptables.

## <a name="i-need-more"></a>J’ai besoin de plus

Comme mentionné, AntiAffinityClassNames est un bloc de manière réversible.  Mais que se passe-t-il si un blocage critique est nécessaire ?  Les machines virtuelles ne peut pas être exécutés sur le même nœud ; Sinon, impact sur les performances se produira et certains services à tourner vers le bas.

Dans ces cas, il existe une propriété de cluster supplémentaire de ClusterEnforcedAntiAffinity.  Ce niveau antiaffinity empêchera à tout prix les valeurs AntiAffinityClassNames mêmes en cours d’exécution sur le même nœud.

Pour afficher la propriété et la valeur, la commande PowerShell (et le résultat) serait :

    PS> Get-Cluster | fl ClusterEnforcedAntiAffinity
    ClusterEnforcedAntiAffinity : 0

La valeur « 0 » signifie qu’elle est désactivée et non à être appliquées.  La valeur « 1 » lui permet d’et est le bloc de disque dur.  Pour activer ce bloc de disque dur, la commande (et le résultat) sont :

    PS> (Get-Cluster).ClusterEnforcedAntiAffinity = 1
    ClusterEnforcedAntiAffinity : 1

Lorsque ces deux éléments sont définies, le groupe ne pourra assemblent en ligne.  S’ils sont sur le même nœud, voici ce que vous verriez dans le Gestionnaire de Cluster de basculement.

![Affinité de cluster](media/Cluster-Affinity/Cluster-Affinity-1.png)

Dans une liste de PowerShell, des groupes, vous verriez cela :

    PS> Get-ClusterGroup

    Name       State
    ----       -----
    Group1     Offline(Anti-Affinity Conflict)
    Group2     Online

## <a name="additional-comments"></a>Commentaires supplémentaires

- Vérifiez que vous utilisez le paramètre AntiAffinity approprié en fonction des besoins.
- N’oubliez pas que dans un scénario de deux nœuds et ClusterEnforcedAntiAffinity, si un nœud est arrêté, les deux groupes ne sera ne pas exécuté.  

- L’utilisation de propriétaires favoris sur les groupes peut être combinée avec AntiAffinity dans un cluster à trois ou plusieurs nœuds.
- Les paramètres AntiAffinityClassNames et ClusterEnforcedAntiAffinity sont lieu uniquement après un recyclage des ressources. I.E. Vous pouvez les définir, mais si les deux groupes sont en ligne sur le même nœud si la valeur, ils continueront tous les deux de rester en ligne.



