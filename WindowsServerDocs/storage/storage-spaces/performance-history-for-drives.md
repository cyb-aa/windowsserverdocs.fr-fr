---
title: Historique des performances de disques
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879190"
---
# <a name="performance-history-for-drives"></a>Historique des performances de disques

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collecté pour les lecteurs. Historique des performances est disponible pour chaque lecteur dans le sous-système de stockage de cluster, indépendamment de bus ou type de média. Toutefois, il n’est pas disponible pour les disques de démarrage du système d’exploitation.

   > [!NOTE]
   > Historique des performances ne peuvent pas être collectées pour les lecteurs dans un serveur est arrêté. Collection reprendra automatiquement lorsque le serveur redevient opérationnel.

## <a name="series-names-and-units"></a>Unités et les noms de série

Ces séries sont collectées pour chaque lecteur éligible :

| série                          | Unit             |
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
| `physicaldisk.size.total`       | bytes            |
| `physicaldisk.size.used`        | bytes            |

## <a name="how-to-interpret"></a>Comment interpréter

| série                          | Comment interpréter                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Nombre d’opérations de lecture par seconde effectuées par le lecteur.                |
| `physicaldisk.iops.write`       | Nombre d’opérations d’écriture par seconde effectuées par le lecteur.               |
| `physicaldisk.iops.total`       | Nombre total de lire ou écrire des opérations par seconde effectuées par le lecteur. |
| `physicaldisk.throughput.read`  | Quantité de données lues à partir du disque par seconde.                            |
| `physicaldisk.throughput.write` | Quantité de données écrites sur le disque par seconde.                           |
| `physicaldisk.throughput.total` | Quantité totale de données lues ou écrites sur le disque par seconde.        |
| `physicaldisk.latency.read`     | Latence moyenne des opérations de lecture à partir du lecteur.                          |
| `physicaldisk.latency.write`    | Latence moyenne des opérations d’écriture sur le lecteur.                           |
| `physicaldisk.latency.average`  | Latence moyenne de toutes les opérations vers ou depuis le lecteur.                     |
| `physicaldisk.size.total`       | La capacité de stockage totale du lecteur.                                    |
| `physicaldisk.size.used`        | La capacité de stockage utilisé du lecteur.                                     |

## <a name="where-they-come-from"></a>S’ils proviennent d'

Le `iops.*`, `throughput.*`, et `latency.*` série est collectée à partir de la `Physical Disk` compteur de performance définies sur le serveur où le lecteur est connecté, une seule instance par le lecteur. Ces compteurs sont mesurées par `partmgr.sys` et n’incluent pas la quantité de la pile de logiciels Windows, ainsi que les sauts de réseau. Ils sont représentatives des performances du périphérique matériel.

| série                          | Compteur de la source           |
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
   > Les compteurs sont mesurées sur tout l’intervalle, ont ne pas été échantillonnée. Par exemple, si le lecteur est inactif pendant 9 secondes, mais se termine 30 IOs dans la deuxième de 10, sa `physicaldisk.iops.total` seront enregistrées comme 3 e/s par seconde en moyenne pendant cet intervalle de 10 secondes. Cela garantit son historique des performances capture toutes les activités et sont robuste au bruit.

Le `size.*` série est collectée à partir de la `MSFT_PhysicalDisk` classe dans WMI, une seule instance par le lecteur.

| série                          | Propriété Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) applet de commande :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
