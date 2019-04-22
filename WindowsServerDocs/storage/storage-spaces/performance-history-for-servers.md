---
title: Historique des performances pour les serveurs
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820590"
---
# <a name="performance-history-for-servers"></a>Historique des performances pour les serveurs

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collecté pour les serveurs. Historique des performances est disponible pour chaque serveur dans le cluster.

   > [!NOTE]
   > Historique des performances ne peuvent pas être collectées pour un serveur qui est arrêté. Collection reprendra automatiquement lorsque le serveur redevient opérationnel.

## <a name="series-names-and-units"></a>Unités et les noms de série

Ces séries sont collectées pour chaque serveur éligibles :

| série                           | Unit    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | % |
| `clusternode.cpu.usage.guest`    | % |
| `clusternode.cpu.usage.host`     | % |
| `clusternode.memory.total`       | bytes   |
| `clusternode.memory.available`   | bytes   |
| `clusternode.memory.usage`       | bytes   |
| `clusternode.memory.usage.guest` | bytes   |
| `clusternode.memory.usage.host`  | bytes   |

En outre, lecteur comme série `physicaldisk.size.total` sont agrégées pour tous les lecteurs concernés attachés au serveur et la série de carte réseau comme `networkadapter.bytes.total` sont agrégés pour toutes les cartes réseau éligibles attachés au serveur.

## <a name="how-to-interpret"></a>Comment interpréter

| série                           | Comment interpréter                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Pourcentage de temps processeur qui n’est pas inactif.                        |
| `clusternode.cpu.usage.guest`    | Pourcentage de temps processeur utilisé pour la demande de l’invité (machine virtuelle). |
| `clusternode.cpu.usage.host`     | Pourcentage de temps processeur utilisé pour la demande d’hôte.                    |
| `clusternode.memory.total`       | La mémoire physique totale du serveur.                              |
| `clusternode.memory.available`   | La mémoire disponible du serveur.                                   |
| `clusternode.memory.usage`       | La mémoire allouée (non disponible) du serveur.                   |
| `clusternode.memory.usage.guest` | La mémoire allouée à la demande de l’invité (machine virtuelle).               |
| `clusternode.memory.usage.host`  | La mémoire allouée à la demande d’hôte.                                  |

## <a name="where-they-come-from"></a>S’ils proviennent d'

Le `cpu.*` série est collectée à partir de différents compteurs de performances selon ou non Hyper-V est activé.

Si Hyper-V est activé :

| série                           | Compteur de la source |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

À l’aide de la `% Total Run Time` compteurs garantit que l’historique des performances attributs de l’ensemble de l’utilisation.

Si Hyper-V n’est pas activé :

| série                           | Compteur de la source |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zero* |
| `clusternode.cpu.usage.host`     | *identique à l’utilisation totale* |

Nonobstant synchronisation imparfaite, `clusternode.cpu.usage` est toujours `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Avec la même restriction `clusternode.cpu.usage.guest` est toujours la somme de `vm.cpu.usage` pour toutes les machines virtuelles sur le serveur hôte.

Le `memory.*` série est (bientôt disponible).

  > [!NOTE]
  > Les compteurs sont mesurées sur tout l’intervalle, ont ne pas été échantillonnée. Par exemple, si le serveur est inactif pour 9 secondes mais des pics de 100 % du processeur dans le deuxième de 10, sa `clusternode.cpu.usage` seront enregistrées en tant que 10 % en moyenne pendant cet intervalle de 10 secondes. Cela garantit son historique des performances capture toutes les activités et sont robuste au bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) applet de commande :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
