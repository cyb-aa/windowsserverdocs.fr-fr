---
title: Gérer les serveurs avec Windows Admin Center
description: Gérer les serveurs avec Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 04ade4a14272c7840b5036ca6ad5a3bf3d09bcf9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891150"
---
# <a name="manage-servers-with-windows-admin-center"></a>Gérer les serveurs avec Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>La gestion des ordinateurs Windows Server

Vous pouvez ajouter des serveurs individuels exécutant Windows Server 2012 ou version ultérieure à Windows Admin Center pour gérer le serveur avec un ensemble complet d’outils y compris les certificats, appareils, événements, processus, rôles et fonctionnalités, les mises à jour, les Machines virtuelles et bien plus encore.

![Écran de vue d’ensemble de connexion de serveur](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Ajout d’un serveur pour Windows Admin Center

Pour ajouter un serveur à Windows Admin Center :

1. Cliquez sur **+ ajouter** sous toutes les connexions.
2. Choisissez d’ajouter un **connexion au serveur**.
3. Tapez le nom du serveur et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cliquez sur **envoyer** se termine.

Le serveur sera ajouté à votre liste des connexions dans la page Vue d’ensemble. Cliquez dessus pour vous connecter au serveur.

> [!NOTE]
> Vous pouvez également ajouter [clusters de basculement](manage-failover-clusters.md) ou [clusters hyperconvergés](manage-hyper-converged.md) comme une connexion distincte dans Windows Admin Center.

## <a name="tools"></a>Outils

Les outils suivants sont disponibles pour les connexions au serveur :

| Tool | Description |
| ---- | ----------- |
| [Vue d’ensemble](#overview) | Afficher les détails du serveur et contrôler l’état du serveur |
| [Sauvegarde](#backup) | Afficher et configurer la sauvegarde Azure |  
| [Certificats](#certificates) | Afficher et modifier les certificats |
| [Conteneurs](#containers) | Afficher les conteneurs |
| [Appareils](#devices) | Afficher et modifier les appareils |
| [Événements](#events) | afficher les événements |
| [Fichiers](#files) | Parcourir les fichiers et dossiers |
| [Firewall](#firewall) | Afficher et modifier les règles de pare-feu |
| [Applications installées](#installed-apps) | Afficher et supprimer des applications installées |
| [Utilisateurs et groupes locaux](#local-users-and-groups) | Afficher et modifier les utilisateurs et groupes locaux |
| [Réseau](#network) | Afficher et modifier les périphériques réseau |
| [PowerShell](#powershell) | Interagir avec le serveur via PowerShell |
| [Processus](#processes) | Afficher et modifier les processus en cours d’exécution |
| [Registre](#registry) | Afficher et modifier les entrées de Registre |
| [Bureau à distance](#remote-desktop) | Interagir avec le serveur via le Bureau à distance |
| [Rôles et fonctionnalités](#roles-and-features) | Afficher et modifier les rôles et fonctionnalités |
| [Tâches planifiées](#scheduled-tasks) | Afficher et modifier les tâches planifiées |
| [Services](#services) | Afficher et modifier les services |
| [Stockage](#storage) | Afficher et modifier les périphériques de stockage |
| [Service de Migration de stockage](#storage-migration-service) | Migrer des serveurs et partages de fichiers sur Azure ou Windows Server 2019 |
| [Réplica de stockage](#storage-replica) | Utiliser le réplica de stockage pour gérer la réplication de stockage de serveur à serveur |
| [Informations système](#system-insights) | Système Insights vous permet de qu'optimiser connaître le fonctionnement de votre serveur. |
| [mises à jour](#updates) | Vue installé et rechercher les nouvelles mises à jour |
| [Machines virtuelles](manage-virtual-machines.md) | Afficher et gérer des machines virtuelles |
| [Commutateurs virtuels](#virtual-switches) | Afficher et gérer des commutateurs virtuels |

## <a name="overview"></a>Vue d'ensemble

**Vue d’ensemble** vous permet de voir l’état actuel de l’UC, de mémoire et de performances du réseau, également comme effectuer des opérations et modifier les paramètres sur un serveur ou un ordinateur cible.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans la vue d’ensemble du Gestionnaire de serveur :

- Afficher les détails de serveur
- Activité du processeur
- Afficher l’activité de mémoire
- Afficher l’activité du réseau
- Redémarrez le serveur
- Arrêter le serveur
- Activer les métriques de disque sur le serveur
- Modifier l’ID de l’ordinateur sur le serveur

[**Afficher les commentaires et les fonctionnalités proposées pour une vue d’ensemble du serveur**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="backup"></a>Secours

**Sauvegarde** permet de protéger votre serveur Windows à partir des altérations, les attaques ou de sinistres en sauvegardant votre serveur directement à Microsoft Azure.
[En savoir plus sur sauvegarde Azure.](https://aka.ms/windows-admin-center-backup)

[Fournir des commentaires pour la sauvegarde dans Windows Admin Center](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans la sauvegarde :

- Afficher une vue d’ensemble de votre état de la sauvegarde Azure
- Configurer la planification et des éléments de sauvegarde
- Démarrer ou arrêter un travail de sauvegarde
- Afficher l’historique des travaux de sauvegarde et l’état
- Afficher les points de récupération et de récupérer des données
- Supprimer les données de sauvegarde

## <a name="certificates"></a>Certificats

**Certificats** vous permet de gérer les magasins de certificats sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les certificats :

- Parcourir et rechercher des certificats existants
- Afficher les détails de certificat
- Exporter des certificats
- Renouveler les certificats
- Demander de nouveaux certificats
- Supprimer des certificats

[**Afficher les commentaires et les fonctionnalités proposées pour les certificats**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Conteneurs

**Conteneurs** vous permet d’afficher les conteneurs sur un hôte de conteneur Windows Server. Dans le cas d’un conteneur Windows Server Core en cours d’exécution, vous pouvez afficher les journaux des événements et accéder à l’interface CLI du conteneur.

[**Afficher les commentaires et les fonctionnalités proposées pour les conteneurs**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Appareils

**Appareils** vous permet de gérer des appareils connectés sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge sur les appareils :

- Périphériques de navigation et de recherche
- Afficher les détails du périphérique
- Désactiver un appareil
- Pilote de mise à jour sur un appareil

[**Afficher les commentaires et les fonctionnalités proposées pour les appareils**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="events"></a>Événements

**Événements** vous permet de gérer les journaux des événements sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les événements :

- Événements de navigation et de recherche
- Afficher les détails
- Effacer les événements du journal
- Exporter à partir du journal des événements

[**Afficher les commentaires et les fonctionnalités proposées pour les événements**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>Fichiers

**Fichiers** vous permet de gérer les fichiers et dossiers sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les fichiers :

- Parcourir les fichiers et dossiers
- Rechercher un fichier ou dossier
- Créer un nouveau dossier
- Supprimer un fichier ou dossier
- Télécharger un fichier ou dossier
- Télécharger un fichier ou un dossier
- Renommer un fichier ou dossier
- Extraire un fichier zip
- Afficher les propriétés de fichier ou dossier
- Gérer la File Server Resource Manager [quotas](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)
- Ajouter, modifier ou supprimer des partages de fichiers
- Modifier les autorisations d’accès sur les partages de fichiers

[**Afficher les commentaires et les fonctionnalités proposées pour les fichiers**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Pare-feu

**Pare-feu** vous permet de gérer les paramètres de pare-feu et les règles sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le pare-feu :

- Afficher une vue d’ensemble des paramètres de pare-feu
- Afficher les règles de pare-feu entrantes
- Afficher les règles de pare-feu de trafic sortant
- Rechercher des règles de pare-feu
- Afficher les détails de règle de pare-feu
- Créer une nouvelle règle de pare-feu
- Activer ou désactiver une règle de pare-feu
- Supprimer une règle de pare-feu
- Modifier les propriétés d’une règle de pare-feu

[**Afficher les commentaires et les fonctionnalités proposées pour le pare-feu**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>Applications installées

**Applications installées** vous permet de répertorier et de désinstaller l’application qui sont installés.

[**Afficher les commentaires et les fonctionnalités proposées pour les applications installées**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Utilisateurs et groupes locaux

**Les utilisateurs et groupes locaux** vous permet de gérer les groupes de sécurité et les utilisateurs qui existent localement sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans utilisateurs et groupes locaux :

- Afficher et rechercher des utilisateurs et groupes
- Créer un nouvel utilisateur ou un groupe
- Gérer l’appartenance de groupe d’un utilisateur
- Supprimer un utilisateur ou un groupe
- Modifier un mot de passe utilisateur
- Modifier les propriétés d’un utilisateur ou un groupe

[**Afficher des commentaires et des fonctionnalités proposées pour les utilisateurs et groupes locaux**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Network (Réseau)

**Réseau** vous permet de gérer les périphériques réseau et les paramètres sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le réseau :

- Parcourir et rechercher les cartes réseau existant
- Afficher les détails d’une carte réseau
- Modifier les propriétés d’une carte réseau
- Créer un [carte de réseau Azure (fonctionnalité en version préliminaire)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Afficher des commentaires et des fonctionnalités proposées pour le réseau**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** vous permet d’interagir avec un ordinateur ou un serveur via une session PowerShell.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans PowerShell :

- Créer une session PowerShell interactive sur le serveur
- Déconnectez la session PowerShell sur le serveur

[**Afficher des commentaires et des fonctionnalités proposées pour PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processus

**Processus** vous autorise à gérer les processus en cours d’exécution sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les processus :

- Parcourir et rechercher les processus en cours d’exécution
- Afficher les détails de processus
- Démarrer un processus
- Terminer un processus
- Créer un vidage de processus
- Rechercher des handles de processus

[**Afficher les commentaires et les fonctionnalités proposées pour les processus**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registre

**Registre** permet de gérer les clés de Registre et les valeurs sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le Registre :

- Parcourir les valeurs et clés de Registre
- Ajouter ou modifier les valeurs de Registre
- Supprimer les valeurs de Registre

[**Afficher les commentaires et les fonctionnalités proposées pour le Registre**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Bureau à distance

**Bureau à distance** vous permet d’interagir avec un ordinateur ou un serveur via une session de bureau interactive.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le Bureau à distance :

- Démarrer une session Bureau à distance interactive
- Déconnecter d’une session Bureau à distance
- Envoyer Ctrl + Alt + Suppr à une session Bureau à distance

[**Afficher les commentaires et les fonctionnalités proposées pour Bureau à distance**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Rôles et fonctionnalités

**Rôles et fonctionnalités** vous permet de gérer des rôles et fonctionnalités sur un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les rôles et fonctionnalités :

- Parcourir la liste des rôles et fonctionnalités sur un serveur
- Afficher les détails de rôle ou fonctionnalité
- Installer un rôle ou une fonctionnalité
- Supprimer un rôle ou une fonctionnalité

[**Afficher les commentaires et les fonctionnalités proposées pour les rôles et fonctionnalités**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>TÂCHES PLANIFIÉES

**Les tâches planifiées** vous autorise à gérer les tâches planifiées sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les tâches planifiées :

- Parcourir la bibliothèque du Planificateur de tâches
- Modifier les tâches planifiées
- Activer et désactiver les tâches planifiées
- Démarrer et arrêter des tâches planifiées
- Créer des tâches planifiées

[**Afficher les commentaires et les fonctionnalités proposées pour les tâches planifiées**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Services

**Services** vous permet de gérer les services sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les Services :

- Parcourir et rechercher des services sur un serveur
- Afficher les détails d’un service
- Démarrer un service
- Suspendre un service
- Modifier les propriétés d’un service

[**Afficher les commentaires et les fonctionnalités proposées pour les Services**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="storage"></a>Stockage

**Stockage** vous autorise à gérer les périphériques de stockage sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le stockage :

- Parcourir et rechercher les disques existants sur un serveur
- Afficher les détails de disque
- Créer un volume
- Initialisation d’un disque
- Créer, attacher et détacher un disque dur virtuel (VHD)
- Hors connexion d’un disque
- Formater un volume
- Redimensionnez un volume
- Modifier les propriétés d’un volume
- Supprimer un volume
- Installer la gestion de Quota

[**Afficher des commentaires et des fonctionnalités proposées pour le stockage**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Service de migration du stockage

**Service de Migration de stockage** vous permet de migrer les serveurs et partages vers Azure ou Windows Server 2019 de fichiers, sans nécessiter d’applications ou aux utilisateurs de modifier quoi que ce soit.
[Obtenir une vue d’ensemble du Service de Migration de stockage](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Service de Migration de stockage nécessite Windows Server 2019.

## <a name="storage-replica"></a>Réplica de stockage

Utilisez **le réplica de stockage** pour gérer la réplication de stockage de serveur à serveur.
[En savoir plus sur le réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Insights système

**Système Insights** introduit l’analytique prédictive en mode natif dans Windows Server pour aider à vous donner augmenté connaître le fonctionnement de votre serveur.
[Obtenir une vue d’ensemble du système Insights](http://aka.ms/systeminsights)

>[!NOTE]
>Système Insights nécessite Windows Server 2019.

## <a name="updates"></a>Mises à jour

**Mises à jour** vous permet de gérer Microsoft et/ou les mises à jour de Windows sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les mises à jour :

- Afficher disponibles de Windows ou de Microsoft Updates
- Afficher une liste d’historique de mise à jour
- Installer les mises à jour
- Consulter en ligne des mises à jour à partir de Microsoft Update
- Gérer [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management) intégration

[**Afficher des commentaires et des fonctionnalités proposées pour les mises à jour**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Ordinateurs virtuels

Consultez [gestion d’ordinateurs virtuels avec Windows Admin Center](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Commutateurs virtuels

**Commutateurs virtuels** permet de gérer les commutateurs virtuels Hyper-V sur un ordinateur ou serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les commutateurs virtuels :

- Parcourir et rechercher les commutateurs virtuels sur un serveur
- Créer un commutateur virtuel
- Renommer un commutateur virtuel
- Supprimer un commutateur virtuel existant
- Modifier les propriétés d’un commutateur virtuel

[**Afficher des commentaires et des fonctionnalités proposées pour les commutateurs virtuels**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
