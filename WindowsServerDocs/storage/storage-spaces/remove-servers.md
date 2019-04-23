---
title: Suppression de serveurs dans les espaces de stockage direct
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
description: Comment supprimer des serveurs d’un cluster d'espaces de stockage direct dans Windows Server.
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: 9fcb67b3c5fbcff0ca2a48ee9a1d2e109af3e9a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890780"
---
# <a name="removing-servers-in-storage-spaces-direct"></a>Suppression de serveurs dans les espaces de stockage direct

>S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique explique comment supprimer des serveurs dans les [espaces de stockage direct](storage-spaces-direct-overview.md) à l’aide de PowerShell.

## <a name="remove-a-server-but-leave-its-drives"></a>Supprimer un serveur en conservant ses lecteurs

Si vous envisagez d'ajouter prochainement le serveur dans le cluster, ou si vous souhaitez conserver ses lecteurs en les déplaçant vers un autre serveur, vous pouvez supprimer le serveur du cluster *sans* supprimer ses lecteurs du pool de stockage. Il s’agit du comportement à adopter par défaut si vous utilisez le Gestionnaire du Cluster de basculement pour supprimer le serveur.

Utilisez l'applet de commande [Remove-ClusterNode](https://technet.microsoft.com/library/hh847251.aspx) dans PowerShell :

```PowerShell
Remove-ClusterNode <Name>
```

L'exécution de cet applet de commande est très rapide, quels que soient les besoins en termes de capacité, car le pool de stockage « se souvient » des lecteurs manquants et attend leur retour. Aucun déplacement de données n'a lieu en dehors des lecteurs manquants. Pendant leur absence, leur **OperationalStatus** est défini sur « Perte de Communication », et vos volumes possèdent le statut « Incomplet ».

Une fois que les lecteurs sont revenus, ils sont automatiquement détectés et réassociés au pool, même s’ils appartiennent désormais à un nouveau serveur.

   >[!WARNING]
   > Ne distribuez pas de lecteurs avec les données du pool depuis un serveur vers plusieurs autres serveurs. Par exemple, si un serveur équipé de dix lecteurs échoue (si sa carte mère ou son lecteur de démarrage a échoué, par exemple), vous **pouvez** déplacer l'ensemble des dix lecteurs vers un nouveau serveur, mais vous **ne pouvez pas** les déplacer séparément vers des serveurs différents.

## <a name="remove-a-server-and-its-drives"></a>Supprimer un serveur et ses lecteurs

Si vous souhaitez supprimer définitivement un serveur du cluster (parfois appelé mise à l’échelle), vous pouvez supprimer le serveur du cluster *et* supprimer ses lecteurs du pool de stockage.

Utilisez l'applet de commande **Remove-ClusterNode** avec l’indicateur facultatif **- CleanUpDisks** :

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

L'exécution de cet applet de commande peut prendre beaucoup de temps (parfois plusieurs heures), car Windows doit déplacer toutes les données stockées sur ce serveur vers d’autres serveurs du cluster. Une fois cette opération terminée, les lecteurs sont définitivement supprimés du pool de stockage, renvoyant ainsi les volumes affectés à un état sain.

### <a name="requirements"></a>Configuration requise

Pour définitivement se mettre à l'échelle (supprimer un serveur *et* ses lecteurs), votre cluster doit respecter deux conditions. Dans le cas contraire, l'applet de commande **CleanUpDisks - Remove-ClusterNode** retourne immédiatement une erreur, avant de commencer à déplacer des données, afin de limiter les perturbations occasionnées.

#### <a name="enough-capacity"></a>Capacité suffisante

Tout d’abord, vous devez disposer de capacité de stockage suffisante dans les autres serveurs pour prendre en charge tous les volumes.

Par exemple, si vous disposez de quatre serveurs, possédant chacun 10 lecteurs d'une capacité d'1 To, vous disposez d'une capacité destockage physique totale de 40 To. Après avoir supprimé un serveur et tous ses lecteurs, il vous restera 30 To de capacité. Si les empreintes de vos volumes représentent plus de 30 To au total, elles ne tiendront pas dans les serveurs restants. L'applet de commande renverra alors une erreur et ne déplacera aucune donnée.

#### <a name="enough-fault-domains"></a>Quantité suffisante de domaines d’erreur

Ensuite, vous devez disposer de suffisamment de domaines d’erreur (généralement des serveurs) pour assurer la résilience de vos volumes.

Par exemple, si vos volumes utilisent la mise en miroir triple au niveau du serveur pour la résilience, ils ne peuvent pas être entièrement sains, sauf si vous possédez au moins trois serveurs. Si vous possédez exactement trois serveurs et que vous essayez d'en supprimer un, ainsi que l'ensemble de ses lecteurs, l’applet de commande renvoie une erreur et ne déplace aucune donnée.

Le tableau suivant indique le nombre minimum de domaines d’erreur requis pour chaque type de résilience.

|    Résilience          |    Nombre de domaines minimum requis par défaut   |
|------------------------|-------------------------------------|
|    Miroir double      |    2                                |
|    Miroir triple    |    3                                |
|    Parité double         |    4                                |

   >[!NOTE]
   > Vous pouvez posséder brièvement un nombre de serveurs réduit (par exemple, lors des pannes ou des maintenances). Toutefois, pour que les volumes reviennent entièrement à un état sain, vous devez disposer du nombre minimal de serveurs indiqué ci-dessus.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
