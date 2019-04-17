---
title: Guide de Configuration de réseau convergé Interface carte (NIC)
description: Cette rubrique fait partie de la convergé NIC Configuration Guide pour Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42f7ef674da1754253ad72ae30ad505f29ba6a7d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="converged-network-interface-card-nic-configuration-guide"></a>Guide de Configuration de réseau convergé Interface carte \(NIC\)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Carte d’interface réseau convergé \(NIC\) vous permet d’exposer RDMA par le biais d’une partition de host\ virtuel NIC \(vNIC\) afin que les services de partition hôte puissent accéder \(RDMA\) Remote Direct Memory Access sur les mêmes cartes réseau qui utilisent les invités Hyper-V pour le trafic TCP/IP.

Avant de la fonctionnalité de la carte d’interface réseau convergé, services \(host partition\) de gestion qui voulaient utiliser RDMA devaient utiliser des cartes réseau prenant en charge RDMA\ dédiés, même si la bande passante est disponible sur les cartes réseau qui ont été liées au commutateur virtuel Hyper-V.

Avec les cartes réseau convergé, les charges de deux travail \ (gestion des utilisateurs de RDMA et invité traffic\) peuvent partager les mêmes cartes réseau physiques, ce qui vous permet d’installer des cartes réseau moins de vos serveurs.

Lorsque vous déployez NIC convergée avec des ordinateurs hôtes Windows Server2016 Hyper-V et commutateurs virtuels Hyper-V, les cartes réseau virtuelles dans les ordinateurs hôtes Hyper-V exposent les services RDMA pour les processus hôte à l’aide de RDMA over sur n’importe quelle technologie RDMA basée sur Ethernet\.

>[!NOTE]
>Pour utiliser la technologie de réseau convergé, les cartes réseau certifiées dans vos serveurs doivent prendre en charge RDMA.

Ce guide fournit deux ensembles d’obtenir des instructions, une pour les déploiements où vos serveurs ont une seule carte réseau installée, qui est un déploiement de base de cartes réseau convergé. et un autre ensemble d’instructions où vos serveurs ont deux ou plusieurs cartes réseau, qui est un déploiement de carte de réseau convergé sur une équipe \(SET\) Switch Embedded Teaming de cartes réseau compatibles RDMA\.

Ce guide contient les rubriques suivantes.

- [Configuration des cartes réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau associées de cartes réseau convergé](cnic-datacenter.md)
- [Configuration du commutateur physique pour les cartes réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergent des Configurations de cartes réseau](cnic-app-troubleshoot.md)

## <a name="prerequisites"></a>Conditions préalables

Voici la configuration requise pour les déploiements de base et le centre de données de convergé la carte réseau.

>[!NOTE]
>Pour les exemples de ce guide, un adaptateur Ethernet de Mellanox ConnectX-3 Pro 40Gbits/s est utilisé, mais vous pouvez utiliser une de Windows Server certifié compatible RDMA\ cartes réseau qui prennent en charge cette fonctionnalité. Pour plus d’informations sur les cartes réseau compatibles, voir la rubrique catalogue Windows Server [cartes LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Conditions préalables convergé les cartes réseau de base

Pour effectuer les étapes de ce guide pour la configuration de base convergé de cartes réseau, vous devez disposer les éléments suivants.

- Deux serveurs qui exécutent Windows Server2016 Datacenter edition ou Windows Server2016, Standard edition.
- Un compatibles RDMA, certifié la carte réseau installée sur chaque serveur.
- Le rôle de serveur Hyper-V doit être installé sur chaque serveur.

### <a name="datacenter-converged-nic-prerequisites"></a>Conditions préalables de centre de données convergé de cartes réseau

Pour effectuer les étapes de ce guide pour la configuration de carte de réseau convergé de centre de données, vous devez disposer des éléments suivants.

- Deux serveurs qui exécutent Windows Server2016 Datacenter edition ou Windows Server2016, Standard edition.
- La certification deux en charge RDMA, les cartes réseau installées sur chaque serveur.
- Le rôle de serveur Hyper-V doit être installé sur chaque serveur.
- Vous devez être familiarisé avec Switch Embedded Teaming \(SET\), qui est une alternative la solution d’association de cartes réseau que vous pouvez utiliser dans les environnements qui incluent Hyper-V et la pile logicielle définie de mise en réseau (SDN) dans Windows Server2016. ENSEMBLE intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. Pour plus d’informations, voir [accès à la mémoire Direct à distance (RDMA) et le commutateur SET (Embedded Teaming)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

