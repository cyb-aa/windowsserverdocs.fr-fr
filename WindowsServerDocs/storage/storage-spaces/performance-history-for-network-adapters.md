---
title: Historique des performances pour les cartes réseau
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849980"
---
# <a name="performance-history-for-network-adapters"></a>Historique des performances pour les cartes réseau

> S'applique à : Présentation de Windows Server Insider

Cette sous-rubrique de [l’historique des performances pour les espaces de stockage Direct](performance-history.md) décrit en détail l’historique des performances collecté pour les cartes réseau. Historique des performances de carte réseau est disponible pour chaque carte réseau physique sur chaque serveur dans le cluster. Historique des performances de l’accès Direct à la mémoire (RDMA) à distance est disponible pour chaque carte réseau physique avec RDMA activé.

   > [!NOTE]
   > Historique des performances ne peuvent pas être collectées pour les cartes réseau sur un serveur qui est arrêté. Collection reprendra automatiquement lorsque le serveur redevient opérationnel.

## <a name="series-names-and-units"></a>Unités et les noms de série

Ces séries sont collectées pour chaque carte réseau éligibles :

| série                               | Unit            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | Bits par seconde |
| `netadapter.bandwidth.outbound`      | Bits par seconde |
| `netadapter.bandwidth.total`         | Bits par seconde |
| `netadapter.bandwidth.rdma.inbound`  | Bits par seconde |
| `netadapter.bandwidth.rdma.outbound` | Bits par seconde |
| `netadapter.bandwidth.rdma.total`    | Bits par seconde |

   > [!NOTE]
   > Historique des performances de carte réseau sont enregistrée dans **bits** par seconde, et non en octets par seconde. Une carte réseau 10 GbE peut envoyer et recevoir d’environ 1 000 000 000 bits = 125,000,000 octets = 1,25 Go par seconde à son maximum théorique.

## <a name="how-to-interpret"></a>Comment interpréter

| série                               | Comment interpréter                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Taux de données reçues par la carte réseau.                         |
| `netadapter.bandwidth.outbound`      | Taux de données envoyées par la carte réseau.                             |
| `netadapter.bandwidth.total`         | Taux total de données reçus ou envoyés par la carte réseau.           |
| `netadapter.bandwidth.rdma.inbound`  | Taux de données reçues sur RDMA sur la carte réseau.               |
| `netadapter.bandwidth.rdma.outbound` | Taux de données envoyées sur RDMA par la carte réseau.                   |
| `netadapter.bandwidth.rdma.total`    | Taux total de données reçus ou envoyés sur RDMA par la carte réseau. |

## <a name="where-they-come-from"></a>S’ils proviennent d'

Le `bytes.*` série est collectée à partir de la `Network Adapter` compteur de performance définies sur le serveur où la carte réseau est installée, une seule instance par carte réseau.

| série                           | Compteur de la source           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

Le `rdma.*` série est collectée à partir de la `RDMA Activity` compteur de performance définies sur le serveur où la carte réseau est installée, une seule instance par carte réseau avec RDMA activé.

| série                               | Compteur de la source           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | × 8 *somme des éléments ci-dessus*   |

   > [!NOTE]
   > Les compteurs sont mesurées sur tout l’intervalle, ont ne pas été échantillonnée. Par exemple, si la carte réseau est inactive pour 9 secondes, mais les transferts bits 200 dans le deuxième de 10, sa `netadapter.bandwidth.total` seront enregistrées comme 20 bits par seconde en moyenne pendant cet intervalle de 10 secondes. Cela garantit son historique des performances capture toutes les activités et sont robuste au bruit.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) applet de commande :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Voir aussi

- [Historique des performances pour les espaces de stockage Direct](performance-history.md)
