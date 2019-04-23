---
title: Quelles sont les nouveautés dans SDN pour Windows Server
description: Cette rubrique fournit des informations sur les nouvelles fonctionnalités Sdn de Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: ee9ec68bbbb0be2befd432fbcc692ce177e8fd2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857950"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Quelles sont les nouveautés dans SDN pour Windows Server 2019

>S’applique à : Windows Server (canal semi-annuel)


| **Fonctionnalité** | **Description** | **Nouveau/mis à jour** | 
| --- | --- | --- |
|[Réseaux chiffrés](vnet-encryption/sdn-vnet-encryption.md) |Chiffrement de réseau virtuel, le chiffrement du trafic de réseau virtuel entre des machines virtuelles qui communiquent entre eux au sein de sous-réseaux marqués comme « Chiffrement Enabled ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l'altération et la falsification par toute personne ayant accès au réseau physique. |Nouveau |
|[L’audit de pare-feu](security/sdn-firewall-auditing.md) |L’audit de pare-feu est une nouvelle fonctionnalité du pare-feu SDN dans Windows Server 2019. Lorsque vous activez le pare-feu SDN, n’importe quel flux traitée par des règles de pare-feu SDN (ACL) qui ont activé la journalisation obtient enregistrée. |Nouveau |
|[Homologation de réseaux virtuels](vnet-peering/sdn-vnet-peering.md) |Homologation de réseaux virtuels vous permet de connecter deux réseaux virtuels en toute transparence. Une fois homologués, à des fins de connectivité, les réseaux virtuels apparaissent comme un seul.  |Nouveau |
|[Sortie de contrôle](manage/sdn-egress.md) |Cette nouvelle fonctionnalité dans Windows Server 2019 permet SDN offrir des compteurs d’utilisation pour les transferts de données sortantes. Avec cette fonctionnalité ajoutée, conserve de contrôleur de réseau une liste d’autorisation par réseau virtuel de toutes les plages d’adresses IP utilisées dans SDN et prendre en compte tout paquet lié pour une destination qui n’est pas incluse dans un de ces plages à être facturé transferts de données sortantes. |Nouveau |
---



