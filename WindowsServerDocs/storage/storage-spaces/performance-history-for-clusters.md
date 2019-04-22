---
title: Historique des performances pour les clusters
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818770"
---
# <a name="performance-history-for-clusters"></a>Historique des performances pour les clusters

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit l’historique des performances collecté pour les clusters.

Il n’existe aucune série lancées au niveau du cluster. Au lieu de cela, série de serveur, tel que `clusternode.cpu.usage`, sont agrégées pour tous les serveurs du cluster. Série de volume, tel que `volume.iops.total`, sont agrégées pour tous les volumes dans le cluster. Et lecteur série, tel que `physicaldisk.size.total`, sont agrégées pour tous les disques du cluster.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) applet de commande :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
