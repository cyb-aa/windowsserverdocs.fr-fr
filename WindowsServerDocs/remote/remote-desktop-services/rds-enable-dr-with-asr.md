---
title: Activer la reprise d’activité après sinistre des services Bureau à distance à l’aide d’Azure Site Recovery
description: Découvrez comment activer la reprise d’activité après sinistre des services Bureau à distance à l’aide d’Azure Site Recovery.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 7aa25602c71e5d114be7ae59c5e3ce168844d700
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446548"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Activer la reprise d’activité après sinistre des services Bureau à distance à l’aide d’Azure Site Recovery

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Pour vérifier que le déploiement des services Bureau à distance est correctement configuré en cas de reprise d’activité après sinistre, vous devez protéger tous les composants qui constituent votre déploiement des services Bureau à distance :

- Active Directory
- Niveau SQL Server
- Composants des services Bureau à distance
- Composants réseau

## <a name="configure-active-directory-and-dns-replication"></a>Configurer Active Directory et la réplication DNS

Vous avez besoin d’Active Directory sur le site de reprise d’activité après sinistre pour que votre déploiement des services Bureau à distance fonctionne. Vous avez deux options possibles en fonction de la complexité de votre déploiement des services Bureau à distance :

- Option 1 - Si vous avez un petit nombre d’applications et un seul contrôleur de domaine pour l’ensemble de votre site local, et si vous devez effectuer le basculement de ce dernier, utilisez la fonctionnalité de récupération automatique du système pour répliquer le contrôleur de domaine sur le site secondaire (aussi bien dans les scénarios de site à site que de site à Azure).
- Option 2 - Si vous avez un grand nombre d’applications et une forêt Active Directory, et si vous devez effectuer le basculement de quelques applications à la fois, configurez un contrôleur de domaine supplémentaire sur le site de reprise d’activité après sinistre (site secondaire ou Azure).

Consultez [Protéger Active Directory et DNS avec Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) pour plus d’informations sur la mise à disposition d’un contrôleur de domaine sur le site de reprise d’activité après sinistre. Pour le reste de ces informations d’aide, nous supposons que vous avez suivi ces étapes et que le contrôleur de domaine est disponible.

## <a name="set-up-sql-server-replication"></a>Configurer la réplication SQL Server

Consultez [Protéger SQL Server à l’aide de la reprise d’activité après sinistre et d’Azure Site Recovery](/azure/site-recovery/site-recovery-sql) pour connaître les étapes de configuration de la réplication SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Activer la protection des composants d’application des services Bureau à distance

En fonction de votre type de déploiement des services Bureau à distance, vous pouvez activer la protection des différentes machines virtuelles qui le composent (comme indiqué dans le tableau ci-dessous) dans Azure Site Recovery. Configurez les éléments Azure Site Recovery appropriés selon que vos machines virtuelles sont déployées sur Hyper-V ou VMWare.


|               Type de déploiement                |                                                                                                     Étapes de protection                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Bureau virtuel personnel (non managé)     | 1. Vérifiez que tous les hôtes de virtualisation disposent du rôle de serveur hôte de virtualisation des services Bureau à distance.    </br>2. Service Broker pour les connexions.  </br>3. Bureaux personnels. </br>4. Modèle de machine virtuelle maître. </br>5. Accès web, serveur de licences et serveur de passerelle |
| Bureau virtuel mis en pool (managé sans UPD) |                    1. Tous les hôtes de virtualisation disposent du rôle de serveur hôte de virtualisation des services Bureau à distance.  </br>2. Service Broker pour les connexions.  </br>3. Modèle de machine virtuelle maître. </br>4. Accès web, serveur de licences et serveur de passerelle.                    |
|   Programmes RemoteApp et sessions Bureau à distance (sans UPD)   |                                                          1. Hôtes de session.  </br>2. Service Broker pour les connexions. </br>3. Accès web, serveur de licences et serveur de passerelle.                                                           |

