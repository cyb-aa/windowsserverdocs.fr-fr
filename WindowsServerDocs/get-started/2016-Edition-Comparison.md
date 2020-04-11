---
title: Produits et éditions Windows Server 2016
description: Explique les différences entre les éditions Windows Server Standard et Windows Server Datacenter
ms.prod: windows-server
ms.date: 10/04/2019
ms.technology: server-general
ms.topic: article
ms.assetid: c5ca3bfe-7ced-49f6-a932-80cab33f419e
author: jasongerend
ms.author: jgerend
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: ce4c35f0b65d0461e9dc2e23404d2637aecff415
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827102"
---
# <a name="comparison-of-standard-and-datacenter-editions-of-windows-server-2016"></a>Comparatif des éditions Standard et Datacenter de Windows Server 2016

> S'applique à : Windows Server 2016
  
## <a name="locks-and-limits"></a>Verrous et limites

| Verrous et limites | Windows Server 2016 Standard | Windows Server 2016 Datacenter |
| ------------------- |---------- | --------------------------- |  
| Nombre maximal d’utilisateurs | Basé sur les licences d'accès client   | Basé sur les licences d'accès client     |
| Nombre maximal de connexions SMB | 16 777 216      | 16 777 216          |
| Nombre maximal de connexions RRAS| Illimité       | Illimité         |
| Nombre maximal de connexions IAS | 2 147 483 647   | 2 147 483 647        |
| Nombre maximal de connexions RDS | 65535           | 65535             |
| Nombre maximal de sockets 64 bits | 64     | 64                |
| Nombre maximal de cœurs | Illimité       | Illimité      |
| Mémoire vive maximale             | 24 To           | 24 To             |
| Peut être utilisé comme invité de virtualisation | Oui, deux machines virtuelles plus un hôte Hyper-V par licence | Oui, <strong>nombre illimité de machines virtuelles</strong>, plus un hôte Hyper-V par licence |
| Serveur joignable à un domaine | oui            | oui                |
| Protection/pare-feu du réseau de périmètre | non     | non                 |
| DirectAccess            | oui             | oui                |
| DLNA – codecs et diffusion multimédia en continu sur le Web | Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur | Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur |

## <a name="server-roles"></a>Rôles serveur

| Rôles Windows Server disponibles     | Services de rôle | Windows Server 2016 Standard | Windows Server 2016 Datacenter |  
| -------------------                | ----------    | ----------                   | ---------------------------    |  
| Services de certificats Active Directory|              | Oui                          | Oui                            |
| Services de domaine Active Directory    |               | Oui                          | Oui                            |
| Services AD FS (Active Directory Federation Services)|               | Oui                          | Oui                            |
| Services AD LDS (Active Directory Lightweight Directory Services)| |Oui|Oui|
| Services AD RMS (Active Directory Rights Management Services)| |Oui|Oui|
| Attestation d’intégrité de l’appareil| |Oui|Oui|
| Serveur DHCP| |Oui|Oui|
| Serveur DNS| |Oui|Oui|
| Serveur de télécopie| |Oui|Oui|
| Services de fichiers et de stockage|Serveurs de fichiers|Oui|Oui|
| Services de fichiers et de stockage|BranchCache pour fichiers réseau|Oui|Oui|
| Services de fichiers et de stockage|Déduplication des données|Oui|Oui|
| Services de fichiers et de stockage|Espaces de noms DFS|Oui|Oui|
| Services de fichiers et de stockage|Réplication DFS|Oui|Oui|
| Services de fichiers et de stockage|File Server Resource Manager|Oui|Oui|
| Services de fichiers et de stockage|Service Agent VSS du serveur de fichiers|Oui|Oui|
| Services de fichiers et de stockage|iSCSI Target Server|Oui|Oui|
| Services de fichiers et de stockage|Fournisseur de stockage cible iSCSI|Oui|Oui|
| Services de fichiers et de stockage|Serveur pour NFS|Oui|Oui|
| Services de fichiers et de stockage|Dossiers de travail|Oui|Oui|
| Services de fichiers et de stockage|Services de stockage|Oui|Oui|
| Service Guardian hôte| |Oui|Oui|
| Hyper-V| |Oui|Oui, <strong>y compris des machines virtuelles dotées d’une protection maximale</strong>|
| MultiPoint Services| |Oui|Oui|
| Contrôleur réseau| |Non| <strong>Oui</strong> |
| Network Policy and Access Services| |Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
| Services d'impression et de numérisation de document| |Oui|Oui|
| Accès à distance| |Oui|Oui|
| Services Bureau à distance| |Oui|Oui|
| Services d’activation en volume| |Oui|Oui|
| Services Web (IIS)| |Oui|Oui|
| Windows Deployment Services| |Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
| Expérience Windows Server Essentials| |Oui|Oui|
| Windows Server Update Services| |Oui|Oui|

