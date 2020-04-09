---
title: Historique des performances pour les clusters
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7a5eec986d6e7d633f1917c599ab6fcd244c7008
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856202"
---
# <a name="performance-history-for-clusters"></a>Historique des performances pour les clusters

> S’applique à : Windows Server 2019

Cette sous-rubrique de [l’historique des performances pour espaces de stockage direct](performance-history.md) décrit l’historique des performances collecté pour les clusters.

Aucune série n’a été lancée au niveau du cluster. Au lieu de cela, les séries serveur, telles que les `clusternode.cpu.usage`, sont agrégées pour tous les serveurs du cluster. Les séries de volumes, telles que `volume.iops.total`, sont agrégées pour tous les volumes du cluster. Et les séries de lecteurs, telles que les `physicaldisk.size.total`, sont agrégées pour tous les lecteurs du cluster.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet de commande [obten-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/get-cluster) :

```PowerShell
Get-Cluster | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
