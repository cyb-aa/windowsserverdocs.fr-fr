---
title: Rôles, services de rôle et fonctionnalités inclus dans Windows Server-Server Core
description: Quels rôles et fonctionnalités sont inclus dans l’option d’installation minimale de Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 2f6aed56083bd606ae2ec06b72152ef4a0461420
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476509"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Rôles, services de rôle et fonctionnalités inclus dans Windows Server-Server Core

> S’applique à : Windows Server 2019, Windows Server 2016 et Windows Server (canal semi-annuel)

Nous parlons généralement de [ce qui *n’est pas* dans Server Core](server-core-removed-roles.md) : nous allons maintenant essayer une approche différente et vous indiquer ce qui est *inclus* et si un élément est *installé par défaut*. Les rôles, services de rôle et fonctionnalités suivants se trouvent *dans* l’option d’installation Server Core de Windows Server. Utilisez ces informations pour déterminer si l’option Server Core fonctionne pour votre environnement. Étant donné qu’il s’agit d’une grande liste, pensez à rechercher le rôle ou la fonctionnalité spécifique qui vous intéresse. Si cette recherche ne retourne pas ce que vous recherchez, elle n’est pas incluse dans Server Core.

Par exemple, si vous recherchez «Bureau à distance hôte de session», vous ne le trouverez pas sur cette page. Cela est dû au fait que l’hôte de session Bureau à distance n’est pas inclus dans l’image Server Core.

N’oubliez pas que vous pouvez [toujours regarder](server-core-removed-roles.md) ce qui *n’est pas* inclus. Il s’agit simplement d’une autre façon d’examiner les choses.

## <a name="roles-included-in-server-core"></a>Rôles inclus dans Server Core
L’option d’installation Server Core comprend les rôles de serveur suivants.

| Rôle                                            | Name                           | Installé par défaut? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Services de certificats Active Directory           | Certificat AD                 | N                     |
| Services de domaine Active Directory                | Services de domaine Active Directory             | N                     |
| Services de fédération Active Directory (AD FS)            | ADFS-Fédération                | N                     |
| Services AD LDS (Active Directory Lightweight Directory Services) | ADLDS                          | N                     |
| Services AD RMS (Active Directory Rights Management Services)     | ADRMS                          | N                     |
| Attestation d’intégrité de l’appareil                       | DeviceHealthAttestationService | N                     |
| Serveur DHCP                                     | DHCP                           | N                     |
| Serveur DNS                                      | DNS                            | N                     |
| Services de fichiers et de stockage                       | FileAndStorage-services        | Y                     |
| Service Guardian hôte                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Services d'impression et de numérisation de document                     | Services d’impression                 | N                     |
| Accès à distance                                   | RemoteAccess                   | N                     |
| Services Bureau à distance                         | Services Bureau à distance        | N                     |
| Services d'activation en volume                      | VolumeActivation               | N                     |
| Serveur Web IIS                                  | Serveur Web                     | N                     |
| Expérience Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Services de rôle inclus dans Server Core
L’option d’installation Server Core comprend les services de rôle suivants.

