---
title: Historique des performances pour les volumes
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882950"
---
# <a name="performance-history-for-volumes"></a>Historique des performances pour les volumes

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collecté pour les volumes. Historique des performances est disponible pour chaque Volume CSV (Cluster Shared) dans le cluster. Toutefois, il n’est pas disponible pour le système d’exploitation de démarrage volumes ni tout autre stockage non-CSV.

   > [!NOTE]
   > Il peut prendre plusieurs minutes avant que la collection commencer pour les volumes nouvellement créées ou renommées.

## <a name="series-names-and-units"></a>Unités et les noms de série

Ces séries sont collectées pour chaque volume éligible :

| série                    | Unit             |
|---------------------------|------------------|
| `volume.iops.read`        | par seconde       |
| `volume.iops.write`       | par seconde       |
| `volume.iops.total`       | par seconde       |
| `volume.throughput.read`  | octets par seconde |
| `volume.throughput.write` | octets par seconde |
| `volume.throughput.total` | octets par seconde |
| `volume.latency.read`     | secondes          |
| `volume.latency.write`    | secondes          |
| `volume.latency.average`  | secondes          |
| `volume.size.total`       | bytes            |
| `volume.size.available`   | bytes            |

## <a name="how-to-interpret"></a>Comment interpréter

| série                    | Comment interpréter                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Nombre d’opérations de lecture par seconde effectuées par ce volume.                |
| `volume.iops.write`       | Nombre d’opérations d’écriture par seconde effectuées par ce volume.               |
| `volume.iops.total`       | Nombre total de lire ou écrire des opérations par seconde effectuées par ce volume. |
| `volume.throughput.read`  | Quantité de données lues à partir de ce volume par seconde.                            |
| `volume.throughput.write` | Quantité de données écrites dans ce volume par seconde.                           |
| `volume.throughput.total` | Quantité totale de données lues ou écrites sur ce volume par seconde.        |
| `volume.latency.read`     | Latence moyenne des opérations de lecture à partir de ce volume.                          |
| `volume.latency.write`    | Latence moyenne des opérations d’écriture sur ce volume.                           |
| `volume.latency.average`  | Latence moyenne de toutes les opérations vers ou à partir de ce volume.                     |
| `volume.size.total`       | La capacité de stockage totale du volume.                                     |
| `volume.size.available`   | La capacité de stockage disponible du volume.                                 |

## <a name="where-they-come-from"></a>S’ils proviennent d'

Le `iops.*`, `throughput.*`, et `latency.*` série est collectée à partir de la `Cluster CSVFS` ensemble de compteurs de performances. Chaque serveur dans le cluster a une instance pour chaque volume partagé de cluster, quelle que soit la propriété. L’historique des performances enregistrement pour le volume `MyVolume` l’agrégat de la `MyVolume` instances sur chaque serveur dans le cluster.

| série                    | Compteur de la source         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *somme des éléments ci-dessus*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *somme des éléments ci-dessus*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *moyenne des éléments ci-dessus* |

   > [!NOTE]
   > Les compteurs sont mesurées sur tout l’intervalle, ont ne pas été échantillonnée. Par exemple, si le volume est inactif pendant 9 secondes, mais se termine 30 IOs dans la deuxième de 10, sa `volume.iops.total` seront enregistrées comme 3 e/s par seconde en moyenne pendant cet intervalle de 10 secondes. Cela garantit son historique des performances capture toutes les activités et sont robuste au bruit.

   > [!TIP]
   > Voici les compteurs utilisés par ce fameux [flotte de machine virtuelle](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) framework de test d’évaluation.

Le `size.*` série est collectée à partir de la `MSFT_Volume` classe dans WMI, une seule instance par volume.

| série                    | Propriété Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) applet de commande :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
