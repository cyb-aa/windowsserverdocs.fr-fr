---
title: Conseils de configuration de carte d’Interface réseau (NIC) convergé
description: Carte d’interface réseau (NIC) vous permet d’exposer RDMA via une carte réseau virtuelle de partition de l’ordinateur hôte (vNIC) afin que les services de la partition hôte peuvent accéder à distance mémoire accès Direct (RDMA) sur les mêmes cartes réseau que les invités Hyper-V utilisé pour le trafic TCP/IP.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: e9f5180285dda790e11cec543a109d0cb58edd2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838840"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Convergé carte d’Interface réseau \(carte réseau\) des conseils de configuration

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Carte d’interface réseau convergé \(NIC\) vous permet d’exposer RDMA via un hôte\-partitionner la carte réseau virtuelle \(carte réseau virtuelle\) afin que les services de la partition hôte peuvent accéder à Remote Direct Memory Access \(RDMA\) sur les mêmes cartes réseau que les invités Hyper-V utilisé pour le trafic TCP/IP.

Avant de la fonctionnalité de carte d’interface réseau convergé, gestion \(héberger la partition\) services qui voulaient utiliser RDMA devaient utiliser RDMA dédié\-cartes réseau compatibles, même si la bande passante n’était disponible sur les cartes réseau qui étaient liés à Hyper-V Commutateur virtuel.

Avec la carte réseau convergé, les deux charges de travail \(utilisateurs de la gestion du trafic RDMA et invité\) peuvent partager les mêmes cartes réseau physiques, ce qui vous permet d’installer moins de cartes réseau sur vos serveurs.

Lorsque vous déployez la carte d’interface réseau convergé avec les hôtes Windows Server 2016 Hyper-V et commutateurs virtuels Hyper-V, les cartes réseau virtuelles dans les hôtes Hyper-V exposent des services RDMA pour les processus hôtes à l’aide de RDMA sur n’importe quel Ethernet\-en fonction de la technologie RDMA.

>[!NOTE]
>Pour utiliser la technologie de carte d’interface réseau convergé, les cartes réseau certifiées dans vos serveurs doivent prendre en charge RDMA.

Ce guide fournit deux ensembles d’instructions, une pour les déploiements où vos serveurs ont une seule carte réseau installée, qui est un déploiement de base de carte de réseau convergé ; et un autre ensemble d’instructions où vos serveurs ont deux ou plusieurs cartes réseau, qui est un déploiement de carte de réseau convergé sur un Switch Embedded Teaming \(définir\) équipe de RDMA\-cartes réseau compatibles.


## <a name="prerequisites"></a>Prérequis

Voici la configuration requise pour les déploiements de base et de centre de données de convergé la carte réseau.

>[!NOTE]
>Pour les exemples fournis, nous utilisons une carte Ethernet de Mellanox ConnectX-3 Pro 40 Gbits/s, mais vous pouvez utiliser des différents serveurs Windows certifié RDMA\-les cartes réseau compatibles qui prennent en charge cette fonctionnalité. Pour plus d’informations sur les cartes réseau compatibles, consultez la rubrique de catalogue Windows Server [cartes LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Conditions préalables de base convergé carte d’interface réseau

Pour effectuer les étapes de ce guide pour la configuration de la carte d’interface réseau convergé base, vous devez disposer des éléments suivants.

- Deux serveurs qui exécutent Windows Server 2016 Datacenter edition ou Windows Server 2016 Standard edition.
- Un prenant en charge RDMA, certifié la carte réseau installée sur chaque serveur.
- Rôle de serveur Hyper-V installé sur chaque serveur.

### <a name="datacenter-converged-nic-prerequisites"></a>Conditions préalables de centre de données convergé carte d’interface réseau

Pour effectuer les étapes de ce guide pour la configuration de carte d’interface réseau convergé de centre de données, vous devez disposer des éléments suivants.

- Deux serveurs qui exécutent Windows Server 2016 Datacenter edition ou Windows Server 2016 Standard edition.
- Deux prenant en charge RDMA, certifié des cartes réseau installées sur chaque serveur.
- Rôle de serveur Hyper-V installé sur chaque serveur.
- Vous devez être familiarisé avec Switch Embedded Teaming \(définir\), un autre réseau NIC solution utilisée dans les environnements qui incluent Hyper-V et la pile de mise en réseau SDN (Software Defined) dans Windows Server 2016. JEU intègre des fonctionnalités d’association de cartes réseau dans le commutateur virtuel Hyper-V. Pour plus d’informations, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Rubriques connexes
- [Configuration de carte réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau de carte réseau convergé](cnic-datacenter.md)
- [Configuration de commutateur physique pour la carte réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergé des Configurations de carte réseau](cnic-app-troubleshoot.md)

---