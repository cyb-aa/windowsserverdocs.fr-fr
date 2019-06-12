---
title: Activer la récupération d’urgence des services Bureau à distance à l’aide d’Azure Site Recovery
description: Découvrez comment activer la récupération d’urgence des services Bureau à distance à l’aide d’Azure Site Recovery.
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446548"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Activer la récupération d’urgence des services Bureau à distance à l’aide d’Azure Site Recovery

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Pour vous assurer que votre déploiement des services Bureau à distance est correctement configuré pour la récupération d’urgence, vous devez protéger tous les composants qui constituent votre déploiement des services Bureau à distance :

- Active Directory
- Niveau de SQL Server
- Composants des services Bureau à distance
- Composants réseau

## <a name="configure-active-directory-and-dns-replication"></a>Configurer la réplication Active Directory et DNS

Vous avez besoin d’Active Directory sur le site de récupération d’urgence pour votre déploiement des services Bureau à distance fonctionne. Vous avez deux possibilités en fonction de la complexité votre déploiement des services Bureau à distance :

- Option 1 : Si vous avez un petit nombre d’applications et un seul contrôleur de domaine pour l’ensemble de votre site local et il souhaite basculer l’intégralité du site, utilisez la réplication ASR pour répliquer le contrôleur de domaine sur le site secondaire (la valeur true pour les deux scénarios de site à site et le site vers Azure).
- Option 2 : Si vous avez un grand nombre d’applications et que vous exécutez une forêt Active Directory, et vous allez basculement quelques applications à la fois, configurer un contrôleur de domaine supplémentaire sur le site de récupération d’urgence (sur un site secondaire ou dans Azure).

Consultez [protéger Active Directory et DNS avec Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) pour plus d’informations sur la disposition d’un contrôleur de domaine sur le site de récupération d’urgence. Pour le reste de ce guide, nous partons du principe que vous avez suivi ces étapes et le contrôleur de domaine disponibles.

## <a name="set-up-sql-server-replication"></a>Configurer la réplication de SQL Server

Consultez [protéger SQL Server à l’aide de la récupération d’urgence de SQL Server et Azure Site Recovery](/azure/site-recovery/site-recovery-sql) pour découvrir comment configurer la réplication de SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Activer la protection pour les composants d’application de services Bureau à distance

Selon votre type de déploiement des services Bureau à distance, vous pouvez activer la protection pour les machines virtuelles composant différent (comme indiqué dans le tableau ci-dessous) dans Azure Site Recovery. Configurer les éléments d’Azure Site Recovery appropriés selon que vos machines virtuelles sont déployées sur Hyper-V ou VMWare.


|               Type de déploiement                |                                                                                                     Procédure de protection                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Bureau virtuel personnel (non managé)     | 1. Assurez-vous que tous les hôtes de virtualisation sont prêts avec le rôle d’ordinateur RDVH installé.    </br>2. Agent de connexion.  </br>3. Bureaux personnels. </br>4. Gold modèle de machine virtuelle. </br>5. Web Access, serveur de licences et de serveur de passerelle |
| Bureau virtuel mis en pool (géré avec sans UPD) |                    1. Tous les hôtes de virtualisation sont prêts avec le rôle d’ordinateur RDVH installé.  </br>2. Agent de connexion.  </br>3. Gold modèle de machine virtuelle. </br>4. Web Access, serveur de licences et de serveur de passerelle.                    |
|   RemoteApps et Sessions de bureau (sans UPD)   |                                                          1. Hôtes de session.  </br>2. Agent de connexion. </br>3. Web Access, serveur de licences et de serveur de passerelle.                                                           |

