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
ms.sourcegitcommit: bad0692ffd0773f61ac62bd140a421cb02c68acf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099138"
---
# Vue d’ensemble du système de gestion de fichiers en réseau

>S’applique à: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit le service de rôle de système de fichiers réseau et les fonctionnalités incluses dans le rôle de serveur Services de fichiers et stockage dans Windows Server. Système de fichiers réseau (NFS) fournit une solution pour les entreprises qui disposent d’environnements hétérogènes qui incluent à la fois Windows et des ordinateurs non-Windows de partage de fichiers.

## Description des fonctionnalités

À l’aide du protocole NFS, vous pouvez transférer des fichiers entre des ordinateurs exécutant Windows et autres systèmes d’exploitation autres que Windows, par exemple, Linux ou UNIX.

NFS dans Windows Server inclut un serveur pour NFS et Client pour NFS. Un ordinateur exécutant Windows Server peut utiliser le serveur pour NFS qui agira comme un serveur de fichiers NFS pour d’autres ordinateurs autres que Windows client. Client pour NFS permet à un ordinateur basé sur Windows exécutant Windows Server pour accéder aux fichiers stockés sur un serveur non - Windows NFS.

## Versions de Windows et Windows Server

Windows prend en charge plusieurs versions du client NFS et du serveur, en fonction de la version du système d’exploitation et leur famille. 

| Systèmes d’exploitation | Versions de serveur NFS |Versions de Client NFS|
| ----------------- | ------------------- | ----------------- |
| Windows 7, Windows 8.1, Windows 10 | N/A | NFSv2, NFSv3 |
| Windows Server 2008, Windows Server 2008 R2 | NFSv2, NFSv3 | NFSv2, NFSv3 |
| Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019 | NFSv2, NFSv3, NFSv4.1  | NFSv2, NFSv3 |

## Applications pratiques

Voici quelques solutions que vous pouvez utiliser NFS:

- Utilisez un serveur de fichiers NFS Windows pour fournir l’accès multi-protocol dans le même partage de fichiers sur les protocoles SMB et NFS à partir de clients multiplateformes.
- Déployer un serveur de fichiers NFS Windows dans un environnement de système d’exploitation principalement non Windows pour fournir des ordinateurs à accéder à des partages de fichiers NFS client non-Windows.
- Migrer des applications à partir d’un système d’exploitation vers un autre en stockant les données sur les partages de fichiers accessibles par le biais des protocoles SMB et NFS.

## Fonctionnalités nouvelles et modifiées

Fonctionnalités nouvelles et modifiées dans le système de fichiers réseau incluent la prise en charge de la version 4.1 NFS et déploiement amélioré et une facilité de gestion. Pour plus d’informations sur les fonctionnalités qui sont nouveaux ou modifiés dans Windows Server 2012, passez en revue le tableau suivant:

