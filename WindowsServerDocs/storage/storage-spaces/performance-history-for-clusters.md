---
title: Historique des performances de clusters
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 68596cbdcf8593cd3017c8ae5d0836891c78229c
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894318"
---
# <a name="performance-history-for-clusters"></a>Historique des performances de clusters

> S’applique à: Windows Server initiés Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit l’historique de performances collecté pour les clusters.

Il n’existe aucune série lancées au niveau du cluster. Au lieu de cela, série du serveur, tel que `clusternode.cpu.usage`, sont regroupées pour tous les serveurs du cluster. Série de volume, tel que `volume.iops.total`, sont regroupées pour tous les volumes du cluster. Et lecteur série, tel que `physicaldisk.size.total`, sont regroupées pour tous les lecteurs du cluster.

## <a name="usage-in-powershell"></a>Utilisation de PowerShell

Utilisez l’applet de commande [Get-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances de stockage espaces Direct](performance-history.md)
