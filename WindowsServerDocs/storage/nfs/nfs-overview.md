---
title: Vue d’ensemble du système de gestion de fichiers en réseau
description: Explique ce qu’est le système de fichiers réseau.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 72f71bc6605103f8240bcd531da3a5b58d470181
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403051"
---
# <a name="network-file-system-overview"></a>Vue d’ensemble du système de gestion de fichiers en réseau

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit le service de rôle système de fichiers réseau et les fonctionnalités incluses dans le rôle serveur services de fichiers et de stockage dans Windows Server. Le système NFS (Network File System) fournit une solution de partage de fichiers pour les entreprises qui possèdent des environnements hétérogènes qui incluent des ordinateurs Windows et non-Windows.

## <a name="feature-description"></a>Description des fonctionnalités

À l’aide du protocole NFS, vous pouvez transférer des fichiers entre des ordinateurs exécutant Windows et d’autres systèmes d’exploitation autres que Windows, tels que Linux ou UNIX.

NFS dans Windows Server comprend le serveur pour NFS et le client pour NFS. Un ordinateur exécutant Windows Server peut utiliser le service serveur pour NFS pour agir en tant que serveur de fichiers NFS pour d’autres ordinateurs clients non-Windows. Le service client pour NFS permet à un ordinateur Windows exécutant Windows Server d’accéder à des fichiers stockés sur un serveur NFS non-Windows.

## <a name="windows-and-windows-server-versions"></a>Versions de Windows et Windows Server

Windows prend en charge plusieurs versions du client et du serveur NFS, en fonction de la version du système d’exploitation et de la famille. 

| Systèmes d’exploitation | Versions du serveur NFS |Versions du client NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/A | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv 4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Cas pratiques

Voici quelques méthodes permettant d’utiliser NFS :

- Utilisez un serveur de fichiers NFS Windows pour fournir un accès multiprotocole au même partage de fichiers sur les protocoles SMB et NFS à partir de clients multi-plateformes.
- Déployez un serveur de fichiers NFS Windows dans un environnement de système d’exploitation non Windows prédominant pour fournir aux ordinateurs clients non-Windows l’accès aux partages de fichiers NFS.
- Migrez les applications d’un système d’exploitation vers un autre en stockant les données sur les partages de fichiers accessibles via les protocoles SMB et NFS.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Les fonctionnalités nouvelles et modifiées dans le système de fichiers réseau incluent la prise en charge de la version NFS 4,1 et un déploiement et une facilité de gestion améliorés. Pour plus d’informations sur les nouvelles fonctionnalités ou les modifications apportées à Windows Server 2012, consultez le tableau suivant :

