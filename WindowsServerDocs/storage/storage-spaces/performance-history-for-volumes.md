---
title: Historique des performances pour les volumes
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5f6acf062d2dba7c2a1a04d8a3f7cb4d7bd51a4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856132"
---
# <a name="performance-history-for-volumes"></a>Historique des performances pour les volumes

> S’applique à : Windows Server 2019

Cette sous-rubrique de l' [historique des performances pour espaces de stockage direct](performance-history.md) décrit en détail l’historique des performances collecté pour les volumes. L’historique des performances est disponible pour chaque Volume partagé de cluster (CSV) du cluster. Toutefois, il n’est pas disponible pour les volumes de démarrage du système d’exploitation ni pour tout autre stockage non-CSV.

   > [!NOTE]
   > Le démarrage de la collecte peut prendre plusieurs minutes pour les volumes nouvellement créés ou renommés.

## <a name="series-names-and-units"></a>Noms et unités des séries

Ces séries sont collectées pour chaque volume éligible :

| Série                    | Unit             |
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
| `volume.size.total`       | octets            |
| `volume.size.available`   | octets            |

## <a name="how-to-interpret"></a>Comment interpréter

| Série                    | Comment interpréter                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Nombre d’opérations de lecture par seconde effectuées par ce volume.                |
| `volume.iops.write`       | Nombre d’opérations d’écriture par seconde effectuées par ce volume.               |
| `volume.iops.total`       | Nombre total d’opérations de lecture ou d’écriture par seconde effectuées par ce volume. |
| `volume.throughput.read`  | Quantité de données lues à partir de ce volume par seconde.                            |
| `volume.throughput.write` | Quantité de données écrites sur ce volume par seconde.                           |
| `volume.throughput.total` | Quantité totale de données lues ou écrites sur ce volume par seconde.        |
| `volume.latency.read`     | Latence moyenne des opérations de lecture à partir de ce volume.                          |
| `volume.latency.write`    | Latence moyenne des opérations d’écriture sur ce volume.                           |
| `volume.latency.average`  | Latence moyenne de toutes les opérations vers ou à partir de ce volume.                     |
| `volume.size.total`       | Capacité de stockage totale du volume.                                     |
| `volume.size.available`   | Capacité de stockage disponible du volume.                                 |

## <a name="where-they-come-from"></a>Origine de leur provenance

Les séries `iops.*`, `throughput.*`et `latency.*` sont collectées à partir de l’ensemble de compteurs de performance `Cluster CSVFS`. Chaque serveur du cluster possède une instance pour chaque volume partagé de cluster, quelle que soit la propriété. L’historique des performances enregistré pour le volume `MyVolume` est l’agrégat des instances de `MyVolume` sur chaque serveur du cluster.

| Série                    | Compteur source         |
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
   > Les compteurs sont mesurés sur l’intégralité de l’intervalle, et non échantillonnés. Par exemple, si le volume est inactif pendant 9 secondes, mais qu’il termine 30 IOs dans le dixième seconde, son `volume.iops.total` sera enregistré comme 3 IOs par seconde en moyenne au cours de cet intervalle de 10 secondes. Cela garantit que son historique des performances capture toutes les activités et est robuste pour le bruit.

   > [!TIP]
   > Il s’agit des mêmes compteurs que ceux utilisés par l’infrastructure de test de la [flotte de machines virtuelles](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) populaire.

Les séries `size.*` sont collectées à partir de la classe `MSFT_Volume` dans WMI, une instance par volume.

| Série                    | Source (propriété) |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet de commande [obtient-volume](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
