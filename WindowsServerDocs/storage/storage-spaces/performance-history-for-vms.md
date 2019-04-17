---
title: Historique des performances pour les machines virtuelles
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239216"
---
# Historique des performances pour les machines virtuelles

> S’applique à: Windows Server Insider Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collectées pour les machines virtuelles (VM). Historique des performances est disponible pour chaque en cours d’exécution, ordinateur virtuel en cluster.

   > [!NOTE]
   > Elle peut prendre plusieurs minutes pour la collecte de commencer pour les ordinateurs virtuels nouvellement créés ou renommés.

## Unités publicitaires et les noms de série

Ces séries sont collectés pour chaque ordinateur virtuel éligible:

| Série                            | Unit             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | %          |
| `vm.memory.assigned`              |  octets            |
| `vm.memory.available`             |  octets            |
| `vm.memory.maximum`               |  octets            |
| `vm.memory.minimum`               |  octets            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               |  octets            |
| `vm.memory.total`                 |  octets            |
| `vmnetworkadapter.bandwidth.inbound`  | bits par seconde |
| `vmnetworkadapter.bandwidth.outbound` | bits par seconde |
| `vmnetworkadapter.bandwidth.total`    | bits par seconde |

En outre, toutes les séries de disque dur virtuel (VHD), tels que `vhd.iops.total`, sont agrégées pour chaque disque dur virtuel attaché à la machine virtuelle.

## Comment interpréter


| Série                            | Description                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | Pourcentage de la machine virtuelle est à l’aide de processeur (s) du serveur de son hôte.                                   |
| `vm.memory.assigned`              | La quantité de mémoire allouée à la machine virtuelle.                                                      |
| `vm.memory.available`             | La quantité de mémoire qui reste disponible, la quantité affectée.                                       |
| `vm.memory.maximum`               | Si vous utilisez la mémoire dynamique, il s’agit de la quantité maximale de mémoire qui peut être affectée à la machine virtuelle. |
| `vm.memory.minimum`               | Si vous utilisez la mémoire dynamique, il s’agit de la quantité minimale de mémoire qui peut être affectée à la machine virtuelle. |
| `vm.memory.pressure`              | Le coefficient de mémoire exigée par la machine virtuelle au cours de la mémoire allouée à la machine virtuelle.            |
| `vm.memory.startup`               | La quantité de mémoire requise pour la machine virtuelle à démarrer.                                            |
| `vm.memory.total`                 | Mémoire totale. |
| `vmnetworkadapter.bandwidth.inbound`  | Taux de données reçues par l’ordinateur virtuel sur toutes les cartes de son réseau virtuel.                        |
| `vmnetworkadapter.bandwidth.outbound` | Taux de données envoyées par la machine virtuelle sur toutes les cartes de son réseau virtuel.                            |
| `vmnetworkadapter.bandwidth.total`    | Vitesse totale de données reçus ou envoyés par l’ordinateur virtuel sur toutes les cartes de son réseau virtuel.          |

   > [!NOTE]
   > Compteurs de performance sont mesurées sur l’intervalle entière, ne pas échantillonné. Par exemple, si l’ordinateur virtuel est inactif pendant 9 secondes, mais les pics d’utiliser 50 % de l’UC hôte dans la deuxième 10, son `vm.cpu.usage` est enregistré en tant que 5 % en moyenne pendant cette période de 10 secondes. Cela garantit son historique de performances capture toutes les activités et est robuste au bruit.

## Utilisation de PowerShell

Utilisez l’applet de commande [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) :

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > L’applet de commande Get-VM retourne uniquement les machines virtuelles sur le serveur local (ou spécifié), pas sur le cluster.

## Articles associés

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