## <a name="features"></a>Fonctionnalités

|Fonctionnalités Windows Server pouvant être installées avec le Gestionnaire de serveur (ou PowerShell)|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|.NET Framework 3.5|Oui|Oui|
|.NET Framework 4.6|Oui|Oui|
|Service de transfert intelligent en arrière-plan (BITS)|Oui|Oui|
|Chiffrement du lecteur BitLocker|Oui|Oui|
|Déverrouillage réseau BitLocker|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|BranchCache|Oui|Oui|
|Client pour NFS|Oui|Oui|
|Conteneurs|Oui (conteneurs Windows en nombre illimité ; conteneurs Hyper-V, jusqu’à 2)|Oui (tous les types de conteneurs, en nombre illimité)|
|Data Center Bridging|Oui|Oui|
|DirectPlay|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Stockage étendu|Oui|Oui|
|Clustering de basculement|Oui|Oui|
|Gestion des stratégies de groupe|Oui|Oui|
|Support Hyper-V pour Guardian hôte|Non|<strong>Oui</strong> |
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
|Network Load Balancing|Oui|Oui|
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
|Équilibrage de la charge logicielle|Non| <strong>Oui</strong> |
|Réplica de stockage|Non|Oui|
|Client Telnet|Oui|Oui|
|Client TFTP|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Outils de protection d'ordinateur virtuel pour la gestion d'infrastructure|Oui|Oui|
|Redirecteur WebDAV|Oui|Oui|
|Windows Biometric Framework|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Fonctionnalités de Windows Defender|Installé|Installé|
|Windows Identity Foundation 3.5|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Base de données interne Windows|Oui|Oui|
|Windows PowerShell|Installé|Installé|
|Service d’activation des processus Windows|Oui|Oui|
|Service Windows Search|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Sauvegarde Windows Server|Oui|Oui|
|Outils de migration de Windows Server|Oui|Oui|
|Gestion du stockage basé sur les standards Windows|Oui|Oui|
|Windows TIFF IFilter|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|
|Extension WinRM IIS|Oui|Oui|
|Serveur WINS|Oui|Oui|
|Service de réseau local sans fil|Oui|Oui|
|Prise en charge de WoW64|Installé|Installé|
|Visionneuse XPS|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|Oui, si vous choisissez l'option d'installation Serveur avec Expérience utilisateur|

|Fonctionnalités généralement disponibles|Windows Server 2016 Standard|Windows Server 2016 Datacenter|  
|-------------------|----------|---------------------------|  
|Best Practice Analyzer|Oui|Oui|
|Direct Access|Oui|Oui|
|Mémoire dynamique (virtualisation)|Oui|Oui|
|Ajout/remplacement de RAM à chaud|Oui|Oui|
|Console MMC (Microsoft Management Console)|Oui|Oui|
|Interface serveur minimale|Oui|Oui|
|Network Load Balancing|Oui|Oui|
|Windows PowerShell|Oui|Oui|
|Option d’installation minimale|Oui|Oui|
|Option d’installation de Nano Server|Oui|Oui|
|Gestionnaire de serveur|Oui|Oui|
|SMB Direct et SMB sur RDMA|Oui|Oui|
| SDN (Software-Defined Networking) | Non | <strong>Oui</strong> |
|Réplica de stockage | Non | <strong>Oui</strong> |
|Espaces de stockage|Oui|Oui|
|Espaces de stockage directs|Non| <strong>Oui</strong> |
|Services d’activation en volume|Oui|Oui|
|Intégration du service VSS (Volume Shadow Copy Service)|Oui|Oui|
|Windows Server Update Services|Oui|Oui|
|Gestionnaire de ressources système Windows|Oui|Oui|
|Journalisation de licence serveur|Oui|Oui|
|Activation héritée|En tant qu'invité si hébergement sur Datacenter| <strong>Peut être hôte ou invité</strong> |
|Dossiers de travail|Oui|Oui|

