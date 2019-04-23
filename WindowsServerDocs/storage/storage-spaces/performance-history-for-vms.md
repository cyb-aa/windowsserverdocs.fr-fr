---
title: Historique des performances pour les machines virtuelles
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890870"
---
# <a name="performance-history-for-virtual-machines"></a>Historique des performances pour les machines virtuelles

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collecté pour les machines virtuelles (VM). Historique des performances sont disponible pour chaque exécution, la machine virtuelle en cluster.

   > [!NOTE]
   > Il peut prendre plusieurs minutes avant que la collection commencer pour les machines virtuelles qui vient d’être créées ou renommées.

## <a name="series-names-and-units"></a>Unités et les noms de série

Ces séries sont collectées pour chaque machine virtuelle éligible :

| série                            | Unit             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | %          |
| `vm.memory.assigned`              | bytes            |
| `vm.memory.available`             | bytes            |
| `vm.memory.maximum`               | bytes            |
| `vm.memory.minimum`               | bytes            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | bytes            |
| `vm.memory.total`                 | bytes            |
| `vmnetworkadapter.bandwidth.inbound`  | Bits par seconde |
| `vmnetworkadapter.bandwidth.outbound` | Bits par seconde |
| `vmnetworkadapter.bandwidth.total`    | Bits par seconde |

En outre, toutes les séries de disque dur virtuel (VHD), tel que `vhd.iops.total`, sont agrégées pour chaque disque dur virtuel attaché à la machine virtuelle.

## <a name="how-to-interpret"></a>Comment interpréter


| série                            | Description                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Pourcentage de la machine virtuelle est à l’aide du processeur (s) de son serveur hôte.                                   |
| `vm.memory.assigned`              | La quantité de mémoire attribuée à la machine virtuelle.                                                      |
| `vm.memory.available`             | La quantité de mémoire reste disponible, la quantité affectée.                                       |
| `vm.memory.maximum`               | Si vous utilisez la mémoire dynamique, il s’agit de la quantité maximale de mémoire qui peut être affectée à la machine virtuelle. |
| `vm.memory.minimum`               | Si vous utilisez la mémoire dynamique, il s’agit de la quantité minimale de mémoire qui peut être affectée à la machine virtuelle. |
| `vm.memory.pressure`              | Rapport entre la mémoire exigée par la machine virtuelle sur la mémoire allouée à la machine virtuelle.            |
| `vm.memory.startup`               | La quantité de mémoire nécessaire pour la machine virtuelle à démarrer.                                            |
| `vm.memory.total`                 | Mémoire totale. |
| `vmnetworkadapter.bandwidth.inbound`  | Taux de données reçues par l’ordinateur virtuel sur toutes les cartes de son réseau virtuel.                        |
| `vmnetworkadapter.bandwidth.outbound` | Taux de données envoyées par l’ordinateur virtuel sur toutes ses cartes réseau virtuelles.                            |
| `vmnetworkadapter.bandwidth.total`    | Taux total de données reçus ou envoyés par l’ordinateur virtuel sur toutes les cartes de son réseau virtuel.          |

   > [!NOTE]
   > Les compteurs sont mesurées sur tout l’intervalle, ont ne pas été échantillonnée. Par exemple, si la machine virtuelle est inactive pendant 9 secondes mais pics à utiliser 50 % du processeur d’ordinateur hôte dans la deuxième de 10, sa `vm.cpu.usage` seront enregistrées en tant que 5 % en moyenne pendant cet intervalle de 10 secondes. Cela garantit son historique des performances capture toutes les activités et sont robuste au bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) applet de commande :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > L’applet de commande Get-VM retourne uniquement les machines virtuelles sur le serveur local (ou spécifié), pas au sein du cluster.

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
