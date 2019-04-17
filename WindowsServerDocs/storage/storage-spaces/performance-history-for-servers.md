---
title: Historique des performances pour les serveurs
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/0s/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 33fd62376e9769c23fc6b00eefde9a9b95eb4650
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1894248"
---
# <a name="performance-history-for-servers"></a>Historique des performances pour les serveurs

> S’applique à: Windows Server initiés Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique de performances collecté pour les serveurs. Historique des performances sont disponible pour chaque serveur du cluster.

   > [!NOTE]
   > Historique des performances ne peut pas être collectées pour un serveur qui est arrêté. Collection reprendra automatiquement lorsque le serveur est à nouveau disponible.

## <a name="series-names-and-units"></a>Unités et noms de série

Ces séries sont collectées pour chaque serveur éligible:

| Série                           | Unit    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | % |
| `clusternode.cpu.usage.guest`    | % |
| `clusternode.cpu.usage.host`     | % |
| `clusternode.memory.total`       |  octets   |
| `clusternode.memory.available`   |  octets   |
| `clusternode.memory.usage`       |  octets   |
| `clusternode.memory.usage.guest` |  octets   |
| `clusternode.memory.usage.host`  |  octets   |

En outre, tel que du lecteur série `physicaldisk.size.total` sont agrégées pour tous les lecteurs éligibles attachés au serveur et la série de cartes réseau comme `networkadapter.bytes.total` sont regroupées pour toutes les cartes réseau connectés au serveur.

## <a name="how-to-interpret"></a>Comment interpréter

| Série                           | Comment interpréter                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Pourcentage de temps processeur qui n’est pas inactif.                        |
| `clusternode.cpu.usage.guest`    | Pourcentage de temps processeur utilisé pour la demande d’invité (machine virtuelle). |
| `clusternode.cpu.usage.host`     | Pourcentage de temps processeur utilisé pour la demande d’hôte.                    |
| `clusternode.memory.total`       | La mémoire physique totale du serveur.                              |
| `clusternode.memory.available`   | La mémoire disponible du serveur.                                   |
| `clusternode.memory.usage`       | La mémoire allouée (non disponible) du serveur.                   |
| `clusternode.memory.usage.guest` | La mémoire allouée à la demande d’invité (machine virtuelle).               |
| `clusternode.memory.usage.host`  | La mémoire allouée à la demande d’hôte.                                  |

## <a name="where-they-come-from"></a>D'où ils proviennent

Le `cpu.*` série est collectées à partir de différents compteurs de performance en fonction de Hyper-V est activé ou non.

Si Hyper-V est activé:

| Série                           | Compteur source |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

À l’aide de la `% Total Run Time` compteurs garantit que l’historique des performances des attributs d’utilisation toutes les.

Si Hyper-V n’est pas activé:

| Série                           | Compteur source |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *zéro* |
| `clusternode.cpu.usage.host`     | *identique à l’utilisation totale* |

Nonobstant la synchronisation imparfaite, `clusternode.cpu.usage` est toujours `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Avec le même avertissement `clusternode.cpu.usage.guest` est toujours la somme des `vm.cpu.usage` pour tous les ordinateurs virtuels sur le serveur hôte.

Le `memory.*` série est (bientôt disponible).

  > [!NOTE]
  > Compteurs sont mesurés sur tout l’intervalle, ne pas échantillonnée. Par exemple, si le serveur est inactif pour 9 secondes mais pointes au processeur de 100 % de la seconde 10, son `clusternode.cpu.usage` sera enregistrée en tant que 10 % en moyenne pendant cette période de 10 secondes. Cela garantit son historique de performances capture toutes les activités et est robuste au bruit.

## <a name="usage-in-powershell"></a>Utilisation de PowerShell

Utilisez l’applet de commande [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances de stockage espaces Direct](performance-history.md)
