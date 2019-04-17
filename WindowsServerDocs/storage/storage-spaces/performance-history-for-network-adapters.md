---
title: Historique des performances pour les cartes réseau
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239236"
---
# Historique des performances pour les cartes réseau

> S’applique à: Windows Server Insider Preview

Cette rubrique secondaire de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collectées pour les cartes réseau. Historique des performances de carte réseau n’est disponible pour chaque carte réseau physique dans chaque serveur du cluster. Historique de performances à distance un accès Direct à la mémoire (RDMA) est disponible pour chaque carte réseau physique avec RDMA activé.

   > [!NOTE]
   > Historique des performances ne peuvent pas être collectés pour les cartes réseau dans un serveur qui est arrêté. Collection reprend automatiquement lorsque le serveur est à nouveau disponible.

## Unités publicitaires et les noms de série

Ces séries sont collectés pour toutes les cartes réseau éligibles:

| Série                               | Unit            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bits par seconde |
| `netadapter.bandwidth.outbound`      | bits par seconde |
| `netadapter.bandwidth.total`         | bits par seconde |
| `netadapter.bandwidth.rdma.inbound`  | bits par seconde |
| `netadapter.bandwidth.rdma.outbound` | bits par seconde |
| `netadapter.bandwidth.rdma.total`    | bits par seconde |

   > [!NOTE]
   > Historique des performances de carte réseau sont enregistré en **bits** par seconde, pas les octets par seconde. Une carte réseau de 10 GbE peut envoyer et recevoir environ 1 000 000 000 bits = 125,000,000 octets = 1,25 Go par seconde à sa valeur maximale théorique.

## Comment interpréter

| Série                               | Comment interpréter                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Taux de données reçues par la carte réseau.                         |
| `netadapter.bandwidth.outbound`      | Taux de données envoyées par la carte réseau.                             |
| `netadapter.bandwidth.total`         | Vitesse totale de données reçus ou envoyés par la carte réseau.           |
| `netadapter.bandwidth.rdma.inbound`  | Taux de données reçues sur RDMA par la carte réseau.               |
| `netadapter.bandwidth.rdma.outbound` | Taux de données envoyées au RDMA par la carte réseau.                   |
| `netadapter.bandwidth.rdma.total`    | Vitesse totale de données reçus ou envoyés sur RDMA par la carte réseau. |

## S’ils proviennent d'

Le `bytes.*` séries sont collectées à partir de la `Network Adapter` compteur de performance défini sur le serveur où est installée la carte réseau, une seule instance par carte réseau.

| Série                           | Compteur de source           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

Le `rdma.*` séries sont collectées à partir de la `RDMA Activity` compteur de performance défini sur le serveur où est installée la carte réseau, une seule instance par la carte réseau avec RDMA activé.

| Série                               | Compteur de source           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *somme ci-dessus*   |

   > [!NOTE]
   > Compteurs de performance sont mesurées sur l’intervalle entière, ne pas échantillonné. Par exemple, si la carte réseau est inactive pendant 9 secondes, mais les transferts bits 200 dans la deuxième 10, son `netadapter.bandwidth.total` est enregistré en tant que 20 bits par seconde en moyenne pendant cette période de 10 secondes. Cela garantit son historique de performances capture toutes les activités et est robuste au bruit.

## Utilisation de PowerShell

Utilisez l’applet de commande [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## Articles associés

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
