---
title: Comparatif des éditions Standard et Datacenter de Windows Server 2019
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-2932-80cab33fe914
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 2cb368b5ff4723c1afb53c5a9e787d9f8a39bef3
ms.sourcegitcommit: 40e4ba214954d198936341c4d6ce1916dc891169
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690455"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2019"></a>Comparatif des éditions Standard et Datacenter de Windows Server 2019

> S'applique à : Windows Server 2019
  
## <a name="locks-and-limits"></a>Verrous et limites

| Verrous et limites                 | Windows Server 2019 Standard | Windows Server 2019 Datacenter |  
| -------------------              | ---------------------------  | --------------------------- |  
| Nombre maximal d'utilisateurs          | Basé sur les licences d'accès client                | Basé sur les licences d'accès client |
| Nombre maximal de connexions SMB          | 16 777 216                   | 16 777 216 |
| Nombre maximal de connexions RRAS         | illimité                    | illimité |
| Nombre maximal de connexions IAS          | 2 147 483 647                | 2 147 483 647 |
| Nombre maximal de connexions RDS          | 65 535                       | 65 535 |
| Nombre maximal de sockets 64 bits | 64                           | 64 |
| Nombre maximal de cœurs          | illimité                    | illimité |
| Mémoire vive maximale                      | 24 To                        | 24 To |
| Peut être utilisé comme invité de virtualisation | Oui, deux machines virtuelles plus un hôte Hyper-V par licence|Oui, <strong>nombre illimité de machines virtuelles</strong>, plus un hôte Hyper-V par licence |
| Serveur joignable à un domaine        | oui                           | oui |
| Protection/pare-feu du réseau de périmètre| non                            | non    |
| DirectAccess                    | oui                           | oui |
| DLNA – codecs et diffusion multimédia en continu sur le Web | Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur | Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur |

## <a name="server-roles"></a>Rôles serveur

|Rôles Windows Server disponibles|Services de rôle|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|----------|---------------------------|  
|Services de certificats Active Directory| |Oui|Oui|
|Services de domaine Active Directory| |Oui|Oui|
|Services de fédération Active Directory (AD FS)| |Oui|Oui|
|Services AD LDS (Active Directory Lightweight Directory Services)| |Oui|Oui|
|Services AD RMS (Active Directory Rights Management Services)| |Oui|Oui|
|Attestation d’intégrité de l’appareil| |Oui|Oui|
|Serveur DHCP| |Oui|Oui|
|Serveur DNS| |Oui|Oui|
|Serveur de télécopie| |Oui|Oui|
|Services de fichiers et de stockage|Serveur de fichiers|Oui|Oui|
|Services de fichiers et de stockage|BranchCache pour fichiers réseau|Oui|Oui|
|Services de fichiers et de stockage|Déduplication des données|Oui|Oui|
|Services de fichiers et de stockage|Espaces de noms DFS|Oui|Oui|
|Services de fichiers et de stockage|Réplication DFS|Oui|Oui|
|Services de fichiers et de stockage|Outils de gestion de ressources pour serveur de fichiers|Oui|Oui|
|Services de fichiers et de stockage|Service Agent VSS du serveur de fichiers|Oui|Oui|
|Services de fichiers et de stockage|iSCSI Target Server|Oui|Oui|
|Services de fichiers et de stockage|Fournisseur de stockage cible iSCSI|Oui|Oui|
|Services de fichiers et de stockage|Serveur pour NFS|Oui|Oui|
|Services de fichiers et de stockage|Dossiers de travail|Oui|Oui|
|Services de fichiers et de stockage|Services de stockage|Oui|Oui|
|Service Guardian hôte| |Oui|Oui|
|Hyper-V| |Oui|Oui, y compris des machines virtuelles dotées d'une protection maximale|
|Contrôleur de réseau| |Non| <strong>Oui</strong> |
|Services de stratégie et d'accès réseau| |Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Services d'impression et de numérisation de document| |Oui|Oui|
|Accès à distance| |Oui|Oui|
|Services Bureau à distance| |Oui|Oui|
|Services d'activation en volume| |Oui|Oui|
|Services Web (IIS)| |Oui|Oui|
|Services de déploiement Windows| |Oui*|Oui*|
|Expérience Windows Server Essentials| |Non | Non |
|Windows Server Update Services| |Oui|Oui|

\* Le serveur de transport des services de déploiement Windows est une nouveauté pour les installations minimales dans Windows Server 2019 (également dans le canal semi-annuel à partir de Windows Server, version 1803)


## <a name="features"></a>Fonctionnalités

