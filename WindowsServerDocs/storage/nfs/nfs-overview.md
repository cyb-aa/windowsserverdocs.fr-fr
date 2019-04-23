---
title: Vue d’ensemble du système de gestion de fichiers en réseau
description: Explique le système de fichiers réseau.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fb31cff44cac6bd66f9aa5b7234ff3fd3b215ccf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876300"
---
# <a name="network-file-system-overview"></a>Vue d’ensemble du système de gestion de fichiers en réseau

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit le service de rôle de système de fichiers réseau et les fonctionnalités incluses avec le rôle de serveur Services de fichiers et stockage dans Windows Server. Système NFS (Network File) fournit une solution pour les entreprises qui disposent d’environnements hétérogènes qui incluent Windows et les ordinateurs non Windows de partage de fichiers.

## <a name="feature-description"></a>Description des fonctionnalités

À l’aide du protocole NFS, vous pouvez transférer des fichiers entre ordinateurs exécutant Windows et autres systèmes d’exploitation non Windows, tels que Linux ou UNIX.

NFS dans Windows Server inclut un serveur pour NFS et Client pour NFS. Un ordinateur exécutant Windows Server permettre utiliser serveur pour NFS pour agir comme un serveur de fichiers NFS pour les autres ordinateurs non Windows client. Client pour NFS permet à un ordinateur Windows exécutant Windows Server pour accéder aux fichiers stockés sur un serveur NFS de Windows non.

## <a name="windows-and-windows-server-versions"></a>Versions de Windows et Windows Server

Windows prend en charge plusieurs versions du client NFS et du serveur, en fonction de la version du système d’exploitation et votre famille. 

| Systèmes d’exploitation | Versions de serveur NFS |Versions du Client NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/A | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## <a name="practical-applications"></a>Cas pratiques

Voici quelques méthodes que vous pouvez utiliser NFS :

- Utiliser un serveur de fichiers NFS de Windows pour fournir un accès de multi-protocol au même partage de fichiers via les protocoles SMB et NFS à partir de clients de multi-plateformes.
- Déployer un serveur de fichiers NFS de Windows dans un environnement de système d’exploitation de majoritairement non Windows pour fournir des ordinateurs l’accès aux partages de fichiers NFS non Windows client.
- Migrer des applications à partir d’un système d’exploitation vers un autre en stockant les données sur les partages de fichiers accessibles par le biais des protocoles SMB et NFS.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Fonctionnalités nouvelles et modifiées dans le système de fichiers réseau incluent la prise en charge de NFS version 4.1 et déploiement amélioré et la facilité de gestion. Pour plus d’informations sur les fonctionnalités nouvelles ou modifiées dans Windows Server 2012, consultez le tableau suivant :

