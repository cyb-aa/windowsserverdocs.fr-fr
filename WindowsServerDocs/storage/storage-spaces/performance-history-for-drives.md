---
title: Historique des performances pour les lecteurs
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: a6c6065b8d7963ada5d80844b270fe088eaa6e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859452"
---
# <a name="performance-history-for-drives"></a>Historique des performances pour les lecteurs

> S’applique à : Windows Server 2019

Cette sous-rubrique de l' [historique des performances pour espaces de stockage direct](performance-history.md) décrit en détail l’historique des performances collecté pour les lecteurs. L’historique des performances est disponible pour chaque lecteur du sous-système de stockage en cluster, quel que soit le type de bus ou de média. Toutefois, il n’est pas disponible pour les lecteurs de démarrage du système d’exploitation.

   > [!NOTE]
   > L’historique des performances ne peut pas être collecté pour les lecteurs d’un serveur défaillant. La collecte reprendra automatiquement lorsque le serveur sera à nouveau disponible.

## <a name="series-names-and-units"></a>Noms et unités des séries

Ces séries sont collectées pour chaque lecteur éligible :

| Série                          | Unit             |
|---------------------------------|------------------|
| `physicaldisk.iops.read`        | par seconde       |
| `physicaldisk.iops.write`       | par seconde       |
| `physicaldisk.iops.total`       | par seconde       |
| `physicaldisk.throughput.read`  | octets par seconde |
| `physicaldisk.throughput.write` | octets par seconde |
| `physicaldisk.throughput.total` | octets par seconde |
| `physicaldisk.latency.read`     | secondes          |
| `physicaldisk.latency.write`    | secondes          |
| `physicaldisk.latency.average`  | secondes          |
| `physicaldisk.size.total`       | octets            |
| `physicaldisk.size.used`        | octets            |

## <a name="how-to-interpret"></a>Comment interpréter

| Série                          | Comment interpréter                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Nombre d’opérations de lecture par seconde effectuées par le lecteur.                |
| `physicaldisk.iops.write`       | Nombre d’opérations d’écriture par seconde effectuées par le lecteur.               |
| `physicaldisk.iops.total`       | Nombre total d’opérations de lecture ou d’écriture par seconde effectuées par le lecteur. |
| `physicaldisk.throughput.read`  | Quantité de données lues à partir du lecteur par seconde.                            |
| `physicaldisk.throughput.write` | Quantité de données écrites sur le disque par seconde.                           |
| `physicaldisk.throughput.total` | Quantité totale de données lues ou écrites sur le lecteur par seconde.        |
| `physicaldisk.latency.read`     | Latence moyenne des opérations de lecture à partir du lecteur.                          |
| `physicaldisk.latency.write`    | Latence moyenne des opérations d’écriture sur le lecteur.                           |
| `physicaldisk.latency.average`  | Latence moyenne de toutes les opérations vers ou à partir du lecteur.                     |
| `physicaldisk.size.total`       | Capacité de stockage totale du lecteur.                                    |
| `physicaldisk.size.used`        | Capacité de stockage utilisée du lecteur.                                     |

## <a name="where-they-come-from"></a>Origine de leur provenance

Les séries `iops.*`, `throughput.*`et `latency.*` sont collectées à partir de l’ensemble de compteurs de performance `Physical Disk` sur le serveur sur lequel le lecteur est connecté, une instance par lecteur. Ces compteurs sont mesurés par `partmgr.sys` et n’incluent pas la plus grande partie de la pile logicielle Windows ni aucun saut réseau. Elles sont représentatives des performances matérielles des appareils.

| Série                          | Compteur source           |
|---------------------------------|--------------------------|
| `physicaldisk.iops.read`        | `Disk Reads/sec`         |
| `physicaldisk.iops.write`       | `Disk Writes/sec`        |
| `physicaldisk.iops.total`       | `Disk Transfers/sec`     |
| `physicaldisk.throughput.read`  | `Disk Read Bytes/sec`    |
| `physicaldisk.throughput.write` | `Disk Write Bytes/sec`   |
| `physicaldisk.throughput.total` | `Disk Bytes/sec`         |
| `physicaldisk.latency.read`     | `Avg. Disk sec/Read`     |
| `physicaldisk.latency.write`    | `Avg. Disk sec/Writes`   |
| `physicaldisk.latency.average`  | `Avg. Disk sec/Transfer` |

   > [!NOTE]
   > Les compteurs sont mesurés sur l’intégralité de l’intervalle, et non échantillonnés. Par exemple, si le lecteur est inactif pendant 9 secondes, mais qu’il termine 30 IOs dans le dixième seconde, son `physicaldisk.iops.total` sera enregistré comme 3 IOs par seconde en moyenne au cours de cet intervalle de 10 secondes. Cela garantit que son historique des performances capture toutes les activités et est robuste pour le bruit.

Les séries `size.*` sont collectées à partir de la classe `MSFT_PhysicalDisk` dans WMI, une instance par lecteur.

| Série                          | Source (propriété)        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet de commande [obten-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