|Fonctionnalités Windows Server pouvant être installées avec le Gestionnaire de serveur (ou PowerShell)|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5 |Oui|Oui|
|.NET Framework 4.7 |Oui|Oui|
|Service de transfert intelligent en arrière-plan (BITS)|Oui|Oui|
|Chiffrement de lecteur BitLocker|Oui|Oui|
|Déverrouillage réseau BitLocker|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|BranchCache|Oui|Oui|
|Client pour NFS|Oui|Oui|
|Conteneurs|Oui (conteneurs Windows en nombre illimité ; jusqu’à deux conteneurs Hyper-V)|Oui (<strong>conteneurs Windows et Hyper-V en nombre illimité</strong>) |
|Data Center Bridging|Oui|Oui|
|DirectPlay|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Stockage étendu|Oui|Oui|
|Clustering de basculement|Oui|Oui|
|Gestion des stratégies de groupe|Oui|Oui|
|Support Hyper-V pour Guardian hôte|Non| <strong>Oui</strong> |
|Qualité de service E/S|Oui|Oui|
|IIS Hostable Web Core|Oui|Oui|
|Client d'impression Internet|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Serveur IPAM|Oui|Oui|
|Service Serveur iSNS|Oui|Oui|
|Moniteur de port LPR|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Extension ISS Management OData|Oui|Oui|
|Media Foundation|Oui|Oui|
|Mise en file d'attente des messages|Oui|Oui|
|MPIO (Multipath I/O)|Oui|Oui|
|MultiPoint Connector|Oui|Oui|
|Équilibrage de la charge réseau|Oui|Oui|
|Protocole PNRP (Peer Name Resolution Protocol)|Oui|Oui|
|Expérience audio-vidéo haute qualité Windows|Oui|Oui|
|Kit d'administration de Microsoft Connection Manager (CMAK) RAS|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Assistance à distance|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Compression différentielle à distance|Oui|Oui|
|Outils d'administration de serveur distant|Oui|Oui|
|Proxy RPC sur HTTP|Oui|Oui|
|Collection des événements de configuration et de démarrage|Oui|Oui|
|Services TCP/IP simples|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Support de partage de fichiers SMB 1.0/CIFS|Installé|Installé|
|Limite de bande passante SMB|Oui|Oui|
|Serveur SMTP|Oui|Oui|
|Service SNMP|Oui|Oui|
|Équilibrage de la charge logicielle|Oui|Oui|
|Réplica de stockage|Oui|Oui|
|Client Telnet|Oui|Oui|
|Client TFTP|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure|Oui|Oui|
|Redirecteur WebDAV|Oui|Oui|
|Windows Biometric Framework|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Fonctionnalités de Windows Defender|Installé|Installé|
|Windows Identity Foundation 3.5|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Base de données interne Windows|Oui|Oui|
|Windows PowerShell|Installé|Installé|
|Service d'activation des processus Windows|Oui|Oui|
|Service Windows Search|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Sauvegarde Windows Server|Oui|Oui|
|Outils de migration de Windows Server|Oui|Oui|
|Gestion du stockage Windows basé sur des normes|Oui|Oui|
|Windows TIFF IFilter|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Extension WinRM IIS|Oui|Oui|
|Serveur WINS|Oui|Oui|
|Service de réseau local sans fil|Oui|Oui|
|Prise en charge WoW64|Installé|Installé|
|Visionneuse XPS|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|

|Fonctionnalités généralement disponibles|Windows Server 2019 Standard|Windows Server 2019 Datacenter|  
|-------------------|----------|---------------------------|  
|Best Practices Analyzer|Oui|Oui|
|DirectAccess|Oui|Oui|
|Mémoire dynamique (virtualisation)|Oui|Oui|
|Ajout/remplacement de RAM à chaud|Oui|Oui|
|Microsoft Management Console|Oui|Oui|
|Interface serveur minimale|Oui|Oui|
|Équilibrage de la charge réseau|Oui|Oui|
|Windows PowerShell|Oui|Oui|
|Option d’installation minimale|Oui|Oui|
|Gestionnaire de serveur|Oui|Oui|
|SMB Direct et SMB sur RDMA|Oui|Oui|
|SDN (Software-Defined Networking)|Non| <strong>Oui</strong> |
|Service de migration du stockage|Oui|Oui|
| Réplica de stockage         | Oui, (1 partenariat et 1 groupe de ressources avec un seul volume de 2 To)    | Oui, <strong>illimité</strong> |
|Espaces de stockage|Oui|Oui|
|Espaces de stockage directs|Non| <strong>Oui</strong> |
|Services d'activation en volume|Oui|Oui|
|Intégration du service VSS (Volume Shadow Copy Service)|Oui|Oui|
|Windows Server Update Services|Oui|Oui|
|Gestionnaire de ressources système Windows|Oui|Oui|
|Journalisation de licence serveur|Oui|Oui|
|Activation héritée|En tant qu'invité si hébergement sur Datacenter| <strong>Peut être un hôte ou un invité</strong> |
|Dossiers de travail|Oui|Oui|
