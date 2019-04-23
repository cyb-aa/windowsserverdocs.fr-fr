---
title: Rôles, Services de rôle et fonctionnalités pas dans la version de conteneurs - Windows Server, Server Core 1803
description: En savoir plus sur les rôles et fonctionnalités que nous avons supprimé de l’image de conteneur de Server Core pour Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873780"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Rôles, Services de rôle et fonctionnalités pas dans la version de conteneurs - Windows Server, Server Core 1803

> S’applique à : Windows Server, version 1803

Dans Windows Server, version 1803, nous avons [réduit la taille globale de l’image de conteneur de Server Core à **Go 1,58**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). Le nous l’avons fait cela consiste à optimiser l’architecture et la suppression des choses vous n’avez pas besoin dans un [conteneur de Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Certains ont été choses qui n’a pas fonctionné dans des conteneurs, certaines ont des rôles et fonctionnalités, qu'aucune autre à l’aide. 

> [!IMPORTANT]
> Nous avons supprimé à partir de Server Core **conteneur** de l’image, pas [Server Core lui-même](server-core-roles-and-services.md). 

Voici la liste complète des fonctionnalités et rôles supprimés de l’image de conteneur de Server Core :

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Utilitaires de BitLocker
<br>BitLocker
<br>BITS
<br>BITSExtensions-Upload
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS-Infrastructure
<br>Conteneurs
<br>CoreFileServer
<br>DataCenterBridging-LLDP-Tools
<br>DataCenterBridging
<br>La déduplication cœur
<br>DeviceHealthAttestationService
<br>DFSN-serveur
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>QoS d’e/s disque
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster-PowerShell
<br>Services de fichiers
<br>FileServerVSSAgent
<br>Infrastructure de FRS
<br>FSRM-Infrastructure-Services
<br>FSRM-Infrastructure
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-Package
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer-PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Licences
<br>LightweightServer
<br>Microsoft-Hyper-V-Management-Clients
<br>Microsoft-Hyper-V-hors connexion
<br>Microsoft-Hyper-V-en ligne
<br>Microsoft Hyper-V
<br>Microsoft-Windows-FCI-Client-Package
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-Subsystem-Linux
<br>Infrastructure de MSRDC
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>L’impression-Client-Gui
<br>Printing-LPDPrintService
<br>Fonctionnalités d’impression-Server-Foundation
<br>Rôle de serveur d’impression
<br>QWAVE
<br>RasRoutingProtocols
<br>Services de bureau à distance
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-Role
<br>RightsManagementServices
<br>Fédération de RMS
<br>Interface utilisateur SBMgr
<br>ServerCore-Drivers-General-WOW64
<br>ServerCore-Drivers-General
<br>ServerForNFS-Infrastructure
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
<br>Storage-Replica-AdminPack
<br>Réplica de stockage
<br>Module de plateforme sécurisée PSH-applets de commande-
<br>UpdateServices-Database
<br>UpdateServices-Services
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Web-Application-Proxy
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-Server
<br>Package de produit de WSS

</div>