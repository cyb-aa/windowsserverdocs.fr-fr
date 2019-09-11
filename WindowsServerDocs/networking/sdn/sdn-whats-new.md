---
title: Nouveautés de SDN pour Windows Server
description: Cette rubrique fournit des informations sur les nouvelles fonctionnalités de mise en réseau définies par logiciel pour Windows Server 1709
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 803ca27ca138281cbea1a93aca7e5a8b799bd862
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870159"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Nouveautés de la mise en réseau SDN (Software Defined Networking) pour Windows Server 2019

>S’applique à : Windows Server (canal semi-annuel)


|                         **Fonctionnalité**                          |                                                                                                                                                                                         **Description**                                                                                                                                                                                         | **Nouveau/mis à jour** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Réseaux chiffrés](vnet-encryption/sdn-vnet-encryption.md) | Le chiffrement de réseau virtuel permet le chiffrement du trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles au sein des sous-réseaux marqués comme « chiffrement activé ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l'altération et la falsification par toute personne ayant accès au réseau physique. |       Nouveau       |
|    [Audit du pare-feu](security/sdn-firewall-auditing.md)    |                                                                                            L’audit de pare-feu est une nouvelle fonctionnalité du pare-feu SDN dans Windows Server 2019. Lorsque vous activez le pare-feu SDN, tout workflow traité par les règles de pare-feu SDN (ACL) pour lequel la journalisation est activée est enregistré.                                                                                            |       Nouveau       |
| [Homologation de réseau virtuel](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      L’homologation de réseaux virtuels vous permet de connecter deux réseaux virtuels en toute transparence. Une fois homologués, à des fins de connectivité, les réseaux virtuels apparaissent comme un seul.                                                                                                                      |       Nouveau       |
|           [Contrôle de sortie](manage/sdn-egress.md)            |                  Cette nouvelle fonctionnalité de Windows Server 2019 permet à SDN d’offrir des compteurs d’utilisation pour les transferts de données sortantes. Une fois cette fonctionnalité ajoutée, le contrôleur de réseau conserve une liste verte par réseau virtuel de toutes les plages d’adresses IP utilisées dans SDN et considère les paquets liés pour une destination qui n’est pas incluse dans l’une de ces plages comme étant des transferts de données sortants facturés.                   |       Nouveau       |

---



