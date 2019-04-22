---
title: Rôles, Services de rôle et fonctionnalités pas dans Windows Server - Server Core
description: En savoir plus sur les rôles et les fonctionnalités non incluses dans l’option d’installation Server Core pour Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825530"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Rôles, Services de rôle et fonctionnalités pas dans Windows Server - Server Core

> S’applique à : Windows Server (canal semi-annuel) et Windows Server 2016

Les rôles, les services de rôle et les fonctionnalités suivantes ont été supprimées à partir de l’option d’installation Server Core de Windows Server. Utilisez ces informations pour aider à déterminer si l’option Server Core fonctionne pour votre environnement.

> [!NOTE]
> Vous pouvez également voir une liste des rôles, services de rôle et des fonctionnalités offertes par [sont inclus dans le Server Core](server-core-roles-and-services.md). Il s’agit d’une liste très volumineuse, pour de meilleurs résultats, recherchez dans cette liste pour le rôle spécifique ou une fonctionnalité qui vous intéresse.

## <a name="roles-not-in-server-core"></a>Rôles pas dans Server Core

- Télécopie
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Pas de Server Core, les Services de rôle
Notez que certains services de rôle du Bureau à distance sont inclus dans Server Core (connexion Broker, gestion des licences, hôte de virtualisation), mais d’autres ne le sont pas (passerelle, hôte de Session Bureau à distance, l’accès Web).

- Serveur d’analyse d’impression
- Print-Internet
- Passerelle de services Bureau à distance
- Serveur de bureau à distance de services Bureau à distance
- RDS-Web-Access
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- Déploiement de WDS
- Transport de WDS *(avant Windows Server version 1803)*

## <a name="features-not-in-server-core"></a>Fonctionnalités pas de Server Core

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Direct-Play
- Internet-Print-Client
- Moniteur de Port LPR
- MSMQ-Multicasting
- CMAK
- Assistance à distance
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- Serveur RSAT-Bits
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-Tools
- Outils de services Bureau à distance de serveur distant
- RSAT-services Bureau à distance-Gateway
- RSAT-services Bureau à distance-licence-diagnostic-interface utilisateur
- UI Gestionnaire de licences des services Bureau à distance
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-Responder
- RSAT-ADRMS
- RSAT-Fax
- Services de fichiers de serveur distant
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- RSAT-Print-Services
- Outils d’évaluation des vulnérabilités serveur distant
- AdminPack de WDS
- SMTP-serveur
- TFTP-Client
- -Redirecteur WebDAV
- Biometric Framework
- Windows-Defender-Gui
- Windows-Identity-Foundation
- PowerShell-ISE
- Service de recherche
- Windows-TIFF-IFilter
- Mise en réseau sans fil
- XPS-Viewer

