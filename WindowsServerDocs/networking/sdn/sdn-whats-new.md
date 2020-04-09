---
title: Nouveautés de SDN pour Windows Server
description: Cette rubrique fournit des informations sur les nouvelles fonctionnalités de mise en réseau définies par logiciel pour Windows Server 1709
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: efad919b-e9e7-4a0c-b373-e68a092f93b5
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: 5c6e90b590af9292b1749acdbdbc143752a1e30a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854392"
---
# <a name="whats-new-in-sdn-for-windows-server-2019"></a>Nouveautés de la mise en réseau SDN (Software Defined Networking) pour Windows Server 2019

>S’applique à : Windows Server (canal semi-annuel)


|                         **Fonctionnalité**                          |                                                                                                                                                                                         **Description**                                                                                                                                                                                         | **Nouveau/mis à jour** |
|--------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| [Réseaux chiffrés](vnet-encryption/sdn-vnet-encryption.md) | Le chiffrement de réseau virtuel permet le chiffrement du trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles au sein des sous-réseaux marqués comme « chiffrement activé ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l'altération et la falsification par toute personne ayant accès au réseau physique. |       Nouveau       |
|    [Audit du pare-feu](security/sdn-firewall-auditing.md)    |                                                                                            L’audit de pare-feu est une nouvelle fonctionnalité du pare-feu SDN dans Windows Server 2019. Lorsque vous activez le pare-feu SDN, tout workflow traité par les règles de pare-feu SDN (ACL) pour lequel la journalisation est activée est enregistré.                                                                                            |       Nouveau       |
| [Peering de réseau virtuel](vnet-peering/sdn-vnet-peering.md)  |                                                                                                                      L’homologation de réseaux virtuels vous permet de connecter deux réseaux virtuels en toute transparence. Une fois homologués, à des fins de connectivité, les réseaux virtuels apparaissent comme un seul.                                                                                                                      |       Nouveau       |
|           [Contrôle de sortie](manage/sdn-egress.md)            |                  Cette nouvelle fonctionnalité de Windows Server 2019 permet à SDN d’offrir des compteurs d’utilisation pour les transferts de données sortantes. Une fois cette fonctionnalité ajoutée, le contrôleur de réseau conserve une liste verte par réseau virtuel de toutes les plages d’adresses IP utilisées dans SDN et considère les paquets liés pour une destination qui n’est pas incluse dans l’une de ces plages comme étant des transferts de données sortants facturés.                   |       Nouveau       |

---



