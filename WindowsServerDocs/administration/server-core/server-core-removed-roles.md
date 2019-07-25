---
title: Rôles, services de rôle et fonctionnalités absents de Windows Server-Server Core
description: En savoir plus sur les rôles et fonctionnalités qui ne sont pas inclus dans l’option d’installation minimale de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce5107e8e0ab573df7588428db65c8b223cf1f13
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476515"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Rôles, services de rôle et fonctionnalités absents de Windows Server-Server Core

> S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Les rôles, services de rôle et fonctionnalités suivants ont été supprimés de l’option d’installation Server Core de Windows Server. Utilisez ces informations pour déterminer si l’option Server Core fonctionne pour votre environnement.

> [!NOTE]
> Vous pouvez également consulter la liste des rôles, des services de rôle et des fonctionnalités [inclus dans Server Core](server-core-roles-and-services.md). Il s’agit d’une très grande liste, afin d’obtenir de meilleurs résultats, recherchez dans cette liste le rôle ou la fonctionnalité spécifique qui vous intéresse.

## <a name="roles-not-in-server-core"></a>Rôles non dans Server Core

- Télécopie
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Services de rôle absents de Server Core
Notez que certains services de rôle Bureau à distance sont inclus dans Server Core (Service Broker de connexion, licence, hôte de virtualisation), mais d’autres ne le sont pas (passerelle, hôte de session Bureau à distance, Accès web).

- Impression-analyse-serveur
- Imprimer-Internet
- RDS-passerelle
- RDS-RD-Server
- RDS-accès Web
- Web-Mgmt-console
- Web-Lgcy-Mgmt-console
- WDS-déploiement
- WDS-transport *(avant Windows Server version 1803)*

## <a name="features-not-in-server-core"></a>Fonctionnalités non disponibles dans Server Core

- BITS-IIS-Ext
- NetworkUnlock BitLocker
- Lecture directe
- Internet-Print-client
- LPR-port-moniteur
- MSMQ-multidiffusion
- CMAK
- Assistance à distance
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-bits-Server
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-outils
- RSAT-RDS-outils
- RSAT-RDS-Gateway
- RSAT-RDS-Licensing-diagnostic-UI
- RDS-licence-UI
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-répondeur
- RSAT-ADRMS
- RSAT-télécopie
- RSAT-fichiers-services
- RSAT-DFS-Mgmt-con
- RSAT-FSRM-Mgmt
- RSAT-NFS-admin
- RSAT-NPAS
- RSAT-Print-Services
- RSAT-VA-Tools
- WDS-AdminPack
- Serveur SMTP
- TFTP-client
- Redirecteur WebDAV
- Biométrique-infrastructure
- Windows-Defender-GUI
- Windows-Identity-Foundation
- PowerShell-ISE
- Service de recherche
- Windows-TIFF-IFilter
- Réseau sans fil
- Visionneuse XPS

