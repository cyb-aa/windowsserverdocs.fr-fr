---
title: Historique des performances pour les serveurs
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: cf4bdabb132c832370e5dffec215c24b54aebdd7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856192"
---
# <a name="performance-history-for-servers"></a>Historique des performances pour les serveurs

> S’applique à : Windows Server 2019

Cette sous-rubrique de l' [historique des performances pour espaces de stockage direct](performance-history.md) décrit en détail l’historique des performances collecté pour les serveurs. L’historique des performances est disponible pour chaque serveur du cluster.

   > [!NOTE]
   > L’historique des performances ne peut pas être collecté pour un serveur qui est inactif. La collecte reprendra automatiquement lorsque le serveur sera à nouveau disponible.

## <a name="series-names-and-units"></a>Noms et unités des séries

Ces séries sont collectées pour chaque serveur éligible :

| Série                           | Unit    |
|----------------------------------|---------|
| `clusternode.cpu.usage`          | percent |
| `clusternode.cpu.usage.guest`    | percent |
| `clusternode.cpu.usage.host`     | percent |
| `clusternode.memory.total`       | octets   |
| `clusternode.memory.available`   | octets   |
| `clusternode.memory.usage`       | octets   |
| `clusternode.memory.usage.guest` | octets   |
| `clusternode.memory.usage.host`  | octets   |

En outre, les séries de lecteurs comme les `physicaldisk.size.total` sont agrégées pour tous les lecteurs éligibles connectés au serveur, et les séries de cartes réseau telles que les `networkadapter.bytes.total` sont agrégées pour toutes les cartes réseau éligibles attachées au serveur.

## <a name="how-to-interpret"></a>Comment interpréter

| Série                           | Comment interpréter                                                      |
|----------------------------------|-----------------------------------------------------------------------|
| `clusternode.cpu.usage`          | Pourcentage de temps processeur qui n’est pas inactif.                        |
| `clusternode.cpu.usage.guest`    | Pourcentage de temps processeur utilisé pour la demande d’invité (machine virtuelle). |
| `clusternode.cpu.usage.host`     | Pourcentage de temps processeur utilisé pour la demande de l’hôte.                    |
| `clusternode.memory.total`       | Mémoire physique totale du serveur.                              |
| `clusternode.memory.available`   | Mémoire disponible du serveur.                                   |
| `clusternode.memory.usage`       | Mémoire allouée (non disponible) du serveur.                   |
| `clusternode.memory.usage.guest` | Mémoire allouée à la demande d’invité (machine virtuelle).               |
| `clusternode.memory.usage.host`  | Mémoire allouée pour héberger la demande.                                  |

## <a name="where-they-come-from"></a>Origine de leur provenance

Les séries `cpu.*` sont collectées à partir de différents compteurs de performances, selon que Hyper-V est activé ou non.

Si Hyper-V est activé :

| Série                           | Compteur source |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Hyper-V Hypervisor Logical Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.guest`    | `Hyper-V Hypervisor Virtual Processor` > `_Total` > `% Total Run Time`      |
| `clusternode.cpu.usage.host`     | `Hyper-V Hypervisor Root Virtual Processor` > `_Total` > `% Total Run Time` |

L’utilisation des compteurs `% Total Run Time` garantit que l’ensemble des attributs d’historique des performances sont utilisés.

Si Hyper-V n’est pas activé :

| Série                           | Compteur source |
|----------------------------------|----------------|
| `clusternode.cpu.usage`          | `Processor` > `_Total` > `% Processor Time` |
| `clusternode.cpu.usage.guest`    | *nuls* |
| `clusternode.cpu.usage.host`     | *identique à l’utilisation totale* |

Nonobstant la synchronisation imparfaite, `clusternode.cpu.usage` est toujours `clusternode.cpu.usage.host` plus `clusternode.cpu.usage.guest`.

Avec le même inconvénient, `clusternode.cpu.usage.guest` est toujours la somme des `vm.cpu.usage` pour toutes les machines virtuelles sur le serveur hôte.

Les séries `memory.*` sont disponibles PROCHAINEment.

  > [!NOTE]
  > Les compteurs sont mesurés sur l’intégralité de l’intervalle, et non échantillonnés. Par exemple, si le serveur est inactif pendant 9 secondes mais pointe vers 100% d’UC dans le dixième seconde, son `clusternode.cpu.usage` est enregistré sous la forme de 10% en moyenne pendant cet intervalle de 10 secondes. Cela garantit que son historique des performances capture toutes les activités et est robuste pour le bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet [de commande obten-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode) :

```PowerShell
Get-ClusterNode <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
