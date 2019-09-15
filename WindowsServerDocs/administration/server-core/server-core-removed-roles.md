---
title: Rôles, services de rôle et fonctionnalités absents de Windows Server-Server Core
description: En savoir plus sur les rôles et fonctionnalités qui ne sont pas inclus dans l’option d’installation minimale de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 882410792c7b8df8a8275c357d64fc17c9f3479e
ms.sourcegitcommit: feec5cbe983c8c5800ccd4fc214914084fcceaba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975259"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Rôles, services de rôle et fonctionnalités absents de Windows Server-Server Core

> S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Les rôles, services de rôle et fonctionnalités suivants ont été supprimés de l’option d’installation Server Core de Windows Server. Utilisez ces informations pour déterminer si l’option Server Core fonctionne pour votre environnement.

> [!NOTE]
> Vous pouvez également consulter la liste des rôles, des services de rôle et des fonctionnalités [inclus dans Server Core](server-core-roles-and-services.md). Il s’agit d’une très grande liste, afin d’obtenir de meilleurs résultats, recherchez dans cette liste le rôle ou la fonctionnalité spécifique qui vous intéresse.

## <a name="roles-not-in-server-core"></a>Rôles non dans Server Core

- Serveur de télécopie (**télécopie**)
- MultiPoint services (**MultiPointServerRole**)
- Services de stratégie et d’accès réseau (**NPAs**)
- Services de déploiement Windows (**WDS**) *(avant windows Server version 1803)*

## <a name="role-services-not-in-server-core"></a>Services de rôle absents de Server Core
Notez que certains services de rôle Bureau à distance sont inclus dans Server Core (Service Broker de connexion, licence, hôte de virtualisation), mais d’autres ne le sont pas (passerelle, hôte de session Bureau à distance, Accès web).

- Services d’impression et de numérisation de document \ Serveur de numérisation distribuée (**impression-analyse-serveur**)
- Services d’impression et de numérisation de document \ impression Internet (**Print-Internet**)
- Services Bureau à distance \ Bureau à distance passerelle (**RDS-passerelle**)
- Services Bureau à distance \ Bureau à distance hôte de session (**RDS-RD-Server**)
- Services Bureau à distance \ Bureau à distance Accès web (**RDS-accès Web**)
- Serveur Web de service de rôle (IIS) \ outils d’administration \ console de gestion IIS (**Web-Mgmt-console**)
- Serveur Web de service de rôle (IIS) \ outils de gestion \ compatibilité avec la gestion IIS 6 \ console de gestion IIS 6 (**Web-Lgcy-Mgmt-console**)
- Services de déploiement Windows \ serveur de déploiement (**WDS-déploiement**)
- Services de déploiement Windows \ serveur de transport (**WDS-transport**) *(avant windows Server version 1803)*

## <a name="features-not-in-server-core"></a>Fonctionnalités non disponibles dans Server Core
- Service de transfert intelligent en arrière-plan (BITS) \ extension de serveur IIS (**bits-IIS-Ext**)
- Déverrouillage réseau BitLocker (**BitLocker-NetworkUnlock**)
- Lecture directe (**direct**)
- Client d’impression Internet (**Internet-Print-client**)
- Moniteur de port LPR (**LPR-Port-Monitor**)
- Message Queuing \ services Message Queuing \ prise en charge de la multidiffusion (**MSMQ-multidiffusion**)
- Kit d’administration du gestionnaire des connexions (**CMAK**) RAS
- Assistance à distance (**assistance à distance**)
- Outils d’administration de serveur distant \ outils d’administration de fonctionnalités \ outils de serveur SMTP (**RSAT-SMTP**)
- Outils d’administration de serveur distant \ outils d’administration de fonctionnalités \ Chiffrement de lecteur BitLocker utilitaires d’administration \ Chiffrement de lecteur BitLocker outils (**RSAT-Feature-Tools-BitLocker-RemoteAdminTool**)
- Outils d’administration de serveur distant \ outils d’administration de fonctionnalités \ outils d’extension de serveur BITS (**RSAT-bits-Server**)
- Outils d’administration de serveur distant \ outils d’administration de fonctionnalités \ outils d’équilibrage de la charge réseau (**RSAT-NLB**)
- Outils d’administration de serveur distant \ outils d’administration de fonctionnalités \ outils SNMP (**RSAT-SNMP**)
- Outils d’administration de serveur distant \ outils d’administration de fonctionnalités \ outils de serveur WINS (**RSAT-WINS**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ outils de gestion Hyper-V \ outils de gestion de l’interface graphique utilisateur Hyper-V (**Hyper-v-outils**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ Services Bureau à distance Tools \ Bureau à distance outils de passerelle (**RSAT-RDS-Tools**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ Services Bureau à distance Tools \ Bureau à distance outils de passerelle (**RSAT-RDS-Gateway**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ Services Bureau à distance outils \ Bureau à distance outils de diagnostic des licences (**RSAT-RDS-Licensing-diagnostic-UI**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ Services Bureau à distance Tools \ Bureau à distance outils de licence (**RDS-licence-UI**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ Windows Server Update Services Tools \ console de gestion de l’interface utilisateur (**updateservices-UI**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ Active Directory les outils des services de certificats (**RSAT-ADCS**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ Active Directory outils des services de certificats \ outils de gestion de l’autorité de certification (**RSAT-ADCS-Mgmt**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ Active Directory outils des services de certificats \ outils de répondeur en ligne (**RSAT-Online-répondeur**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ services AD RMS (Active Directory Rights Management Services) Tools (**RSAT-ADRMS**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ outils de serveur de télécopie (**RSAT-Fax**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ outils de services de fichiers (**RSAT-file-services**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ outils de gestion de fichiers \ outils de gestion DFS (**RSAT-DFS-Mgmt-con**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ outils de services de fichiers \ outils de Gestionnaire des ressources de serveur de fichiers (**RSAT-FSRM-Mgmt**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ fichiers outils \ services pour les outils de gestion du système de fichiers réseau (**RSAT-NFS-admin**)
- Outils d’administration de serveur distant \ outils d’administration de rôle \ outils de stratégie et d’accès réseau (**RSAT-NPAs**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ Services d’impression et de numérisation de document Tools (**RSAT-Print-Services**)
- Outils d’administration de rôles \ outils d’administration de l’Outils d’administration de serveur distant \ (**RSAT-va-Tools**)
- Outils d’administration de serveur distant \ outils d’administration de rôles \ outils des services de déploiement Windows (**WDS-AdminPack**)
- Services TCP/IP simples (**simple-tcpip**)
- Serveur SMTP (**serveur SMTP**)
- Client TFTP (**TFTP-client**)
- Redirecteur WebDAV (**WebDAV-redirecteur**)
- Windows Biometric Framework (**biométrique-infrastructure**)
- Fonctionnalités de Windows Defender \ GUI pour Windows Defender (**Windows-Defender-GUI**)
- Windows Identity Foundation 3,5 (**Windows-Identity-Foundation**)
- Windows PowerShell \ Windows PowerShell ISE (**PowerShell-ISE**)
- Search Service Windows (**service de recherche**)
- Windows TIFF IFilter (**Windows-TIFF-IFilter**)
- Service de réseau local sans fil (**réseau sans fil**)
- Visionneuse XPS (**XPS-Viewer**)