|Fonctionnalité/fonction|Nouveauté ou mise à jour|Description|
|---|---|---|
|[Version NFS 4,1](#nfs-version-41)|Nouveau|Amélioration de la sécurité, des performances et de l’interopérabilité par rapport à NFS version 3.|
|[Infrastructure NFS](#nfs-infrastructure)|Mise à jour terminée|Améliore le déploiement et la gestion, et augmente la sécurité.|
|[Disponibilité continue de NFS version 3](#nfs-version-3-continuous-availability)|Mise à jour terminée|Améliore la disponibilité continue sur les clients NFS version 3.|
|[Améliorations du déploiement et de la gestion](#deployment-and-manageability-improvements)|Mise à jour terminée|Vous permet de déployer et de gérer facilement NFS avec les nouvelles applets de commande Windows PowerShell et un nouveau fournisseur WMI.|

## <a name="nfs-version-41"></a>Version NFS 4,1

La version NFS 4,1 implémente tous les aspects requis, en plus de certains aspects facultatifs, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Pseudo-système de fichiers**, système de fichiers qui sépare les espaces de noms physiques et logiques, et est compatible avec les versions NFS 3 et NFS version 2. Un alias est fourni pour le système de fichiers exporté, qui fait partie du Pseudo-système de fichiers.
- Les **RPC composés** combinent les opérations pertinentes et réduisent échanges excessifs.
- Les **sessions et le Trunking de session** activent une seule sémantique et permettent une disponibilité continue et de meilleures performances tout en utilisant plusieurs réseaux entre les clients NFS 4,1 et le serveur NFS.

## <a name="nfs-infrastructure"></a>Infrastructure NFS

Les améliorations apportées à l’infrastructure NFS globale dans Windows Server 2012 sont détaillées ci-dessous :

- L’infrastructure de transport de **représentation de données (XDR)/External de l’appel de procédure distante (RPC)** , alimentée par le protocole réseau Winsock, est disponible à la fois pour le serveur pour NFS et pour le client pour NFS. Cela remplace l’interface TDI (transport Device Interface), offre une meilleure prise en charge et offre une meilleure évolutivité et une mise à l’échelle côté réception (RSS).
- La fonctionnalité de **multiplexeur de port RPC** est compatible avec les pare-feu (moins de ports à gérer) et simplifie le déploiement de NFS.
- Les **caches réglés automatiquement et les pools de threads** sont des fonctions de gestion des ressources de la nouvelle infrastructure RPC/XDR qui sont dynamiques, en paramétrage automatique des caches et des pools de threads en fonction de la charge de travail. Cela supprime complètement les incertitudes liées au paramétrage des paramètres, en fournissant des performances optimales dès le déploiement de NFS.
- **Nouvelles options de mise en œuvre de la confidentialité et d’authentification Kerberos** avec l’ajout de la prise en charge de la confidentialité Kerberos (Krb5p), ainsi que les options d’authentification krb5 et krb5i existantes.
- Le mappage d’identité des applets de commande du **module Windows PowerShell** facilite la gestion du mappage des identités, la configuration de services AD LDS (Active Directory Lightweight Directory Services) (AD LDS) et la configuration de fichiers passwd et de fichiers plats UNIX et Linux.
- Le **point de montage de volume** vous permet d’accéder aux volumes montés sous un partage NFS avec la version NFS 4,1.
- La fonctionnalité de **multiplexage de port** prend en charge le multiplexeur de port RPC (port 2049), qui est compatible avec le pare-feu et simplifie le déploiement NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilité continue de NFS version 3

Les clients NFS version 3 peuvent avoir des basculements planifiés rapides et transparents avec davantage de disponibilité et des temps d’arrêt réduits. Le processus de basculement est plus rapide pour les clients NFS version 3 car :

- L’infrastructure de clusters autorise désormais une ressource par nom de réseau au lieu d’une ressource par partage, ce qui améliore considérablement le temps de basculement des ressources.
- Les chemins de basculement au sein d’un serveur NFS sont réglés pour améliorer les performances.
- L’inscription de caractères génériques dans un serveur NFS n’est plus nécessaire et les basculements sont plus précis.
- Les notifications de Status Monitor réseau (NSM) sont envoyées après un processus de basculement, et les clients n’ont plus besoin d’attendre que les délais d’attente TCP se reconnectent au serveur basculé.

Notez que Serveur pour NFS prend en charge le basculement transparent uniquement quand il est lancé manuellement, en général pendant les maintenances planifiées. Si un basculement non planifié se produit, les clients NFS perdent leurs connexions. Le serveur pour NFS n’intègre pas non plus le filtre de clé de reprise. Cela signifie que si une application locale ou une session SMB tente d’accéder au même fichier auquel accède un client NFS immédiatement après un basculement planifié, le client NFS peut perdre ses connexions (le basculement transparent échoue).

## <a name="deployment-and-manageability-improvements"></a>Améliorations du déploiement et de la gestion

Le déploiement et la gestion de NFS a amélioré les manières suivantes :

- Plus de 40 nouvelles applets de commande Windows PowerShell facilitent la configuration et la gestion des partages de fichiers NFS. Pour plus d’informations, consultez [applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Le mappage d’identité est amélioré avec un magasin de mappage de fichiers plats local et de nouvelles applets de commande Windows PowerShell pour configurer le mappage d’identité.
- L’interface utilisateur graphique de Gestionnaire de serveur est plus facile à utiliser.
- Le nouveau fournisseur WMI version 2 est disponible pour faciliter la gestion.
- Le multiplexeur de port RPC (port 2049) est compatible avec le pare-feu et simplifie le déploiement de NFS.

## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur

Dans Gestionnaire de serveur ou le centre d' [administration Windows](../../manage/windows-admin-center/understand/windows-admin-center.md) plus récent, utilisez l’Assistant Ajout de rôles et de fonctionnalités pour ajouter le service de rôle serveur pour NFS (sous le rôle Services de fichiers et iSCSI). Pour obtenir des informations générales sur l’installation de fonctionnalités, voir [Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Les outils de serveur pour NFS incluent le composant logiciel enfichable MMC services du système de fichiers réseau pour gérer les composants serveur pour NFS et client pour NFS. À l’aide du composant logiciel enfichable, vous pouvez gérer les composants serveur pour NFS installés sur l’ordinateur. Le serveur pour NFS contient également plusieurs outils d’administration en ligne de commande Windows :

- **Mount** monte un partage NFS distant (également appelé exportation) localement et le mappe à une lettre de lecteur local sur l’ordinateur client Windows.
- **Nfsadmin** gère les paramètres de configuration du serveur pour les composants NFS et client pour NFS.
- **Nfsshare** configure les paramètres de partage NFS pour les dossiers partagés à l’aide de Server pour NFS.
- **Nfsstat** affiche ou réinitialise les statistiques des appels reçus par le serveur pour NFS.
- **Showmount** affiche les systèmes de fichiers montés, exportés par le serveur pour NFS.
- **Umount** supprime les lecteurs montés NFS.

NFS dans Windows Server 2012 introduit le module NFS pour Windows PowerShell avec plusieurs nouvelles applets de commande spécifiquement pour NFS. Ces applets de commande offrent un moyen simple d’automatiser les tâches de gestion NFS. Pour plus d’informations, consultez [applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Informations complémentaires

Le tableau suivant fournit des ressources supplémentaires pour l’évaluation de NFS.

|Type de contenu|Références|
|---|---|
|Déploiement|[Déployer le système de fichiers réseau](deploy-nfs.md)|
|Opérations|[Applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Technologies associées|[Storage](../storage.md) (Stockage)|