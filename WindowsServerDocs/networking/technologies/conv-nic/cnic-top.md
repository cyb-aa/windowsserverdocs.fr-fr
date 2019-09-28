---
title: Guide de configuration de la carte d’interface réseau (NIC) convergée
description: La carte d’interface réseau (NIC) convergée vous permet d’exposer RDMA via une carte réseau virtuelle de partition d’hôte (carte réseau virtuelle) afin que les services de partition d’hôte puissent accéder à l’accès direct à la mémoire à distance (RDMA) sur les mêmes cartes réseau que celles que les invités Hyper-V utilisent pour le trafic TCP/IP.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d791e0d51278d1f83f344250d38b1c7005c1a14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355435"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Carte d’interface réseau convergée \(NIC @ no__t-1 Guide de configuration

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

La carte d’interface réseau convergée \(NIC @ no__t-1 vous permet d’exposer RDMA via une carte réseau virtuelle hôte @ no__t-2partition \(vNIC @ no__t-4 afin que les services de partition de l’hôte puissent accéder à l’accès direct à la mémoire à distance \(RDMA @ no__t-6 sur les mêmes cartes réseau. que les invités Hyper-V utilisent pour le trafic TCP/IP.

Avant la fonctionnalité de carte réseau convergée, les services de la partition \(host de gestion @ no__t-1 qui souhaitaient utiliser RDMA étaient requis pour utiliser des cartes réseau RDMA @ no__t-2capable dédiées, même si la bande passante était disponible sur les cartes réseau qui étaient liées au commutateur virtuel Hyper-V.

Avec la carte réseau convergée, les deux charges de travail @no__t les utilisateurs 0management de RDMA et le trafic invité @ no__t-1 peuvent partager les mêmes cartes réseau physiques, ce qui vous permet d’installer moins de cartes réseau dans vos serveurs.

Lorsque vous déployez une carte réseau convergée avec des ordinateurs hôtes Hyper-V Windows Server 2016 et des commutateurs virtuels Hyper-V, les cartes réseau virtuelles dans les hôtes Hyper-V exposent les services RDMA pour héberger les processus à l’aide de RDMA sur toute technologie RDMA Ethernet @ no__t-0based.

>[!NOTE]
>Pour utiliser la technologie de cartes réseau convergée, les cartes réseau certifiées de vos serveurs doivent prendre en charge RDMA.

Ce guide fournit deux ensembles d’instructions, un pour les déploiements où les serveurs ont une seule carte réseau installée, qui est un déploiement de base de la carte réseau convergée. et un autre ensemble d’instructions sur lesquelles vos serveurs ont deux cartes réseau ou plus installées, qui est un déploiement de carte réseau convergée sur une association incorporée de commutateur \(SET @ no__t-1 équipe de cartes réseau RDMA @ no__t-2capable.


## <a name="prerequisites"></a>Prérequis

Vous trouverez ci-dessous les conditions préalables pour les déploiements de base et Datacenter de la carte réseau convergée.

>[!NOTE]
>Pour les exemples fournis, nous utilisons une carte Ethernet Mellanox ConnectX-3 Pro 40 Gbits/s, mais vous pouvez utiliser n’importe quelle carte réseau RDMA @ no__t-0capable Windows Server certifiée qui prend en charge cette fonctionnalité. Pour plus d’informations sur les cartes réseau compatibles, consultez la rubrique Windows Server Catalog [Cards LAN Cards](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Configuration requise pour la carte réseau de base convergée

Pour effectuer les étapes de ce guide pour une configuration de carte réseau convergée de base, vous devez disposer des éléments suivants.

- Deux serveurs qui exécutent Windows Server 2016 Datacenter Edition ou Windows Server 2016 Standard Edition.
- Une carte réseau certifiée avec connexion RDMA installée sur chaque serveur.
- Rôle de serveur Hyper-V installé sur chaque serveur.

### <a name="datacenter-converged-nic-prerequisites"></a>Configuration requise des cartes réseau convergées pour les centres de.

Pour effectuer les étapes décrites dans ce guide pour la configuration des cartes réseau convergées par le centre de informations, vous devez disposer des éléments suivants.

- Deux serveurs qui exécutent Windows Server 2016 Datacenter Edition ou Windows Server 2016 Standard Edition.
- Deux cartes réseau certifiées et basées sur RDMA installées sur chaque serveur.
- Rôle de serveur Hyper-V installé sur chaque serveur.
- Vous devez vous familiariser avec switch Embedded Teaming \(SET @ no__t-1, une autre solution d’association de cartes réseau utilisée dans les environnements qui incluent Hyper-V et la pile de mise en réseau à définition logicielle (SDN) dans Windows Server 2016. SET intègre certaines fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. Pour plus d’informations, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Rubriques connexes
- [Configuration de carte réseau convergée avec une seule carte réseau](cnic-single.md)
- [Configuration de carte réseau associée à une carte réseau convergée](cnic-datacenter.md)
- [Configuration du commutateur physique pour la carte réseau convergée](cnic-app-switch-config.md)
- [Résolution des problèmes de configuration de cartes réseau convergées](cnic-app-troubleshoot.md)

---