| Rôle                                  | Service de rôle                                                   | Nom                    | Installé par défaut? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Services de certificats Active Directory | Autorité de certification                                        | ADCS-CERT-Authority     | N                     |
|                                       | Service Web Stratégie d’inscription de certificats                      | ADCS-inscrire-Web-pol     | N                     |
|                                       | Service Web Inscription de certificats                             | ADCS-inscrire-Web-SVC     | N                     |
|                                       | Inscription de l’autorité de certification via le Web                         | ADCS-inscription sur le Web     | N                     |
|                                       | Service d’inscription de périphériques réseau                              | ADCS-inscription d’appareils  | N                     |
|                                       | Répondeur en ligne                                               | ADCS-Online-CERT        | N                     |
| Active Directory Rights Management    | Serveur AD RMS (Active Directory Rights Management Server)                      | ADRMS-serveur            | N                     |
|                                       | Prise en charge de la fédération des identités                                    | ADRMS-identité          | N                     |
| Services de fichiers et de stockage             | Services de fichiers et iSCSI                                        | Services de fichiers           | N                     |
|                                       | Serveur de fichiers                                                    | FS-FileServer           | N                     |
|                                       | BranchCache pour fichiers réseau                                  | FS-BranchCache          | N                     |
|                                       | Déduplication des données                                             | FS-déduplication des données   | N                     |
|                                       | Espaces de noms DFS                                                 | FS-DFS-espace de noms        | N                     |
|                                       | Réplication DFS                                                | FS-réplication DFS      | N                     |
|                                       | Outils de gestion de ressources pour serveur de fichiers                                   | FS-Resource-Manager     | N                     |
|                                       | Service Agent VSS du serveur de fichiers                                  | FS-VSS-agent            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget-serveur      | N                     |
|                                       | Fournisseur de stockage cible iSCSI (fournisseurs de matériel VDS et VSS) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Serveur pour NFS                                                 | FS-NFS-service          | N                     |
|                                       | Dossiers de travail                                                   | FS-SyncShareService     | N                     |
|                                       | Services de stockage                                               | Services de stockage        | Y                     |
| Services d'impression et de numérisation de document           | Serveur d’impression                                                   | Serveur d’impression            | N                     |
|                                       | Service LDP                                                    | Print-LPD-service       | N                     |
| Accès à distance                         | DirectAccess et VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Routage                                                        | Routage                 | N                     |
|                                       | Proxy d'application web                                          | Web-application-proxy   | N                     |
| Services Bureau à distance               | Service Broker pour les connexions Bureau à distance                               | RDS-Connection-Broker   | N                     |
|                                       | Gestionnaire de licences des services Bureau à distance                                       | Gestion des licences des services Bureau à distance           | N                     |
|                                       | Hôte de virtualisation des services Bureau à distance                             | RDS-virtualisation      | N                     |
| Serveur Web (IIS)                      | Serveur Web                                                     | Web-webserver           | N                     |
|                                       | Fonctionnalités HTTP courantes                                           | Web-commun-http         | N                     |
|                                       | Document par défaut                                               | Web-Default-doc         | N                     |
|                                       | Exploration de répertoire                                             | Web-Rép-navigation        | N                     |
|                                       | Erreurs HTTP                                                    | Web-http-Erreurs         | N                     |
|                                       | Contenu statique                                                 | Web-statique-contenu      | N                     |
|                                       | Redirection HTTP                                               | Web-http-Redirect       | N                     |
|                                       | Publication WebDAV                                              | Web-DAV-publication      | N                     |
|                                       | Intégrité et diagnostic                                         | Web-intégrité              | N                     |
|                                       | Journalisation HTTP                                                   | Web-http-journalisation        | N                     |
|                                       | Journalisation personnalisée                                                 | Web-journalisation personnalisée      | N                     |
|                                       | Outils de journalisation                                                  | Web-bibliothèques de journaux       | N                     |
|                                       | Journalisation ODBC                                                   | Web-ODBC-Logging        | N                     |
|                                       | Observateur de demandes                                              | Web-requête-moniteur     | N                     |
|                                       | Traçage                                                        | Web-traçage http        | N                     |
|                                       | Performances                                                    | Web-performances         | N                     |
|                                       | Compression de contenu statique                                     | Web-stat-compression    | N                     |
|                                       | Compression de contenu dynamique                                    | Web-dyn-compression     | N                     |
|                                       | Sécurité                                                       | Web-sécurité            | N                     |
|                                       | Request Filtering                                              | Filtrage Web           | N                     |
|                                       | Authentification de base                                           | Web-de base-auth          | N                     |
|                                       | Prise en charge centralisée des certificats SSL                            | CertProvider Web        | N                     |
|                                       | Authentification par mappage de certificat client                      | Web-client-auth         | N                     |
|                                       | Authentification Digest                                          | Web-Digest-auth         | N                     |
|                                       | Authentification par mappage de certificat client IIS                  | Web-CERT-auth           | N                     |
|                                       | Restrictions IP et de domaine                                     | Web-IP-sécurité         | N                     |
|                                       | Autorisation d’URL                                              | Web-URL-auth            | N                     |
|                                       | Authentification Windows                                         | Web-authentification Windows        | N                     |
|                                       | Développement d’applications                                        | Web-App-dev             | N                     |
|                                       | Extensibilité .NET 3,5                                         | Web-net-ext             | N                     |
|                                       | Extensibilité .NET 4,6                                         | Web-net-Ext45           | N                     |
|                                       | Initialisation d’application                                     | AppInit Web             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3,5                                                    | Web-ASP-NET             | N                     |
|                                       | ASP.NET 4,6                                                    | Web-ASP-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensions ISAPI                                               | Web-ISAPI-ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-filtre        | N                     |
|                                       | Fichiers Include côté serveur                                           | Web-comprend            | N                     |
|                                       | Protocole WebSocket                                             | Web-WebSocket          | N                     |
|                                       | Serveur FTP                                                     | Web-FTP-Server          | N                     |
|                                       | Service FTP                                                    | Web-FTP-service         | N                     |
|                                       | Extensibilité FTP                                              | Web-FTP-ext             | N                     |
|                                       | Outils de gestion                                               | Web-Mgmt-outils          | N                     |
|                                       | Compatibilité avec la gestion IIS 6                                 | Gestion Web-compatibilité         | N                     |
|                                       | Compatibilité avec la métabase IIS 6                                   | Web-métabase            | N                     |
|                                       | Outils de script IIS 6                                          | Web-Lgcy-scripts      | N                     |
|                                       | Compatibilité avec le service WMI IIS 6                                        | Web-WMI                 | N                     |
|                                       | Scripts et outils de gestion IIS                               | Script Web-outils     | N                     |
|                                       | Service de gestion                                             | Web-Mgmt-service        | N                     |
| Windows Server Update Services        | Connectivité WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Services WSUS                                                  | UpdateServices-services | N                     |
|                                       | Connectivité SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Fonctionnalités incluses dans Server Core
L’option d’installation Server Core comprend les fonctionnalités suivantes.

