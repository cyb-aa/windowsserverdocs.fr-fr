---
title: Historique des performances de disques
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: d162275a885dac79e7efe749328ebdca471fcad1
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589753"
---
# <a name="performance-history-for-drives"></a>Historique des performances de disques

> S’applique à: Windows Server initiés Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique de performances collecté pour les lecteurs. Historique des performances est disponible pour chaque lecteur dans le sous-système de stockage de cluster, quel que soit le bus ou type de média. Toutefois, il n’est pas disponible pour les lecteurs de démarrage du système d’exploitation.

   > [!NOTE]
   > L’historique des performances ne peut pas être collectés pour les lecteurs d’un serveur est arrêté. Collection reprendra automatiquement lorsque le serveur est à nouveau disponible.

## <a name="series-names-and-units"></a>Unités et noms de série

Ces séries sont collectées pour chaque lecteur éligible:

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
| `physicaldisk.size.total`       |  octets            |
| `physicaldisk.size.used`        |  octets            |

## <a name="how-to-interpret"></a>Comment interpréter

| Série                          | Comment interpréter                                                            |
|---------------------------------|-----------------------------------------------------------------------------|
| `physicaldisk.iops.read`        | Nombre d’opérations de lecture par seconde effectuées par le lecteur.                |
| `physicaldisk.iops.write`       | Nombre d’opérations d’écriture par seconde effectuées par le lecteur.               |
| `physicaldisk.iops.total`       | Nombre total de lire ou écrire des opérations par seconde effectuées par le lecteur. |
| `physicaldisk.throughput.read`  | Quantité de données lues à partir du disque par seconde.                            |
| `physicaldisk.throughput.write` | Quantité de données écrites sur le disque par seconde.                           |
| `physicaldisk.throughput.total` | Quantité totale de données, lire ou écrire sur le disque par seconde.        |
| `physicaldisk.latency.read`     | Latence moyenne des opérations de lecture à partir du lecteur.                          |
| `physicaldisk.latency.write`    | Latence moyenne des opérations d’écriture sur le disque.                           |
| `physicaldisk.latency.average`  | Latence moyenne de toutes les opérations vers ou depuis le lecteur.                     |
| `physicaldisk.size.total`       | La capacité totale de stockage du lecteur.                                    |
| `physicaldisk.size.used`        | La capacité de stockage utilisé du lecteur.                                     |

## <a name="where-they-come-from"></a>D'où ils proviennent

Le `iops.*`, `throughput.*`, et `latency.*` série est collectées à partir de la `Physical Disk` compteur de performance définies sur le serveur où le lecteur est connecté, une seule instance par lecteur. Ces compteurs sont mesurés en `partmgr.sys` et ne pas inclure de la pile logicielle Windows, ainsi que les sauts de réseau. Ils sont représentant des performances des périphériques matériels.

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
   > Compteurs sont mesurés sur tout l’intervalle, ne pas échantillonnée. Par exemple, si le lecteur est inactif pendant 9 secondes mais complète 30 IOs dans la deuxième 10, son `physicaldisk.iops.total` sera enregistrée en tant que 3 e/s par seconde en moyenne pendant cette période de 10 secondes. Cela garantit son historique de performances capture toutes les activités et est robuste au bruit.

Le `size.*` série est collectées à partir de la `MSFT_PhysicalDisk` classe dans WMI, une seule instance par lecteur.

| Série                          | Propriété Source        |
|---------------------------------|------------------------|
| `physicaldisk.size.total`       | `Size`                 |
| `physicaldisk.size.used`        | `VirtualDiskFootprint` |

## <a name="usage-in-powershell"></a>Utilisation de PowerShell

Utilisez l’applet de commande [Get-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/get-physicaldisk) :

```PowerShell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances de stockage espaces Direct](performance-history.md)
