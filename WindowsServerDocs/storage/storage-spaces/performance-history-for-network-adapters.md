---
title: Historique des performances pour les cartes réseau
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: e2379ce540cb26c02bc79f591d2a597874ab287c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856212"
---
# <a name="performance-history-for-network-adapters"></a>Historique des performances pour les cartes réseau

> S’applique à : Windows Server 2019

Cette sous-rubrique de l' [historique des performances pour espaces de stockage direct](performance-history.md) décrit en détail l’historique des performances collecté pour les cartes réseau. L’historique des performances de la carte réseau est disponible pour chaque carte réseau physique de chaque serveur du cluster. L’historique des performances d’accès direct à la mémoire à distance (RDMA) est disponible pour chaque carte réseau physique avec RDMA activé.

   > [!NOTE]
   > L’historique des performances ne peut pas être collecté pour les cartes réseau d’un serveur défaillant. La collecte reprendra automatiquement lorsque le serveur sera à nouveau disponible.

## <a name="series-names-and-units"></a>Noms et unités des séries

Ces séries sont collectées pour chaque carte réseau éligible :

| Série                               | Unit            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bits par seconde |
| `netadapter.bandwidth.outbound`      | bits par seconde |
| `netadapter.bandwidth.total`         | bits par seconde |
| `netadapter.bandwidth.rdma.inbound`  | bits par seconde |
| `netadapter.bandwidth.rdma.outbound` | bits par seconde |
| `netadapter.bandwidth.rdma.total`    | bits par seconde |

   > [!NOTE]
   > L’historique des performances de la carte réseau est enregistré en **bits** par seconde, et non pas en octets par seconde. La carte réseau 1 10 GbE peut envoyer et recevoir environ 1 milliard bits = 125 millions octets = 1,25 Go par seconde à sa valeur maximale théorique.

## <a name="how-to-interpret"></a>Comment interpréter

| Série                               | Comment interpréter                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Taux de données reçues par la carte réseau.                         |
| `netadapter.bandwidth.outbound`      | Taux de données envoyées par la carte réseau.                             |
| `netadapter.bandwidth.total`         | Taux total de données reçues ou envoyées par la carte réseau.           |
| `netadapter.bandwidth.rdma.inbound`  | Taux de données reçues sur RDMA par la carte réseau.               |
| `netadapter.bandwidth.rdma.outbound` | Taux de données envoyées via RDMA par la carte réseau.                   |
| `netadapter.bandwidth.rdma.total`    | Taux total de données reçues ou envoyées via RDMA par la carte réseau. |

## <a name="where-they-come-from"></a>Origine de leur provenance

Les séries `bytes.*` sont collectées à partir de l’ensemble de compteurs de performances `Network Adapter` sur le serveur sur lequel la carte réseau est installée, une instance par carte réseau.

| Série                           | Compteur source           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

Les séries `rdma.*` sont collectées à partir de l’ensemble de compteurs de performances `RDMA Activity` sur le serveur sur lequel la carte réseau est installée, une instance par carte réseau avec RDMA activé.

| Série                               | Compteur source           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *somme des éléments ci-dessus*   |

   > [!NOTE]
   > Les compteurs sont mesurés sur l’intégralité de l’intervalle, et non échantillonnés. Par exemple, si la carte réseau est inactive pendant 9 secondes mais transfère 200 bits dans le dixième seconde, son `netadapter.bandwidth.total` sera enregistré comme 20 bits par seconde en moyenne pendant cet intervalle de 10 secondes. Cela garantit que son historique des performances capture toutes les activités et est robuste pour le bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet de commande [obtient-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour espaces de stockage direct](performance-history.md)
