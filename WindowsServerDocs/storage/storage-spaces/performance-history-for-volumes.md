---
title: Historique des performances des volumes
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: fea1d3d67ab96d95b1699e8ac0129dba698477fe
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589702"
---
# <a name="performance-history-for-volumes"></a>Historique des performances des volumes

> S’applique à: Windows Server initiés Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique de performances collecté pour les volumes. Historique des performances est disponible pour chaque Cluster partagés Volume CSV du cluster. Toutefois, il n’est pas disponible pour le système d’exploitation de démarrage volumes ni tout autre stockage non-CSV.

   > [!NOTE]
   > Elle peut prendre plusieurs minutes pour la collection commencer à des volumes nouvellement créés ou renommés.

## <a name="series-names-and-units"></a>Unités et noms de série

Ces séries sont collectées pour chaque volume éligible:

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
| `volume.size.total`       |  octets            |
| `volume.size.available`   |  octets            |

## <a name="how-to-interpret"></a>Comment interpréter

| Série                    | Comment interpréter                                                              |
|---------------------------|-------------------------------------------------------------------------------|
| `volume.iops.read`        | Nombre d’opérations de lecture par seconde effectuées par ce volume.                |
| `volume.iops.write`       | Nombre d’opérations d’écriture par seconde effectuées par ce volume.               |
| `volume.iops.total`       | Nombre total de lire ou écrire des opérations par seconde effectuées par ce volume. |
| `volume.throughput.read`  | Quantité de données lues à partir de ce volume par seconde.                            |
| `volume.throughput.write` | Quantité de données écrites à ce volume par seconde.                           |
| `volume.throughput.total` | Quantité totale de données lu ou écrit à ce volume par seconde.        |
| `volume.latency.read`     | Latence moyenne des opérations de lecture à partir de ce volume.                          |
| `volume.latency.write`    | Latence moyenne des opérations d’écriture sur ce volume.                           |
| `volume.latency.average`  | Latence moyenne de toutes les opérations vers ou à partir de ce volume.                     |
| `volume.size.total`       | La capacité totale de stockage du volume.                                     |
| `volume.size.available`   | La capacité de stockage disponible du volume.                                 |

## <a name="where-they-come-from"></a>D'où ils proviennent

Le `iops.*`, `throughput.*`, et `latency.*` série est collectées à partir de la `Cluster CSVFS` ensemble de compteurs de performances. Chaque serveur du cluster possède une instance de chaque volume CSV, indépendamment de la propriété. L’historique des performances enregistrés pour volume `MyVolume` est l’agrégation de la `MyVolume` instances sur chaque serveur du cluster.

| Série                    | Compteur source         |
|---------------------------|------------------------|
| `volume.iops.read`        | `Reads/sec`            |
| `volume.iops.write`       | `Writes/sec`           |
| `volume.iops.total`       | *somme des ci-dessus*     |
| `volume.throughput.read`  | `Read bytes/sec`       |
| `volume.throughput.write` | `Write bytes/sec`      |
| `volume.throughput.total` | *somme des ci-dessus*     |
| `volume.latency.read`     | `Avg. sec/Read`        |
| `volume.latency.write`    | `Avg. sec/Write`       |
| `volume.latency.average`  | *moyenne de la liste ci-dessus* |

   > [!NOTE]
   > Compteurs sont mesurés sur tout l’intervalle, ne pas échantillonnée. Par exemple, si le volume est inactif pendant 9 secondes mais complète 30 IOs dans la deuxième 10, son `volume.iops.total` sera enregistrée en tant que 3 e/s par seconde en moyenne pendant cette période de 10 secondes. Cela garantit son historique de performances capture toutes les activités et est robuste au bruit.

   > [!TIP]
   > Voici les compteurs de même utilisés par l’infrastructure de banc d’essai de [Machine virtuelle flotte](https://github.com/Microsoft/diskspd/blob/master/Frameworks/VMFleet/watch-cluster.ps1) populaires.

Le `size.*` série est collectées à partir de la `MSFT_Volume` classe dans WMI, une seule instance par volume.

| Série                    | Propriété Source |
|---------------------------|-----------------|
| `volume.size.total`       | `Size`          |
| `volume.size.available`   | `SizeRemaining` |

## <a name="usage-in-powershell"></a>Utilisation de PowerShell

Utilisez l’applet de commande [Get-Volume](https://docs.microsoft.com/powershell/module/storage/get-volume) :

```PowerShell
Get-Volume -FriendlyName <FriendlyName> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances de stockage espaces Direct](performance-history.md)
