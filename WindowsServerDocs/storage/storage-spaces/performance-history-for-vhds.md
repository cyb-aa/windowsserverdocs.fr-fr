---
title: Historique des performances de disques durs virtuels
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1589719"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Historique des performances de disques durs virtuels

> S’applique à: Windows Server initiés Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique de performances collecté pour les fichiers de disque dur virtuel (VHD). Historique des performances est disponible pour chaque disque dur virtuel attaché à un ordinateur virtuel en cours d’exécution, en cluster. Historique des performances est disponible pour les formats de disque dur virtuel et de VHDX, mais il n’est pas disponible pour les fichiers VHDX Shared.

   > [!NOTE]
   > Elle peut prendre plusieurs minutes pour la collection de début pour les fichiers de disque dur virtuel nouvellement créées ou déplacées.

## <a name="series-names-and-units"></a>Unités et noms de série

Ces séries sont collectées pour chaque disque dur virtuel éligible:

| Série                    | Unit             |
|---------------------------|------------------|
| `vhd.iops.read`           | par seconde       |
| `vhd.iops.write`          | par seconde       |
| `vhd.iops.total`          | par seconde       |
| `vhd.throughput.read`     | octets par seconde |
| `vhd.throughput.write`    | octets par seconde |
| `vhd.throughput.total`    | octets par seconde |
| `vhd.latency.average`     | secondes          |
| `vhd.size.current`        |  octets            |
| `vhd.size.maximum`        |  octets            |

## <a name="how-to-interpret"></a>Comment interpréter

| Série                    | Comment interpréter                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Nombre d’opérations de lecture par seconde effectuées par le disque dur virtuel.                                         |
| `vhd.iops.write`          | Nombre d’opérations d’écriture par seconde effectuées par le disque dur virtuel.                                        |
| `vhd.iops.total`          | Nombre total de lire ou écrire des opérations par seconde effectuées par le disque dur virtuel.                          |
| `vhd.throughput.read`     | Quantité de données lues à partir du disque dur virtuel par seconde.                                                     |
| `vhd.throughput.write`    | Quantité de données écrites sur le disque dur virtuel par seconde.                                                    |
| `vhd.throughput.total`    | Quantité totale de données lu ou écrit sur le disque dur virtuel par seconde.                                 |
| `vhd.latency.average`     | Latence moyenne de toutes les opérations vers ou à partir du disque dur virtuel.                                              |
| `vhd.size.current`        | La taille du fichier de disque dur virtuel, si l’extension dynamique. Si le cas, la série n’est pas collectée. |
| `vhd.size.maximum`        | La taille maximale du disque dur virtuel, si l’extension dynamique. Si fixe, la taille.                  |

## <a name="where-they-come-from"></a>D'où ils proviennent

Le `iops.*`, `throughput.*`, et `latency.*` série est collectées à partir de la `Hyper-V Virtual Storage Device` compteur de performance est définie sur le serveur où l’ordinateur virtuel est en cours d’exécution, une seule instance par disque dur virtuel ou VHDX.

| Série                    | Compteur source         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *somme des ci-dessus*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *somme des ci-dessus*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Compteurs sont mesurés sur tout l’intervalle, ne pas échantillonnée. Par exemple, si le disque dur virtuel est inactif pour 9 secondes mais complète 30 IOs dans la deuxième 10, son `vhd.iops.total` sera enregistrée en tant que 3 e/s par seconde en moyenne pendant cette période de 10 secondes. Cela garantit son historique de performances capture toutes les activités et est robuste au bruit.

## <a name="usage-in-powershell"></a>Utilisation de PowerShell

Utilisez l’applet de commande [Get-disque dur virtuel](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Pour obtenir le chemin d’accès de chaque disque dur virtuel sur l’ordinateur virtuel:

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > L’applet de commande Get-VHD nécessite un chemin d’accès à fournir. Il ne prend pas en charge énumération.

## <a name="see-also"></a>Voir aussi

- [Historique des performances de stockage espaces Direct](performance-history.md)
