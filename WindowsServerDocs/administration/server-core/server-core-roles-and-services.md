---
title: Rôles, Services de rôle et fonctionnalités incluses dans Windows Server - Server Core
description: Les rôles et fonctionnalités sont incluses dans l’option d’installation Server Core de Windows Server ?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859290"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Rôles, Services de rôle et fonctionnalités incluses dans Windows Server - Server Core

> S’applique à : Windows Server (canal semi-annuel) et Windows Server 2016

Nous parlons généralement [ce que de *pas* dans Server Core](server-core-removed-roles.md) - maintenant nous allons tester une autre approche vous dire ce que de *inclus* et si quelque chose est *installé par défaut*. Les rôles suivants, les services de rôle et les fonctionnalités sont *dans* l’option d’installation Server Core de Windows Server. Utilisez ces informations pour aider à déterminer si l’option Server Core fonctionne pour votre environnement. Comme il s’agit d’une grande liste, envisagez de recherche du rôle spécifique ou une fonctionnalité qui vous intéresse - si cette recherche ne retourne pas ce que vous recherchez, il n’est pas inclus dans Server Core.

Par exemple, si vous recherchez « Remote Desktop Session Host » - vous ne le trouverez sur cette page. C’est parce que l’hôte de Session Bureau à distance n’est pas inclus dans l’image de Server Core.

N’oubliez pas que vous pouvez [ne cherchent toujours](server-core-removed-roles.md) à quel de *pas* inclus. Il s’agit simplement un moyen différent d’examiner les choses.

## <a name="roles-included-in-server-core"></a>Rôles inclus dans Server Core
L’option d’installation Server Core comprend les rôles de serveur suivants.

| Rôle                                            | Nom                           | Installé par défaut ? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Services de certificats Active Directory           | Certificat AD                 | N                     |
| Services de domaine Active Directory                | Services de domaine AD             | N                     |
| Services de fédération Active Directory (AD FS)            | Fédération-ADFS                | N                     |
| Services AD LDS (Active Directory Lightweight Directory Services) | ADLDS                          | N                     |
| Services AD RMS (Active Directory Rights Management Services)     | ADRMS                          | N                     |
| Attestation d’intégrité de l’appareil                       | DeviceHealthAttestationService | N                     |
| Serveur DHCP                                     | DHCP                           | N                     |
| Serveur DNS                                      | DNS                            | N                     |
| Services de fichiers et de stockage                       | FileAndStorage-Services        | Y                     |
| Service Guardian hôte                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Services d'impression et de numérisation de document                     | Services d’impression                 | N                     |
| Accès à distance                                   | RemoteAccess                   | N                     |
| Services Bureau à distance                         | Services de bureau à distance        | N                     |
| Services d'activation en volume                      | VolumeActivation               | N                     |
| Serveur Web IIS                                  | Serveur Web                     | N                     |
| Expérience Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Services de rôle inclus dans Server Core
L’option d’installation Server Core inclut les services de rôle suivants.