|Fonctionnalité/fonction|Nouveauté ou mise à jour|Description|
|---|---|---|
|[NFS version 4.1](#nfs-version-4.1)|Nouveau|Sécurité accrue, performances et l’interopérabilité par rapport à NFS version 3.|
|[Infrastructure NFS](#nfs-infrastructure)|Mise à jour terminée|Améliore le déploiement et la facilité de gestion et accroît la sécurité.|
|[Disponibilité continue de NFS version 3](#nfs-version-3-continuous-availability)|Mise à jour terminée|Améliore la disponibilité continue sur les clients NFS version 3.|
|[Améliorations de déploiement et la facilité de gestion](#deployment-and-manageability-improvements)|Mise à jour terminée|Vous permet de facilement déployer et gérer des NFS avec les nouvelles applets de commande Windows PowerShell et un nouveau fournisseur WMI.|

## <a name="nfs-version-41"></a>NFS version 4.1

NFS version 4.1 implémente tous les aspects requis, en plus de certains aspects facultatif, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Système de fichiers de pseudo**, un système de fichiers qui sépare l’espace de noms physique et logique et est compatible avec NFS version 3 et NFS version 2. Un alias est fourni pour le système de fichiers exporté, qui fait partie du système de fichiers pseudo.
- **Composée RPC** combiner des opérations pertinentes et de réduire les échanges excessifs.
- **Sessions et trunking** permet une sémantique et permet une disponibilité continue et meilleures performances lors de l’utilisation de plusieurs réseaux entre les clients NFS 4.1 et le serveur NFS.

## <a name="nfs-infrastructure"></a>Infrastructure NFS

Améliorations apportées à l’infrastructure globale de NFS dans Windows Server 2012 sont détaillées ci-dessous :

- Le **l’appel de procédure distante (RPC) / représentation sous forme de données externes (XDR)** infrastructure de transport, alimenté par le protocole de réseau WinSock, est disponible pour les deux serveur pour NFS et Client pour NFS. Cela remplace appareil Interface TDI (Transport), offre une meilleure prise en charge et fournit une meilleure extensibilité et mise à l’échelle côté réception (RSS).
- Le **port RPC multiplexeur** fonctionnalité est compatible avec les pare-feu (moins de ports pour gérer) et simplifie le déploiement de NFS.
- **Caches réglées automatiquement et les pools de threads** sont des fonctionnalités de gestion de la nouvelle infrastructure RPC/XDR sont dynamiques, réglage automatiquement les caches et selon la charge de travail des pools de threads de ressources. Cela supprime complètement les spéculations impliqués lors du réglage des paramètres, offrant ainsi des performances optimales, dès que NFS est déployé.
- **Nouvelles de Kerberos options d’implémentation et l’authentification de confidentialité** avec l’ajout de prise en charge de la confidentialité (Krb5p) Kerberos, ainsi que les options d’authentification krb5i krb5 existant.
- **Module de mappage Windows PowerShell d’identité** applets de commande rendent plus faciles à gérer le mappage d’identité, configurer Active Directory Lightweight Directory Services (AD LDS) et configurer passwd UNIX et Linux et les fichiers plats.
- **Point de montage de volume** permet d’accéder à des volumes montés sous un partage NFS avec NFS version 4.1.
- Le **Port multiplexage** fonctionnalité prend en charge le port RPC multiplexeur (ports 2049), qui est compatible avec les pare-feu et simplifie le déploiement de NFS.

## <a name="nfs-version-3-continuous-availability"></a>Disponibilité continue de NFS version 3

Les clients NFS version 3 peuvent avoir des basculements planifiés rapides et transparents avec plus de disponibilité et de temps mort réduit. Le processus de basculement est plus rapide pour les clients NFS version 3, car :

- L’infrastructure de clustering permet désormais d’une ressource par le nom de réseau au lieu d’une ressource par partage, ce qui améliore considérablement les temps de basculement des ressources.
- Chemins d’accès de basculement au sein d’un serveur NFS sont paramétrées pour améliorer les performances.
- L’inscription de caractère générique dans un serveur NFS n’est plus nécessaire, et les basculements sont plus enrichis.
- Moniteur d’état (NSM) du réseau les notifications sont envoyées après un processus de basculement, et les clients n’avez plus besoin d’attendre des délais d’expiration TCP pour vous reconnecter à l’échec de la sur le serveur.

Notez que Serveur pour NFS prend en charge le basculement transparent uniquement quand il est lancé manuellement, en général pendant les maintenances planifiées. Si un basculement non planifié se produit, les clients NFS perdent leurs connexions. Également serveur pour NFS n’a pas une intégration avec le filtre de clé de reprise. Cela signifie que si une application locale ou une session SMB tente d’accéder au même fichier auquel accède un client NFS immédiatement après un basculement planifié, le client NFS peut perdre ses connexions (le basculement transparent échoue).

## <a name="deployment-and-manageability-improvements"></a>Améliorations de déploiement et la facilité de gestion

Déploiement et la gestion NFS a amélioré de plusieurs manières :

- Plus de 40 nouvelles applets de commande Windows PowerShell rendent plus faciles à configurer et gérer les partages de fichiers NFS. Pour plus d’informations, consultez [applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Mappage d’identité est améliorée avec une banque de mappage de fichier plat local et les nouvelles applets de commande Windows PowerShell pour configurer le mappage d’identité.
- L’interface utilisateur graphique de gestionnaire de serveur est plus facile à utiliser.
- Nouveau fournisseur WMI version 2 est disponible pour faciliter la gestion.
- Le port multiplexeur (port RPC 2049) est compatible avec les pare-feu et simplifie le déploiement de NFS.

## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur

Dans le Gestionnaire de serveur - ou la plus récente [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) -permet d’ajouter le service serveur pour NFS rôle Ajout de rôles et fonctionnalités Assistant (sous les fichiers et iSCSI rôle de Services). Pour obtenir des informations générales sur l’installation de fonctionnalités, voir [Installer ou désinstaller des rôles, des services de rôle ou des fonctionnalités](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). Outils NFS de serveur pour inclure les Services pour le composant logiciel enfichable MMC de système de fichiers réseau pour gérer le serveur pour NFS et Client pour NFS. À l’aide du composant logiciel enfichable, vous pouvez gérer le serveur pour NFS installé des composants sur l’ordinateur. Serveur pour NFS contient également plusieurs outils d’administration de ligne de commande de Windows :

- **Monter** monte un partage NFS distant (également appelé une exportation) localement et le mappe à une lettre de lecteur local sur l’ordinateur client Windows.
- **Nfsadmin** gère les paramètres de configuration du serveur pour NFS et Client pour les composants NFS.
- **Nfsshare** configure les paramètres de partage NFS pour les dossiers partagés à l’aide du serveur pour NFS.
- **Nfsstat appartient** affiche ou réinitialise les statistiques d’appels reçus par le serveur pour NFS.
- **Showmount** affiche les systèmes de fichiers monté exportés par serveur pour NFS.
- **Unmount** supprime les lecteurs montés NFS.

NFS dans Windows Server 2012 introduit spécifiquement pour NFS avec plusieurs nouvelles applets de commande du module NFS pour Windows PowerShell. Ces applets de commande fournissent un moyen simple pour automatiser les tâches de gestion NFS. Pour plus d’informations, consultez [applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## <a name="additional-information"></a>Informations complémentaires

Le tableau suivant fournit des ressources supplémentaires pour l’évaluation de NFS.

|Type de contenu|Références|
|---|---|
|Déploiement|[Déployer le système de fichiers réseau](deploy-nfs.md)|
|Opérations|[Applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Technologies associées|[Stockage dans Windows Server](../storage.md)|