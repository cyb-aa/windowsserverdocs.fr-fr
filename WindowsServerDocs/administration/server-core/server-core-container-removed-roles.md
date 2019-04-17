---
title: Rôles, Services de rôle et fonctionnalités pas dans la version de Windows Server, les conteneurs - Server Core 1803
description: Obtenir des informations sur les rôles et les fonctionnalités que nous avons supprimé à partir de l’image de conteneur Server Core de Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1859900"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Rôles, Services de rôle et fonctionnalités pas dans la version de Windows Server, les conteneurs - Server Core 1803

> S’applique à: Windows Server, version 1803

Dans Windows Server, version 1803, nous avons [réduit la taille globale de l’image de conteneur Server Core **1,58**Go](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). La façon dont nous avons fait est en optimisant l’architecture et de suppression des choses que vous n’avez pas besoin d’un [conteneur Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Certains éléments qui n’a pas fonctionné dans des conteneurs, certaines ont des rôles et fonctionnalités que n’a été utilisé. 

> [!IMPORTANT]
> Nous avons supprimé à partir de l’image de **conteneur** Server Core, pas [Server Core proprement dit](server-core-roles-and-services.md). 

Voici la liste complète des fonctionnalités et de rôles supprimés de l’image de conteneur Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>BitLocker-utilitaires
<br>BitLocker
<br>BITS
<br>BITSExtensions-téléchargement
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>Infrastructure ClientForNFS
<br>Conteneurs
<br>CoreFileServer
<br>DataCenterBridging-LLDP-outils
<br>DataCenterBridging
<br>Cœur Dedup
<br>DeviceHealthAttestationService
<br>Serveur DFS
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>DiskIo-QoS
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster PowerShell
<br>Services de fichiers
<br>FileServerVSSAgent
<br>Infrastructure FRS
<br>Services d’Infrastructure FSRM
<br>Infrastructure FSRM
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>Package HostGuardianService
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Gestion des licences
<br>LightweightServer
<br>Microsoft Hyper-V-gestion-les Clients
<br>Microsoft-Hyper-V-hors connexion
<br>Microsoft Hyper-V-en ligne
<br>Microsoft Hyper-V
<br>Microsoft-Windows-FCI-Client-Package
<br>Microsoft-Windows-SystemRoot%\System32\GroupPolicy-ServerAdminTools-mise à jour
<br>Microsoft-Windows-sous-système-Linux
<br>Infrastructure MSRDC
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Impression-Client-Gui
<br>Impression-LPDPrintService
<br>Fonctionnalités d’impression-Server-Foundation
<br>Rôle de serveur d’impression
<br>QWAVE
<br>RasRoutingProtocols
<br>Services de bureau à distance
<br>Accès à distance
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-Role
<br>RightsManagementServices
<br>RMS-Federation
<br>SBMgr-interface utilisateur
<br>ServerCore-pilotes-général-WOW64
<br>ServerCore pilotes-général
<br>Infrastructure ServerForNFS
<br>Outils du fonctionnalité RSAT ServerManager principaux
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-serveur
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>Stockage-réplica-AdminPack
<br>Réplica de stockage
<br>Cmdlets de PSH TPM
<br>Base de données UpdateServices
<br>UpdateServices-Services
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Proxy d’Application Web
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-serveur
<br>Package du produit WSS

</div>