| Rôle                                  | Service de rôle                                                   | Nom                    | Installé par défaut ? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Services de certificats Active Directory | Autorité de certification                                        | Autorité de certificat AD CS     | N                     |
|                                       | Service Web Stratégie d’inscription de certificats                      | ADCS-Enroll-Web-Pol     | N                     |
|                                       | Service Web Inscription de certificats                             | ADCS-Enroll-Web-Svc     | N                     |
|                                       | Inscription de l’autorité de certification via le Web                         | AD CS-Web-Inscription     | N                     |
|                                       | Service d’inscription de périphériques réseau                              | Inscription d’appareil AD CS  | N                     |
|                                       | Répondeur en ligne                                               | ADCS-Online-Cert        | N                     |
| Active Directory Rights Management    | Serveur AD RMS (Active Directory Rights Management Server)                      | Serveur AD RMS            | N                     |
|                                       | Prise en charge de la fédération des identités                                    | ADRMS-Identity          | N                     |
| Services de fichiers et de stockage             | Services de fichiers et iSCSI                                        | Services de fichiers           | N                     |
|                                       | Serveur de fichiers                                                    | FS-FileServer           | N                     |
|                                       | BranchCache pour fichiers réseau                                  | BranchCache-FS          | N                     |
|                                       | Déduplication des données                                             | Déduplication des données FS   | N                     |
|                                       | Espaces de noms DFS                                                 | FS-DFS-Namespace        | N                     |
|                                       | Réplication DFS                                                | FS-DFS-Replication      | N                     |
|                                       | Outils de gestion de ressources pour serveur de fichiers                                   | FS-Resource-Manager     | N                     |
|                                       | Service Agent VSS du serveur de fichiers                                  | FS-VSS-Agent            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget-Server      | N                     |
|                                       | iSCSI (fournisseurs de matériel VDS et VSS) du fournisseur de stockage cible | iSCSITarget-VSS-VDS     | N                     |
|                                       | Serveur pour NFS                                                 | Service FS-NFS          | N                     |
|                                       | Dossiers de travail                                                   | FS-SyncShareService     | N                     |
|                                       | Services de stockage                                               | Services de stockage        | Y                     |
| Services d'impression et de numérisation de document           | Serveur d’impression                                                   | Serveur d’impression            | N                     |
|                                       | Service LDP                                                    | Print-LPD-Service       | N                     |
| Accès à distance                         | DirectAccess et VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Routage                                                        | Routage                 | N                     |
|                                       | Proxy d'application web                                          | Web-Application-Proxy   | N                     |
| Services Bureau à distance               | Service Broker pour les connexions Bureau à distance                               | Agent de connexion services Bureau à distance   | N                     |
|                                       | Gestionnaire de licences bureau à distance                                       | Licences des services Bureau à distance           | N                     |
|                                       | Hôte de virtualisation des services Bureau à distance                             | Virtualisation des services Bureau à distance      | N                     |
| Serveur Web (IIS)                      | Serveur Web                                                     | Web-WebServer           | N                     |
|                                       | Fonctionnalités HTTP courantes                                           | Web-Common-Http         | N                     |
|                                       | Document par défaut                                               | Web-Default-Doc         | N                     |
|                                       | Exploration de répertoire                                             | Web-Dir-Browsing        | N                     |
|                                       | Erreurs HTTP                                                    | Web-Http-Errors         | N                     |
|                                       | Contenu statique                                                 | Web-Static-Content      | N                     |
|                                       | Redirection HTTP                                               | Web-Http-redirection       | N                     |
|                                       | Publication WebDAV                                              | Publication Web-DAV      | N                     |
|                                       | Intégrité et diagnostic                                         | État du Web              | N                     |
|                                       | Journalisation HTTP                                                   | Web-Http-Logging        | N                     |
|                                       | Journalisation personnalisée                                                 | Web-personnalisé-journalisation      | N                     |
|                                       | Outils de journalisation                                                  | Web-Log-Libraries       | N                     |
|                                       | Journal ODBC                                                   | Web-ODBC-Logging        | N                     |
|                                       | Observateur de demandes                                              | Web-Request-Monitor     | N                     |
|                                       | Suivi                                                        | Web-Http-le suivi        | N                     |
|                                       | Performances                                                    | Performances de site Web         | N                     |
|                                       | Compression du contenu statique                                     | Web-Stat-Compression    | N                     |
|                                       | Compression de contenu dynamique                                    | Web-Dyn-Compression     | N                     |
|                                       | Sécurité                                                       | Sécurité Web            | N                     |
|                                       | Request Filtering                                              | Filtrage de contenu Web           | N                     |
|                                       | Authentification de base                                           | Authentification de base de Web          | N                     |
|                                       | Prise en charge des certificats SSL centralisés                            | Web-CertProvider        | N                     |
|                                       | Authentification par mappage de certificat client                      | Web-Client-Auth         | N                     |
|                                       | Authentification Digest                                          | Web-Digest-Auth         | N                     |
|                                       | Authentification par mappage de certificat Client IIS                  | Web-Cert-Auth           | N                     |
|                                       | Restrictions IP et domaine                                     | Web-IP-sécurité         | N                     |
|                                       | Autorisation d’URL                                              | Web-Url-Auth            | N                     |
|                                       | Authentification Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Développement d’applications                                        | Développement d’applications Web             | N                     |
|                                       | Extensibilité .NET 3.5                                         | Web-Net-Ext             | N                     |
|                                       | Extensibilité .NET 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | Initialisation d’application                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensions ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-Filter        | N                     |
|                                       | Fichiers Include côté serveur                                           | Web inclut            | N                     |
|                                       | Protocole WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Serveur FTP                                                     | Serveur Web-Ftp          | N                     |
|                                       | Service FTP                                                    | Web-Ftp-Service         | N                     |
|                                       | Extensibilité FTP                                              | Web-Ftp-Ext             | N                     |
|                                       | Outils de gestion                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilité avec la gestion IIS 6                                 | Web-Mgmt-Compat         | N                     |
|                                       | Compatibilité avec la métabase IIS 6                                   | Métabase du Web            | N                     |
|                                       | Outils de script 6 IIS                                          | Lgcy-scripts Web      | N                     |
|                                       | Compatibilité avec le service WMI IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts et outils de gestion IIS                               | Web--Outils de script     | N                     |
|                                       | Service de gestion                                             | Service Web-Mgmt        | N                     |
| Windows Server Update Services        | Connectivité WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Services WSUS                                                  | UpdateServices-Services | N                     |
|                                       | Connectivité SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Fonctionnalités incluses dans Server Core
L’option d’installation Server Core inclut les fonctionnalités suivantes.

