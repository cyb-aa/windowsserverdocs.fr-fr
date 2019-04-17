---
title: Rôles, Services de rôle et fonctionnalités incluses dans Windows Server - Server Core
description: Les rôles et les fonctionnalités sont incluses dans l’option d’installation Server Core de Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 439edac95e1427086268cab469ed03b94db630da
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2018
ms.locfileid: "2217739"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Rôles, Services de rôle et fonctionnalités incluses dans Windows Server - Server Core

> S’applique à: Windows Server (Channel annuel séparées) et Windows Server 2016

Généralement parler [que du *pas* dans Server Core](server-core-removed-roles.md) : maintenant nous allons essayer une autre approche et vous indiquer ce qui est *inclus* et si un élément est *installé par défaut*. Les rôles, services de rôle et fonctionnalités suivants se trouvent *dans* l’option d’installation Server Core de Windows Server. Utiliser ces informations pour aider à déterminer si l’option Server Core fonctionne pour votre environnement. Étant donné qu’il s’agit d’une grande liste, envisagez de recherche du rôle spécifique ou une fonctionnalité qui vous intéresse - si que la recherche ne renvoie pas ce que vous recherchez, il n’est pas inclus dans Server Core.

Par exemple, si vous recherchez «Distant hôte de Session bureau» - vous ne le trouverez sur cette page. C’est parce que l’hôte de Session Bureau à distance n’est pas inclus dans l’image du serveur principal.

N’oubliez pas que vous pouvez [toujours ressembler](server-core-removed-roles.md) à ce qui n’est *pas* inclus. Il s’agit juste un autre moyen de voir les choses.

## <a name="roles-included-in-server-core"></a>Rôles inclus dans Server Core
L’option d’installation Server Core comprend les rôles de serveur suivants.

| Rôle                                            | Nom                           | Installés par défaut? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Services de certificats Active Directory           | Certificat AD                 | N                     |
| Services de domaine Active Directory                | Services de domaine Active Directory             | N                     |
| Services AD FS (Active Directory Federation Services)            | Fédération ADFS                | N                     |
| Services AD LDS (Active Directory Lightweight Directory Services) | ADLDS                          | N                     |
| Services AD RMS (Active Directory Rights Management Services)     | ADRMS                          | N                     |
| Attestation d’intégrité des appareils                       | DeviceHealthAttestationService | N                     |
| Serveur DHCP                                     | DHCP                           | N                     |
| Serveur DNS                                      | DNS                            | N                     |
| Services de fichiers et de stockage                       | FileAndStorage-Services        | Y                     |
| Service Guardian hôte                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Services d’impression et de numérisation de document                     | Services d’impression                 | N                     |
| Accès à distance                                   | Accès à distance                   | N                     |
| Services Bureau à distance                         | Services de bureau à distance        | N                     |
| Services d’activation en volume                      | VolumeActivation               | N                     |
| Serveur Web IIS                                  | Serveur Web                     | N                     |
| Expérience WindowsServer Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Services de rôle inclus dans Server Core
L’option d’installation Server Core comprend les services de rôle suivants.

