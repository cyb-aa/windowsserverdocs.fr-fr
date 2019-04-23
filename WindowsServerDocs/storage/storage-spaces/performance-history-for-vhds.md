---
title: Historique des performances de disques durs virtuels
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: 7a0d8d132b6a5ff42cbe78a22c67dd9fec397184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880400"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Historique des performances de disques durs virtuels

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collecté pour les fichiers de disque dur virtuel (VHD). Historique des performances sont disponible pour chaque disque dur virtuel attaché à une machine virtuelle en cours d’exécution, en cluster. Historique des performances sont disponible pour les formats VHD et VHDX, il n’est pas disponible pour les fichiers VHDX partagés.

   > [!NOTE]
   > Il peut prendre plusieurs minutes avant que la collection commencer pour les fichiers de disque dur virtuel qui vient d’être créés ou déplacés.

## <a name="series-names-and-units"></a>Unités et les noms de série

Ces séries sont collectées pour chaque disque dur virtuel éligible :

| série                    | Unit             |
|---------------------------|------------------|
| `vhd.iops.read`           | par seconde       |
| `vhd.iops.write`          | par seconde       |
| `vhd.iops.total`          | par seconde       |
| `vhd.throughput.read`     | octets par seconde |
| `vhd.throughput.write`    | octets par seconde |
| `vhd.throughput.total`    | octets par seconde |
| `vhd.latency.average`     | secondes          |
| `vhd.size.current`        | bytes            |
| `vhd.size.maximum`        | bytes            |

## <a name="how-to-interpret"></a>Comment interpréter

| série                    | Comment interpréter                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Nombre d’opérations de lecture par seconde effectuées par le disque dur virtuel.                                         |
| `vhd.iops.write`          | Nombre d’opérations d’écriture par seconde effectuées par le disque dur virtuel.                                        |
| `vhd.iops.total`          | Nombre total de lire ou écrire des opérations par seconde effectuées par le disque dur virtuel.                          |
| `vhd.throughput.read`     | Quantité de données lues à partir du disque dur virtuel par seconde.                                                     |
| `vhd.throughput.write`    | Quantité de données écrites sur le disque dur virtuel par seconde.                                                    |
| `vhd.throughput.total`    | Quantité totale de données lues ou écrites sur le disque dur virtuel par seconde.                                 |
| `vhd.latency.average`     | Latence moyenne de toutes les opérations vers ou depuis le disque dur virtuel.                                              |
| `vhd.size.current`        | Sa taille actuelle du disque dur virtuel, si la taille dynamique. Si fixe, la série n’est pas collectée. |
| `vhd.size.maximum`        | La taille maximale du disque dur virtuel, si la taille dynamique. Si fixe, la taille.                  |

## <a name="where-they-come-from"></a>S’ils proviennent d'

Le `iops.*`, `throughput.*`, et `latency.*` série est collectée à partir de la `Hyper-V Virtual Storage Device` ensemble de compteurs de performances sur le serveur où la machine virtuelle est en cours d’exécution, une seule instance par disque dur virtuel ou VHDX.

| série                    | Compteur de la source         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *somme des éléments ci-dessus*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *somme des éléments ci-dessus*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Les compteurs sont mesurées sur tout l’intervalle, ont ne pas été échantillonnée. Par exemple, si le disque dur virtuel est inactif pour 9 secondes, mais se termine 30 IOs dans la deuxième de 10, sa `vhd.iops.total` seront enregistrées comme 3 e/s par seconde en moyenne pendant cet intervalle de 10 secondes. Cela garantit son historique des performances capture toutes les activités et sont robuste au bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) applet de commande :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Pour obtenir le chemin d’accès de chaque disque dur virtuel à partir de la machine virtuelle :

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > L’applet de commande Get-VHD nécessite un chemin d’accès de fichier doit être fourni. Il ne gère pas l’énumération.

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
