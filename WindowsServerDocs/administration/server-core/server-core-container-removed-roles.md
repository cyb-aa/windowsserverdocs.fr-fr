---
title: Rôles, services de rôle et fonctionnalités qui ne sont pas dans les conteneurs Server Core-Windows Server, version 1803
description: En savoir plus sur les rôles et les fonctionnalités que nous avons supprimés de l’image de conteneur Server Core pour Windows Server.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 41b5a9ac32066f1b2a41de84f66b9be79252c336
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383411"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Rôles, services de rôle et fonctionnalités qui ne sont pas dans les conteneurs Server Core-Windows Server, version 1803

> S’applique à : Windows Server, version 1803

Dans Windows Server, la version 1803, nous avons [réduit la taille globale de l’image de conteneur Server Core à **1,58 Go**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). Pour ce faire, vous pouvez optimiser l’architecture et supprimer les éléments dont vous n’avez pas besoin dans un [conteneur Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Certains étaient des éléments qui ne fonctionnaient pas dans les conteneurs, certains étaient des rôles et des fonctionnalités qui n’utilisaient pas. 

> [!IMPORTANT]
> Nous les avons supprimés de l’image de **conteneur** Server Core, et non de [Server Core](server-core-roles-and-services.md). 

Voici la liste complète des fonctionnalités et des rôles supprimés de l’image de conteneur Server Core :

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Utilitaires BitLocker
<br>BitLocker
<br>BITS
<br>BITSExtensions-charger
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS-infrastructure
<br>Containers
<br>CoreFileServer
<br>Outils datacenterbridging-LLDP-Tools
<br>Outils datacenterbridging
<br>Déduplication-Core
<br>DeviceHealthAttestationService
<br>DFSN-serveur
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>E-QoS
<br>EnhancedStorage
<br>«Failovercluster-AdminPak
<br>« Failovercluster-AutomationServer »
<br>« Failovercluster-Cmdinterface »
<br>«Failovercluster-FullServer
<br>«Failovercluster-PowerShell
<br>Services de fichiers
<br>FileServerVSSAgent
<br>Infrastructure FRS
<br>Services d’infrastructure (FSRM)
<br>Infrastructure FSRM
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>Package HostGuardianService
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer-PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Licences
<br>LightweightServer
<br>Microsoft-Hyper-V-Management-clients
<br>Microsoft-Hyper-V-hors connexion
<br>Microsoft-Hyper-V-en ligne
<br>Microsoft-Hyper-V
<br>Microsoft-Windows-FCI-client-package
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-sous-système-Linux
<br>MSRDC-infrastructure
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Impression-client-GUI
<br>Impression-LPDPrintService
<br>Impression-serveur-base-fonctionnalités
<br>Impression-rôle de serveur
<br>QWAVE
<br>RasRoutingProtocols
<br>Services Bureau à distance
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-rôle
<br>RightsManagementServices
<br>RMS-Fédération
<br>SBMgr-UI
<br>ServerCore-drivers-Généralités-WOW64
<br>ServerCore-drivers-Généralités
<br>ServerForNFS-infrastructure
<br>ServerManager-Core-RSAT-Feature-Tools
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
<br>TPM-PSH-applets de commande
<br>UpdateServices-base de données
<br>UpdateServices-services
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Web-application-proxy
<br>Accès à WebAccès
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-serveur
<br>WSS-produit-package

</div>