| Rôle                                  | Service de rôle                                                   | Nom                    | Installés par défaut? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Services de certificats Active Directory | Autorité de certification                                        | Autorité de certificat ADC     | N                     |
|                                       | Service Web de l’inscription de stratégie de certificat                      | ADC-inscrire-Web-Pol     | N                     |
|                                       | Service Web d’inscription des certificats                             | ADC-inscrire-Web-Svc     | N                     |
|                                       | Inscription de l’autorité de certification via le Web                         | Inscription ADC-Web     | N                     |
|                                       | Service d’inscription de périphériques réseau                              | Inscription de l’appareil ADC  | N                     |
|                                       | Répondeur en ligne                                               | ADC Online Cert        | N                     |
| Active Directory Rights Management    | Serveur AD RMS (Active Directory Rights Management Server)                      | ADRMS-serveur            | N                     |
|                                       | Prise en charge de la fédération identités                                    | ADRMS-Identity          | N                     |
| Services de fichiers et de stockage             | Services de fichiers et iSCSI                                        | Services de fichiers           | N                     |
|                                       | Serveur de fichiers                                                    | FS-serveur de fichiers           | N                     |
|                                       | BranchCache pour fichiers réseau                                  | FS-BranchCache          | N                     |
|                                       | Déduplication des données                                             | FS déduplication   | N                     |
|                                       | Espaces de nomsDFS                                                 | FS-DFS-Namespace        | N                     |
|                                       | Réplication DFS                                                | Réplication FS-DFS      | N                     |
|                                       | Gestionnaire de ressources du serveur de fichiers                                   | Gestionnaire de ressources FS     | N                     |
|                                       | Service Agent VSS du serveur de fichiers                                  | Agent FS-VSS            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget-serveur      | N                     |
|                                       | Fournisseur de stockage cible (fournisseurs de matériel VDS et VSS) iSCSI | iSCSITarget-VSS-VDS     | N                     |
|                                       | Serveur pour NFS                                                 | Service FS-NFS          | N                     |
|                                       | Dossiers de travail                                                   | FS-SyncShareService     | N                     |
|                                       | Services de stockage                                               | Services de stockage        | Y                     |
| Services d’impression et de numérisation de document           | Serveur d’impression                                                   | Serveur d’impression            | N                     |
|                                       | Service LPD                                                    | Service d’impression LPD       | N                     |
| Accès à distance                         | DirectAccess et VPN (RAS)                                     | DirectAccess VPN        | N                     |
|                                       | Routage des communications                                                        | Routage des communications                 | N                     |
|                                       | Proxy d'application web                                          | Proxy d’Application Web   | N                     |
| Services Bureau à distance               | Broker connexion Bureau à distance                               | Agent de connexion RDS   | N                     |
|                                       | Licences de bureau à distance                                       | Licences de RDS           | N                     |
|                                       | Hôte de virtualisation des services Bureau à distance                             | Virtualisation de RDS      | N                     |
| Serveur Web (IIS)                      | Serveur web                                                     | Web-WebServer           | N                     |
|                                       | Fonctionnalités HTTP courantes                                           | Web-commun-Http         | N                     |
|                                       | Document par défaut                                               | Par défaut-Web-Doc         | N                     |
|                                       | Exploration de répertoire                                             | Dir-navigation sur le Web        | N                     |
|                                       | Erreurs HTTP                                                    | Erreurs de Web Http         | N                     |
|                                       | Contenu statique                                                 | Contenu Web-statique      | N                     |
|                                       | Redirection HTTP                                               | Redirection de Http Web       | N                     |
|                                       | Publication WebDAV                                              | Publication Web-DAV      | N                     |
|                                       | Intégrité et diagnostic                                         | Web-santé              | N                     |
|                                       | Journalisation HTTP                                                   | Journalisation Web-Http        | N                     |
|                                       | Journalisation personnalisé                                                 | Journalisation Web-personnalisé      | N                     |
|                                       | Outils de journalisation                                                  | Bibliothèques de journal Web       | N                     |
|                                       | Journal ODBC                                                   | Journal Web-ODBC        | N                     |
|                                       | Observateur de demandes                                              | Observateur de demandes Web     | N                     |
|                                       | Suivi                                                        | Http-Web-suivi        | N                     |
|                                       | Performances                                                    | Performances Web         | N                     |
|                                       | Compression de contenu statique                                     | Web-Stat-Compression    | N                     |
|                                       | Compression de contenu dynamique                                    | Compression de Dyn Web     | N                     |
|                                       | Sécurité                                                       | Sécurité Web            | N                     |
|                                       | Filtrage des demandes                                              | Filtrage de contenu Web           | N                     |
|                                       | Authentification de base                                           | Web-Basic-Auth          | N                     |
|                                       | Prise en charge des certificats SSL centralisée                            | Web-CertProvider        | N                     |
|                                       | Authentification par mappage de certificat client                      | Authentification de Client Web         | N                     |
|                                       | Authentification Digest                                          | Authentification Web-Digest         | N                     |
|                                       | Authentification de mappage de certificat Client IIS                  | Authentification de certificat Web           | N                     |
|                                       | Adresse IP et les Restrictions de domaine                                     | Sécurité Web-IP         | N                     |
|                                       | Autorisation d’URL                                              | Authentification de l’Url Web            | N                     |
|                                       | Authentification Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Développement d’applications                                        | Développement d’applications Web             | N                     |
|                                       | Extensibilité .NET 3.5                                         | Web-Net-Ext             | N                     |
|                                       | Extensibilité .NET 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | Initialisation de l’application                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp-Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Extensions ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | Filtres ISAPI                                                  | Filtre Web-ISAPI        | N                     |
|                                       | Inclusions côté serveur                                           | Web inclut            | N                     |
|                                       | Protocole WebSocket                                             | Web-WebSocket          | N                     |
|                                       | Serveur FTP                                                     | Serveur Web-Ftp          | N                     |
|                                       | Service FTP                                                    | Service Web-Ftp         | N                     |
|                                       | Extensibilité FTP                                              | Web-Ftp-Ext             | N                     |
|                                       | Outils de gestion                                               | Outils de gestion Web          | N                     |
|                                       | Compatibilité avec la gestion IIS6                                 | Web-Mgmt-descendante         | N                     |
|                                       | Compatibilité avec la métabase 6 IIS                                   | Web-métabase            | N                     |
|                                       | Outils de script 6 IIS                                          | Script-Lgcy Web      | N                     |
|                                       | Compatibilité de 6 WMI IIS                                        | Web-WMI                 | N                     |
|                                       | Gestion des Scripts et outils IIS                               | Outils de script Web     | N                     |
|                                       | Service de gestion                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Connectivité WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Services WSUS                                                  | UpdateServices-Services | N                     |
|                                       | Connectivité SQL Server                                        | Base de données UpdateServices       | N                     |

