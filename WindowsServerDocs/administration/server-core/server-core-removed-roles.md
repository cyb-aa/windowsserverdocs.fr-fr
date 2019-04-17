---
title: Rôles, Services de rôle et fonctionnalités pas dans Windows Server - Server Core
description: Obtenir des informations sur les rôles et les fonctionnalités non incluses dans l’option d’installation Server Core de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2018
ms.locfileid: "2604787"
---
# Rôles, Services de rôle et fonctionnalités pas dans Windows Server - Server Core

> S’applique à: Windows Server (Channel annuel séparées) et Windows Server 2016

Les rôles, services de rôle et fonctionnalités suivantes ont été supprimées de l’option d’installation Server Core de Windows Server. Utiliser ces informations pour aider à déterminer si l’option Server Core fonctionne pour votre environnement.

> [!NOTE]
> Vous pouvez également consulter une liste de rôles, services de rôle, les fonctionnalités qui [sont inclus dans le serveur principal](server-core-roles-and-services.md). Il s’agit d’une très grande liste, pour des résultats optimaux, recherchez dans cette liste pour le rôle spécifique ou une fonctionnalité qui vous intéresse.

## Server Core de rôles

- Télécopie
- MultiPointServerRole
- NPAS
- WDS

## Services de rôle pas dans Server Core
Notez que certains services de rôle Bureau à distance sont incluses dans Server Core (connexion Broker, licences, hôte de virtualisation), mais d’autres ne le sont pas (passerelle, hôte de Session Bureau à distance, l’accès Web).

- Serveur d’analyse d’impression
- Impression-Internet
- Passerelle RDS
- Serveur RDS-Bureau à distance
- Accès-Web-RDS
- Console de gestion Web
- Console Web-Lgcy-Mgmt
- WDS-déploiement
- WDS-Transport *(avant Windows Server version 1803)*

## Server Core de fonctionnalités

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Direct-Play
- Client de l’impression Internet
- Moniteur de Port LPR
- MSMQ-multidiffusion
- CONNECTION MANAGER
- Assistance à distance
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- Serveur RSAT-Bits
- RSAT-ÉQUILIBRAGE DE CHARGE RÉSEAU
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-Tools
- Outils RSAT-RDS
- RSAT-RDS-passerelle
- RSAT-RDS-licence-diagnostic-interface utilisateur
- Interface RDS utilisateur gestion des licences
- UpdateServices-interface utilisateur
- RSAT-ADC
- RSAT-ADC-Mgmt
- Répondeur en ligne RSAT
- RSAT-ADRMS
- RSAT-télécopie
- Services de fichiers RSAT
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- Services d’impression RSAT
- Outils RSAT-VA
- WDS-AdminPack
- Serveur SMTP
- Client TFTP
- Redirecteur de WebDAV
- Biométrique Framework
- Windows-Defender-Gui
- Identité-Windows-Foundation
- PowerShell ISE
- Service de recherche
- Windows-TIFF-IFilter
- Mise en réseau sans fil
- Visionneuse XPS

