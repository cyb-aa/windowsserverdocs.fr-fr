---
title: Historique des performances pour les machines virtuelles
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: aefc9c3c33cb93be241aae4ef18d815a9f8defef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856142"
---
# <a name="performance-history-for-virtual-machines"></a>Historique des performances pour les machines virtuelles

> S’applique à : Windows Server 2019

Cette sous-rubrique de l' [historique des performances pour espaces de stockage direct](performance-history.md) décrit en détail l’historique des performances collecté pour les machines virtuelles (VM). L’historique des performances est disponible pour chaque machine virtuelle en cours d’exécution.

   > [!NOTE]
   > Le démarrage de la collecte peut prendre plusieurs minutes pour les machines virtuelles nouvellement créées ou renommées.

## <a name="series-names-and-units"></a>Noms et unités des séries

Ces séries sont collectées pour chaque machine virtuelle éligible :

| Série                            | Unit             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | percent          |
| `vm.memory.assigned`              | octets            |
| `vm.memory.available`             | octets            |
| `vm.memory.maximum`               | octets            |
| `vm.memory.minimum`               | octets            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | octets            |
| `vm.memory.total`                 | octets            |
| `vmnetworkadapter.bandwidth.inbound`  | bits par seconde |
| `vmnetworkadapter.bandwidth.outbound` | bits par seconde |
| `vmnetworkadapter.bandwidth.total`    | bits par seconde |

En outre, toutes les séries de disques durs virtuels (VHD), telles que les `vhd.iops.total`, sont agrégées pour chaque disque dur virtuel attaché à la machine virtuelle.

## <a name="how-to-interpret"></a>Comment interpréter


| Série                            | Description                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Pourcentage d’utilisation de l’ordinateur virtuel par le ou les processeurs du serveur hôte.                                   |
| `vm.memory.assigned`              | Quantité de mémoire affectée à l’ordinateur virtuel.                                                      |
| `vm.memory.available`             | Quantité de mémoire qui reste disponible, du montant affecté.                                       |
| `vm.memory.maximum`               | Si vous utilisez la mémoire dynamique, il s’agit de la quantité maximale de mémoire qui peut être affectée à la machine virtuelle. |
| `vm.memory.minimum`               | Si vous utilisez la mémoire dynamique, il s’agit de la quantité minimale de mémoire qui peut être affectée à la machine virtuelle. |
| `vm.memory.pressure`              | Rapport entre la mémoire sollicitée par l’ordinateur virtuel et la mémoire allouée à l’ordinateur virtuel.            |
| `vm.memory.startup`               | Quantité de mémoire requise pour le démarrage de la machine virtuelle.                                            |
| `vm.memory.total`                 | Mémoire totale. |
| `vmnetworkadapter.bandwidth.inbound`  | Taux de données reçues par l’ordinateur virtuel sur toutes les cartes réseau virtuelles.                        |
| `vmnetworkadapter.bandwidth.outbound` | Taux de données envoyées par la machine virtuelle sur toutes les cartes réseau virtuelles.                            |
| `vmnetworkadapter.bandwidth.total`    | Taux total de données reçues ou envoyées par la machine virtuelle sur toutes les cartes réseau virtuelles.          |

   > [!NOTE]
   > Les compteurs sont mesurés sur l’intégralité de l’intervalle, et non échantillonnés. Par exemple, si la machine virtuelle est inactive pendant 9 secondes mais que les pics d’utilisation 50% de l’UC hôte dans le dixième seconde, son `vm.cpu.usage` est enregistré en moyenne 5% pendant cet intervalle de 10 secondes. Cela garantit que son historique des performances capture toutes les activités et est robuste pour le bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet de commande [obtient-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > L’applet de commande obtient-VM ne retourne que les ordinateurs virtuels sur le serveur local (ou spécifié), et non sur le cluster.

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