| Fonctionnalité                                                | Nom                               | Installé par défaut ? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Fonctionnalités de .NET framework 3.5                            | NET-Framework-fonctionnalités             | N                     |
| .NET framework 3.5 (inclut .NET 2.0 et 3.0)       | NET-Framework-Core                 | (supprimé)             |
| Activation HTTP                                        | NET-HTTP-Activation                | N                     |
| Activation non-HTTP                                    | NET-Non-HTTP-Activ                 | N                     |
| Fonctionnalités de .NET framework 4.6                            | NET-Framework-45-fonctionnalités          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| Services WCF                                           | NET-WCF-Services45                 | Y                     |
| Activation HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Activation Message Queuing (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activation de canaux nommés                                  | NET-WCF-Pipe-Activation45          | N                     |
| Activation TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Partage de Port TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Service de transfert intelligent en arrière-plan (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS Compact monoserveur                | N                     |
| Chiffrement de lecteur BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client pour NFS                                         | Client NFS                         | N                     |
| Conteneurs                                             | Conteneurs                         | N                     |
| Data Center Bridging                                   | Data-Center-Bridging               | N                     |
| Stockage étendu                                       | EnhancedStorage                    | N                     |
| Clustering de basculement                                    | Le Clustering de basculement                | N                     |
| Gestion des stratégies de groupe                                | Console GPMC                               | N                     |
| Qualité de service E/S                                 | QoS d’e/s disque                         | N                     |
| IIS Hostable Web Core                                  | Web-WHC                            | N                     |
| Serveur de gestion des adresses IP (IPAM)                    | IPAM                               | N                     |
| Service Serveur iSNS                                    | ISNS                               | N                     |
| Extension ISS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Serveur-Media-Foundation            | N                     |
| Message Queuing                                        | MSMQ                               | N                     |
| Services Message Queuing                               | MSMQ-Services                      | N                     |
| Serveur Message Queuing                                 | MSMQ-Server                        | N                     |
| Intégration du service d’annuaire                          | Répertoire de MSMQ                     | N                     |
| Prise en charge HTTP                                           | MSMQ-HTTP-Support                  | N                     |
| Déclencheurs Message Queuing                               | MSMQ-Triggers                      | N                     |
| Service de routage                                        | MSMQ-Routing                       | N                     |
| Message Queuing DCOM Proxy                             | MSMQ-DCOM                          | N                     |
| MPIO (Multipath I/O)                                          | Multipath i/o-e/s                       | N                     |
| MultiPoint Connector                                   | MultiPoint-Connector               | N                     |
| Connecteur multiPoint Services                          | MultiPoint-Connector-Services      | N                     |
| Le gestionnaire multiPoint et tableau de bord MultiPoint            | Outils de multiPoint                   | N                     |
| Équilibrage de la charge réseau                                 | NLB                                | N                     |
| Protocole PNRP (Peer Name Resolution Protocol)                          | PNRP                               | N                     |
| Expérience audio-vidéo haute qualité Windows                 | qWave                              | N                     |
| Compression différentielle à distance                        | COMPRESSION DIFFÉRENTIELLE À DISTANCE                                | N                     |
| Outils d’administration de serveur distant                     | Outils d'administration de serveur distant                               | N                     |
| Outils d’Administration de fonctionnalités                           | RSAT-Feature-Tools                 | N                     |
| Utilitaires d’Administration de chiffrement de lecteur BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Outils DataCenterBridging LLDP                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Outils de clustering de basculement                              | RSAT-Clustering                    | N                     |
| Module Cluster de basculement pour Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Serveur Automation de Cluster de basculement                     | RSAT-Clustering-AutomationServer   | N                     |
| Interface de commande de Cluster de basculement                     | RSAT-Clustering-CmdInterface       | N                     |
| Client de gestion (IPAM) d’adresse IP                    | IPAM-Client-Feature                | N                     |
| Outils de la machine virtuelle protégée                                      | RSAT--VM-outils protégée             | N                     |
| Module de réplica de stockage pour Windows PowerShell          | RSAT-Storage-Replica               | N                     |
| Outils d’administration de rôles                              | RSAT-Role-Tools                    | N                     |
| Outils AD DS et AD LDS                                 | Outils RSAT-AD                      | N                     |
| Module Active Directory pour Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Outils AD DS                                            | RSAT-ADDS                          | N                     |
| Centre d’administration Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Composants logiciels enfichables et outils en ligne de commande AD DS                  | Ajoute-outils de serveur distant                    | N                     |
| Outils de ligne de commande et les composants logiciel enfichables AD LDS                 | RSAT-ADLDS                         | N                     |
| Outils de gestion Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Module Hyper-V pour Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Outils de Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| Applets de commande PowerShell et API                             | UpdateServices-API                 | N                     |
| Outils du serveur DHCP                                      | RSAT-DHCP                          | N                     |
| Outils du serveur DNS                                       | RSAT-DNS-Server                    | N                     |
| Outils de gestion d’accès à distance                         | RSAT-RemoteAccess                  | N                     |
| Module d’accès à distance pour Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| Proxy RPC sur HTTP                                    | RPC-over-HTTP-Proxy                | N                     |
| Collection des événements de configuration et de démarrage                        | Le programme d’installation-et-démarrage-collecte des événements    | N                     |
| Services TCP/IP simples                                 | Simple-TCPIP                       | N                     |
| Support de partage de fichiers SMB 1.0/CIFS                      | FS-SMB1                            | Y                     |
| Limite de bande passante SMB                                    | FS-SMBBW                           | N                     |
| Service SNMP                                           | SNMP-Service                       | N                     |
| Fournisseur WMI SNMP                                      | Fournisseur de WMI SNMP                  | N                     |
| Client Telnet                                          | Telnet-Client                      | N                     |
| Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure               | FabricShieldedTools                | N                     |
| Fonctionnalités de Windows Defender                              | Fonctionnalités de Windows Defender          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Base de données interne Windows                              | Windows--base de données interne          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Moteur de Windows PowerShell 2.0                          | PowerShell-V2                      | (supprimé)             |
| Windows PowerShell Desired State Configuration de Service | DSC-Service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Service d'activation des processus Windows                     | A ÉTÉ                                | N                     |
| Modèle de processus                                          | Modèle de processus a été                  | N                     |
| Environnement .NET 3.5                                   | ÉTAIT-NET-Environment                | N                     |
| API de configuration                                     | WAS-Config-APIs                    | N                     |
| Sauvegarde Windows Server                                  | Sauvegarde de Windows Server              | N                     |
| Outils de migration de Windows Server                         | Migration                          | N                     |
| Gestion du stockage Windows basé sur des normes             | WindowsStorageManagementService    | N                     |
| Extension WinRM IIS                                    | WinRM-IIS-Ext                      | N                     |
| Serveur WINS                                            | WINS                               | N                     |
| Prise en charge WoW64                                          | WoW64-Support                      | Y                     |
|                                                        |                                    |                       |