|Fonctionnalité/fonction|Nouveauté ou mise à jour|Description|
|---|---|---|
|[NFS version 4.1](#nfs-version-4.1)|Nouveau|Une sécurité accrue, performances et l’interopérabilité par rapport aux NFS version 3.|
|[Infrastructure NFS](#nfs-infrastructure)|Mise à jour|Améliore le déploiement et la facilité de gestion et augmente la sécurité.|
|[Disponibilité permanente NFS version 3](#nfs-version-3-continuous-availability)|Mise à jour|Améliore la disponibilité en continu sur des clients NFS version 3.|
|[Améliorations de déploiement et la facilité de gestion](#deployment-and-manageability-improvements)|Mise à jour|Vous permet de facilement déployer et gérer des NFS avec les nouvelles applets de commande Windows PowerShell et un nouveau fournisseur WMI.|

## NFS version 4.1

NFS version 4.1 implémente tous les aspects requis, en plus de certains des aspects facultatifs, de [RFC 5661](https://tools.ietf.org/html/rfc5661):

- **Système de fichiers Pseudo**, un système de fichiers qui sépare l’espace de noms physique et logique et est compatible avec NFS version 3 et NFS version 2. Un alias est fourni pour le système de fichiers exporté, qui fait partie du système de fichiers pseudo.
- **Appels de procédure distante composée** combiner les opérations pertinentes et de réduire les échanges.
- **Sessions et agrégation session** permet simplement une sémantique et permet une disponibilité permanente et obtenir de meilleures performances lors de l’utilisation de plusieurs réseaux entre les clients de 4.1 NFS et le serveur NFS.

## Infrastructure NFS

Améliorations apportées à l’infrastructure NFS globale dans Windows Server 2012 sont indiquées ci-dessous:

- Le **l’appel de procédure distante (RPC) / représentation sous forme de données externes (XDR)** infrastructure de transport, pris en charge par le protocole réseau WinSock, est disponible pour les deux serveur pour NFS et le Client pour NFS. Cela remplace Device Interface TDI (Transport), mieux prendre en charge et propose une meilleure évolutivité et mise à l’échelle côté réception (RSS).
- La fonctionnalité de **port RPC multiplexage** est compatible avec les pare-feu (moins de ports pour gérer) et simplifie le déploiement de NFS.
- **Caches optimisées automatique et les pools de threads** sont les fonctionnalités de gestion des ressources de la nouvelle infrastructure RPC/XDR qui sont dynamiques, réglage automatique des caches et en fonction de la charge de travail des pools de threads. Cela supprime complètement les spéculations qui entrent en jeu lors du réglage des paramètres, en fournissant des performances optimales dès que NFS est déployé.
- **Options de mise en œuvre et l’authentification Kerberos nouvelle confidentialité** avec l’ajout de prise en charge de la confidentialité (Krb5p) Kerberos, ainsi que les options d’authentification krb5i krb5 existant.
- **Module Windows PowerShell de la mise en correspondance identité** applets de commande facilitent la gérer le mappage des identités, configurer Active Directory Lightweight Directory Services (AD LDS) et configuration de mot de passe UNIX et Linux et les fichiers plats.
- **Point de montage de volume** vous permet de qu'accéder volumes montés sous un partage NFS avec NFS version 4.1.
- La fonctionnalité de **Multiplexage de Port** prend en charge le port RPC multiplexage (port 2049), qui est compatible avec les pare-feu et simplifie le déploiement de NFS.

## Disponibilité permanente NFS version 3

Clients de la version 3 NFS peuvent avoir rapides et transparents basculements planifiées avec plus de disponibilité et de réduction des interruptions. Le processus de basculement est plus rapide pour les clients de la version 3 NFS dans la mesure où:

- L’infrastructure de gestion de clusters permet désormais à une ressource par le nom du réseau au lieu d’une ressource par partage, ce qui améliore considérablement le temps de basculement des ressources.
- Chemins de reprise au sein d’un serveur NFS sont réglés pour de meilleures performances.
- Inscription de caractère générique dans un serveur NFS n’est plus nécessaire et les basculements sont plus ajustés.
- Les notifications d’état moniteur (NSM) réseau sont envoyées après un processus de basculement, et les clients n’ont plus besoin d’attendre de délais d’attente TCP pour vous reconnecter à l’échec de serveur.

Notez que serveur pour NFS prend en charge le basculement transparent uniquement quand manuellement lancé, en règle générale, durant la maintenance planifié. Si un basculement non planifié se produit, les clients NFS perdent leurs connexions. Serveur pour NFS n’a pas également disposer d’une intégration avec le filtre de reprise d’une clé. Cela signifie que si une application locale ou une session SMB tente d’accéder au même fichier qui accède à un client NFS immédiatement après un basculement planifié, le client NFS risquez de perdre ses connexions (basculement transparent ne seraient pas réussir).

## Améliorations de déploiement et la facilité de gestion

Déploiement et de gestion NFS a amélioré sur les points suivants:

- Plus de 40 nouvelles applets de commande Windows PowerShell facilitent configurer et gérer les partages de fichiers NFS. Pour plus d’informations, voir [NFS des applets de commande dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).
- Mappage d’identité est améliorée avec un stockage de mappage des fichiers plate local et nouvelles applets de commande Windows PowerShell pour configurer le mappage d’identité.
- Le Gestionnaire de serveur d’interface utilisateur graphique est plus facile à utiliser.
- Le nouveau fournisseur de version 2 WMI est disponible pour faciliter la gestion.
- Le port multiplexage (port RPC 2049) est compatible avec les pare-feu et simplifie le déploiement de NFS.

## Informations sur le Gestionnaire de serveur

Dans le Gestionnaire de serveur - ou la plus récente de [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) - utiliser l’ajout de rôles et fonctionnalités Assistant pour ajouter le serveur pour le service de rôle NFS (sous les fichiers et iSCSI rôle Services). Pour obtenir des informations générales sur l’installation de fonctionnalités, voir [installer ou désinstaller des rôles, Services de rôle ou fonctionnalités](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831809(v=ws.11)>). NFS outils de serveur pour inclure les Services pour le composant logiciel enfichable MMC de système de fichiers réseau pour gérer le serveur pour NFS et Client pour NFS. Vous utilisez le composant logiciel enfichable, vous pouvez gérer le serveur pour les composants NFS installée sur l’ordinateur. Serveur pour NFS contient également plusieurs outils d’administration de ligne de commande de Windows:

- **Montez** monte un partage NFS distant (également appelé une exportation) en local et il correspond à une lettre de lecteur local sur l’ordinateur client Windows.
- **Nfsadmin** gère les paramètres de configuration du serveur pour NFS et le Client pour les composants NFS.
- **Nfsshare** configure les paramètres de partage NFS pour les dossiers qui sont partagés à l’aide de serveur pour NFS.
- **Nfsstat appartient** affiche ou réinitialise les statistiques d’appels reçus par serveur pour NFS.
- **Showmount** affiche les systèmes de fichiers monté exportés par serveur pour NFS.
- **Unmount** supprime les lecteurs montés NFS.

NFS dans Windows Server 2012 introduit le module NFS pour Windows PowerShell avec plusieurs nouvelles applets de commande spécifiquement pour NFS. Ces applets de commande fournissent un moyen simple pour automatiser les tâches de gestion NFS. Pour plus d’informations, voir [NFS des applets de commande dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps).

## Informations complémentaires

Le tableau suivant fournit des ressources supplémentaires pour l’évaluation NFS.

|Type de contenu|Références|
|---|---|
|Déploiement|[Déployer le système de gestion de fichiers en réseau](deploy-nfs.md)|
|Opérations|[Applets de commande NFS dans Windows PowerShell](https://docs.microsoft.com/powershell/module/nfs/?view=win10-ps)|
|Technologies associées|[Stockage dans WindowsServer](../storage.md)|