| Fonctionnalité                                                | Name                               | Installé par défaut? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Fonctionnalités de .NET Framework 3,5                            | .NET-Framework-fonctionnalités             | N                     |
| .NET Framework 3,5 (comprend .NET 2,0 et 3,0)       | NET-Framework-Core                 | supprimé             |
| Activation HTTP                                        | NET-HTTP-activation                | N                     |
| Activation non-HTTP                                    | NET-non-HTTP-activable                 | N                     |
| Fonctionnalités de .NET Framework 4,6                            | NET-Framework-45-fonctionnalités          | Y                     |
| .NET Framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4,6                                            | NET-Framework-45-ASPNET            | N                     |
| Services WCF                                           | NET-WCF-Services45                 | Y                     |
| Activation HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Activation d’Message Queuing (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activation du canal nommé                                  | NET-WCF-pipe-Activation45          | N                     |
| Activation TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Partage de port TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Service de transfert intelligent en arrière-plan (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-Server                | N                     |
| Chiffrement de lecteur BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client pour NFS                                         | NFS-client                         | N                     |
| Containers                                             | Containers                         | N                     |
| Data Center Bridging                                   | Data-Center-pontage               | N                     |
| Stockage étendu                                       | EnhancedStorage                    | N                     |
| Clustering de basculement                                    | Basculement-clustering                | N                     |
| Gestion des stratégies de groupe                                | Console GPMC                               | N                     |
| Qualité de service E/S                                 | E-QoS                         | N                     |
| IIS Hostable Web Core                                  | WHC Web                            | N                     |
| Serveur de gestion des adresses IP (IPAM)                    | IPAM                               | N                     |
| Service Serveur iSNS                                    | ISNS                               | N                     |
| Extension ISS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Serveur-support-base            | N                     |
| Message Queuing                                        | MSMQ                               | N                     |
| Services Message Queuing                               | Services MSMQ                      | N                     |
| Serveur de Message Queuing                                 | Serveur MSMQ                        | N                     |
| Intégration du service d’annuaire                          | Répertoire MSMQ                     | N                     |
| Prise en charge HTTP                                           | Prise en charge de MSMQ-HTTP                  | N                     |
| Déclencheurs Message Queuing                               | Déclencheurs MSMQ                      | N                     |
| Service de routage                                        | Routage MSMQ                       | N                     |
| Message Queuing DCOM Proxy                             | MSMQ-DCOM                          | N                     |
| MPIO (Multipath I/O)                                          | Chemins d’accès multiples-e/s                       | N                     |
| MultiPoint Connector                                   | MultiPoint-connecteur               | N                     |
| Services du connecteur MultiPoint                          | MultiPoint-Connector-services      | N                     |
| Gestionnaire MultiPoint et tableau de bord MultiPoint            | MultiPoint-outils                   | N                     |
| Équilibrage de la charge réseau                                 | NLB                                | N                     |
| Protocole PNRP (Peer Name Resolution Protocol)                          | PROTOCOLE                               | N                     |
| Expérience audio-vidéo haute qualité Windows                 | qWave                              | N                     |
| Compression différentielle à distance                        | 6\.1                                | N                     |
| Outils d’administration de serveur distant                     | Outils d'administration de serveur distant                               | N                     |
| Outils d’administration de fonctionnalités                           | RSAT-Feature-Tools                 | N                     |
| Utilitaires d’administration de Chiffrement de lecteur BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Outils LLDP outils datacenterbridging                          | RSAT-outils datacenterbridging-LLDP-Tools | N                     |
| Outils de clustering de basculement                              | RSAT-clustering                    | N                     |
| Module de cluster de basculement pour Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Serveur Automation de cluster de basculement                     | RSAT-clustering-AutomationServer»   | N                     |
| Interface de commande de cluster de basculement                     | RSAT-clustering-Cmdinterface»       | N                     |
| Client gestion des adresses IP (IPAM)                    | IPAM-client-fonctionnalité                | N                     |
| Outils de machines virtuelles protégées                                      | RSAT-blindée-machine virtuelle-outils             | N                     |
| Module de réplica de stockage pour Windows PowerShell          | RSAT-stockage-réplica               | N                     |
| Outils d’administration de rôles                              | RSAT-Role-Tools                    | N                     |
| Outils AD DS et AD LDS                                 | RSAT-AD-outils                      | N                     |
| Module Active Directory pour Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Outils AD DS                                            | RSAT-ADDS                          | N                     |
| Centre d’administration Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Composants logiciels enfichables et outils en ligne de commande AD DS                  | RSAT-ADDS-Tools                    | N                     |
| Composants logiciels enfichables et outils en ligne de commande AD LDS                 | RSAT-ADLDS                         | N                     |
| Outils de gestion Hyper-V                               | RSAT-Hyper-V-outils                 | N                     |
| Module Hyper-V pour Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Outils de Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| API et applets de commande PowerShell                             | UpdateServices-API                 | N                     |
| Outils de serveur DHCP                                      | RSAT-DHCP                          | N                     |
| Outils du serveur DNS                                       | RSAT-serveur DNS                    | N                     |
| Outils de gestion de l’accès à distance                         | RSAT-RemoteAccess                  | N                     |
| Module d’accès à distance pour Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| Proxy RPC sur HTTP                                    | RPC sur HTTP-Proxy                | N                     |
| Collection des événements de configuration et de démarrage                        | Setup-and-Boot-Event-collection    | N                     |
| Services TCP/IP simples                                 | Simple-TCPIP                       | N                     |
| Support de partage de fichiers SMB 1.0/CIFS                      | FS-SMB1                            | Y                     |
| Limite de bande passante SMB                                    | FS-SMBBW                           | N                     |
| Service SNMP                                           | SNMP-service                       | N                     |
| Fournisseur WMI SNMP                                      | SNMP-WMI-fournisseur                  | N                     |
| Client Telnet                                          | Telnet-client                      | N                     |
| Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure               | FabricShieldedTools                | N                     |
| Fonctionnalités de Windows Defender                              | Windows-Defender-fonctionnalités          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Base de données interne Windows                              | Windows-interne-base de données          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5,1                                 | PowerShell                         | Y                     |
| Moteur Windows PowerShell 2,0                          | PowerShell-v2                      | supprimé             |
| Service de configuration d’état souhaité Windows PowerShell | DSC-service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Service d'activation des processus Windows                     | AVAIT                                | N                     |
| Modèle de processus                                          | WAS-Process-Model                  | N                     |
| Environnement .NET 3,5                                   | WAS-NET-environnement                | N                     |
| API de configuration                                     | API WAS-config                    | N                     |
| Sauvegarde Windows Server                                  | Windows-Server-sauvegarde              | N                     |
| Outils de migration de Windows Server                         | Migration                          | N                     |
| Gestion du stockage Windows basé sur des normes             | WindowsStorageManagementService    | N                     |
| Extension WinRM IIS                                    | WinRM-IIS-Ext                      | N                     |
| Serveur WINS                                            | WINS                               | N                     |
| Prise en charge WoW64                                          | WoW64-support                      | Y                     |
|                                                        |                                    |                       |