## <a name="features-included-in-server-core"></a>Fonctionnalités incluses dans Server Core
L’option d’installation Server Core inclut les fonctionnalités suivantes.

| Fonctionnalité                                                | Nom                               | Installés par défaut? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Fonctionnalités de .NET framework 3.5                            | Fonctionnalités de NET Framework             | N                     |
| .NET framework 3.5 (inclut .NET 2.0 et 3.0)       | NET Framework cœur                 | (supprimer)             |
| Activation HTTP                                        | Activation NET-HTTP                | N                     |
| Activation non-HTTP                                    | NET-Non-HTTP-activer                 | N                     |
| Fonctionnalités .NET framework 4.6                            | NET--45-fonctionnalités de l’infrastructure          | Y                     |
| .NET Framework 4.6                                     | NET Framework 45 cœur              | Y                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET.            | N                     |
| Services WCF                                           | NET-WCF-Services45                 | Y                     |
| Activation HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Message Queuing (MSMQ) de l’Activation                      | NET-WCF-MSMQ-Activation45          | N                     |
| Activation des canaux nommés                                  | NET-WCF-canal-Activation45          | N                     |
| Activation TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Partage de Port TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Service de transfert intelligent en arrière-plan (BITS)         | BITS                               | N                     |
| Serveur Compact                                         | Serveur BITS-Compact                | N                     |
| Chiffrement de lecteur BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client pour NFS                                         | NFS-Client                         | N                     |
| Conteneurs                                             | Conteneurs                         | N                     |
| Data Center Bridging                                   | Centre de données pontage               | N                     |
| Stockage étendu                                       | EnhancedStorage                    | N                     |
| Clustering de basculement                                    | Clustering de basculement                | N                     |
| Gestion des stratégies de groupe                                | CONSOLE GPMC                               | N                     |
| Qualité de service E/S                                 | DiskIo-QoS                         | N                     |
| IIS Hostable Web Core                                  | Web-WHC                            | N                     |
| Adresse IP, serveur de gestion (IPAM)                    | IPAM                               | N                     |
| Service Serveur iSNS                                    | ISNS                               | N                     |
| Extension ISS Management OData                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Serveur-Media-Foundation            | N                     |
| Message Queuing                                        | MSMQ                               | N                     |
| Services Message Queuing                               | Services de MSMQ                      | N                     |
| Serveur Message Queuing                                 | MSMQ-Server                        | N                     |
| Intégration du Service d’annuaire                          | MSMQ-Directory                     | N                     |
| Prise en charge HTTP                                           | MSMQ-HTTP-prise en charge                  | N                     |
| Déclencheurs de Message Queuing                               | Déclencheurs de MSMQ                      | N                     |
| Service de routage                                        | Routage MSMQ                       | N                     |
| Message Queuing DCOM Proxy                             | MSMQ-DCOM                          | N                     |
| MPIO (Multipath I/O)                                          | MPIO-e/s                       | N                     |
| MultiPointConnector                                   | Connecteur multiPoint               | N                     |
| Services de connecteur multiPoint                          | Services de connecteur multiPoint      | N                     |
| Gestionnaire de multiPoint et de tableau de bord MultiPoint            | Outils-multiPoint                   | N                     |
| Équilibrage de la charge réseau                                 | ÉQUILIBRAGE DE CHARGE RÉSEAU                                | N                     |
| Protocole PNRP (Peer Name Resolution Protocol)                          | PNRP                               | N                     |
| Expérience audio-vidéo haute qualité Windows                 | qWave                              | N                     |
| Compression différentielle à distance                        | RDC                                | N                     |
| Outils d’administration de serveur distant                     | Outils d'administration de serveur distant                               | N                     |
| Outils d’Administration de fonctionnalité                           | RSAT-Feature-Tools                 | N                     |
| Utilitaires de chiffrement de lecteur BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Outils DataCenterBridging LLDP                          | Outils RSAT-DataCenterBridging-LLDP | N                     |
| Outils du Clustering avec basculement                              | Clustering de RSAT                    | N                     |
| Module de Cluster de basculement pour Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Serveur Automation de Cluster de basculement                     | RSAT Clustering AutomationServer   | N                     |
| Interface de commande de Cluster de basculement                     | RSAT Clustering CmdInterface       | N                     |
| Client de gestion (IPAM) adresse IP                    | Fonctionnalité IPAM-Client                | N                     |
| Outils de machine virtuelle protégée                                      | RSAT-protégé-VM-outils             | N                     |
| Module de réplica de stockage pour Windows PowerShell          | RSAT réplica de stockage               | N                     |
| Outils d’Administration de rôles                              | Outils RSAT-Role                    | N                     |
| AD DS et outils AD LDS                                 | Outils RSAT-AD                      | N                     |
| Module Active Directory pour Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Outils AD DS                                            | AJOUTE DE RSAT                          | N                     |
| Centre d’administration Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| AD DS enfichables et les outils de ligne de commande                  | Outils RSAT ajoute                    | N                     |
| AD LDS enfichables et outils                 | RSAT-ADLDS                         | N                     |
| Outils de gestion Hyper-V                               | RSAT-Hyper-V-outils                 | N                     |
| Module de Hyper-V pour Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Outils de Windows Server Update Services                   | RSAT-UpdateServices                | N                     |
| Applets de commande PowerShell et les API                             | UpdateServices-API                 | N                     |
| Outils du serveur DHCP                                      | RSAT-DHCP                          | N                     |
| Outils du serveur DNS                                       | RSAT-DNS-serveur                    | N                     |
| Outils de gestion de l’accès à distance                         | RSAT-accès distant                  | N                     |
| Module d’accès à distance de Windows PowerShell            | Accès distant RSAT-PowerShell       | N                     |
| Proxy RPC sur HTTP                                    | RPC sur HTTP Proxy                | N                     |
| Collection des événements de configuration et de démarrage                        | Le programme d’installation-et-démarrage--collecte d’événements    | N                     |
| Services TCP/IP simples                                 | Simple-TCPIP                       | N                     |
| Support de partage de fichiers SMB1.0/CIFS                      | FS-SMB1                            | Y                     |
| Limite de bande passante SMB                                    | FS-SMBBW                           | N                     |
| Service SNMP                                           | Service SNMP                       | N                     |
| Fournisseur WMI SNMP                                      | Fournisseur WMI-SNMP                  | N                     |
| Client Telnet                                          | Client Telnet                      | N                     |
| Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure               | FabricShieldedTools                | N                     |
| Fonctionnalités de Windows Defender                              | Fonctionnalités de Windows Defender          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Base de données interne Windows                              | Données interne Windows          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Moteur de Windows PowerShell 2.0                          | PowerShell-V2                      | (supprimer)             |
| Service de Configuration de l’état vous le souhaitez Windows PowerShell | DSC-Service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Service d'activation des processus Windows                     | A ÉTÉ                                | N                     |
| Modèle de processus                                          | Modèle de processus a été                  | N                     |
| Environnement .NET 3.5                                   | Environnement réseau a été                | N                     |
| API de configuration                                     | API de configuration a été                    | N                     |
| Sauvegarde WindowsServer                                  | Sauvegarde de Windows Server              | N                     |
| Outils de migration de WindowsServer                         | Migration                          | N                     |
| Gestion du stockage Windows basé sur des normes             | WindowsStorageManagementService    | N                     |
| Extension WinRM IIS                                    | WinRM-IIS-Ext                      | N                     |
| Serveur WINS                                            | WINS                               | N                     |
| Prise en charge WoW64                                          | Prise en charge WoW64                      | Y                     |
|                                                        |                                    |                       |
