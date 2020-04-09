---
title: Historique des performances pour les disques durs virtuels
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: d917c2d75c1e4078438b94e8aa4a6f921019af5a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856162"
---
# <a name="performance-history-for-virtual-hard-disks"></a>Historique des performances pour les disques durs virtuels

> S’applique à : Windows Server 2019

Cette sous-rubrique de l' [historique des performances pour espaces de stockage direct](performance-history.md) décrit en détail l’historique des performances collecté pour les fichiers de disque dur virtuel (VHD). L’historique des performances est disponible pour tous les disques durs virtuels attachés à un ordinateur virtuel en cours d’exécution. L’historique des performances est disponible pour les formats VHD et VHDX. Toutefois, il n’est pas disponible pour les fichiers VHDX partagés.

   > [!NOTE]
   > Le démarrage de la collecte peut prendre plusieurs minutes pour les fichiers VHD nouvellement créés ou déplacés.

## <a name="series-names-and-units"></a>Noms et unités des séries

Ces séries sont collectées pour chaque disque dur virtuel éligible :

| Série                    | Unit             |
|---------------------------|------------------|
| `vhd.iops.read`           | par seconde       |
| `vhd.iops.write`          | par seconde       |
| `vhd.iops.total`          | par seconde       |
| `vhd.throughput.read`     | octets par seconde |
| `vhd.throughput.write`    | octets par seconde |
| `vhd.throughput.total`    | octets par seconde |
| `vhd.latency.average`     | secondes          |
| `vhd.size.current`        | octets            |
| `vhd.size.maximum`        | octets            |

## <a name="how-to-interpret"></a>Comment interpréter

| Série                    | Comment interpréter                                                                                                 |
|---------------------------|------------------------------------------------------------------------------------------------------------------|
| `vhd.iops.read`           | Nombre d’opérations de lecture par seconde effectuées par le disque dur virtuel.                                         |
| `vhd.iops.write`          | Nombre d’opérations d’écriture par seconde effectuées par le disque dur virtuel.                                        |
| `vhd.iops.total`          | Nombre total d’opérations de lecture ou d’écriture par seconde effectuées par le disque dur virtuel.                          |
| `vhd.throughput.read`     | Quantité de données lues à partir du disque dur virtuel par seconde.                                                     |
| `vhd.throughput.write`    | Quantité de données écrites sur le disque dur virtuel par seconde.                                                    |
| `vhd.throughput.total`    | Quantité totale de données lues ou écrites sur le disque dur virtuel par seconde.                                 |
| `vhd.latency.average`     | Latence moyenne de toutes les opérations vers ou à partir du disque dur virtuel.                                              |
| `vhd.size.current`        | Taille actuelle du fichier de disque dur virtuel, en cas de développement dynamique. S’il est corrigé, la série n’est pas collectée. |
| `vhd.size.maximum`        | Taille maximale du disque dur virtuel, en cas de développement dynamique. S’il est corrigé, est la taille.                  |

## <a name="where-they-come-from"></a>Origine de leur provenance

Les séries `iops.*`, `throughput.*`et `latency.*` sont collectées à partir de l’ensemble de compteurs de performance `Hyper-V Virtual Storage Device` sur le serveur sur lequel l’ordinateur virtuel est en cours d’exécution, une instance par VHD ou VHDX.

| Série                    | Compteur source         |
|---------------------------|------------------------|
| `vhd.iops.read`           | `Read Operations/Sec`  |
| `vhd.iops.write`          | `Write Operations/Sec` |
| `vhd.iops.total`          | *somme des éléments ci-dessus*     |
| `vhd.throughput.read`     | `Read Bytes/sec`       |
| `vhd.throughput.write`    | `Write Bytes/sec`      |
| `vhd.throughput.total`    | *somme des éléments ci-dessus*     |
| `vhd.latency.average`     | `Latency`              |

   > [!NOTE]
   > Les compteurs sont mesurés sur l’intégralité de l’intervalle, et non échantillonnés. Par exemple, si le disque dur virtuel est inactif pendant 9 secondes mais qu’il termine 30 IOs dans le dixième seconde, son `vhd.iops.total` sera enregistré comme 3 IOs par seconde en moyenne au cours de cet intervalle de 10 secondes. Cela garantit que son historique des performances capture toutes les activités et est robuste pour le bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet [de commande obten-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd) :

```PowerShell
Get-VHD <Path> | Get-ClusterPerf
```

Pour récupérer le chemin d’accès de chaque disque dur virtuel à partir de la machine virtuelle :

```PowerShell
(Get-VM <Name>).HardDrives | Select Path
```

   > [!NOTE]
   > L’applet de commande obtenir-VHD requiert la mise à disposition d’un chemin d’accès de fichier. Elle ne prend pas en charge l’énumération.

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
