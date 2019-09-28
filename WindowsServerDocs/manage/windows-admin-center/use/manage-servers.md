---
title: Gérer les serveurs à l’aide du centre d’administration Windows
description: Gérer les serveurs à l’aide du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 03/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c7f436ea9b2baa00294ccef52a5d7a27c7247e4a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406788"
---
# <a name="manage-servers-with-windows-admin-center"></a>Gérer les serveurs à l’aide du centre d’administration Windows

>S'applique à : Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center ?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## <a name="managing-windows-server-machines"></a>Gestion des ordinateurs Windows Server

Vous pouvez ajouter des serveurs individuels exécutant Windows Server 2012 ou une version ultérieure au centre d’administration Windows pour gérer le serveur à l’aide d’un ensemble complet d’outils, notamment des certificats, des appareils, des événements, des processus, des rôles, des fonctionnalités, des mises à jour, des machines virtuelles, etc.

![Écran de présentation de la connexion au serveur](../media/manage-servers/server-overview.png)

## <a name="adding-a-server-to-windows-admin-center"></a>Ajout d’un serveur au centre d’administration Windows

Pour ajouter un serveur au centre d’administration Windows :

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisissez d’ajouter une **connexion au serveur**.
3. Tapez le nom du serveur et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cliquez sur **Envoyer** pour terminer.

Le serveur sera ajouté à votre liste de connexions sur la page vue d’ensemble. Cliquez dessus pour vous connecter au serveur.

> [!NOTE]
> Vous pouvez également ajouter des [clusters de basculement](manage-failover-clusters.md) ou des [clusters hyper-convergent](manage-hyper-converged.md) comme connexion distincte dans le centre d’administration Windows.

## <a name="tools"></a>Outils

Les outils suivants sont disponibles pour les connexions au serveur :

| Tool | Description |
| ---- | ----------- |
| [Vue d’ensemble](#overview) | Afficher les détails du serveur et contrôler l’état du serveur |
| [Active Directory](#active-directory-preview) | Gérer les Active Directory |
| [Sauvegarde](#backup) | Afficher et configurer la sauvegarde Azure |  
| [Certificates](#certificates) | Afficher et modifier des certificats |
| [Recipie](#containers) | Afficher les conteneurs |
| [Appareils](#devices) | Afficher et modifier des appareils |
| [DHCP](#dhcp) | Afficher et gérer la configuration du serveur DHCP |
| [DNS](#dns) | Afficher et gérer la configuration du serveur DNS |
| [Événements](#events) | Visualiser les événements |
| [Fichiers](#files) | Parcourir les fichiers et les dossiers |
| [Pare](#firewall) | Afficher et modifier des règles de pare-feu |
| [Applications installées](#installed-apps) | Afficher et supprimer les applications installées |
| [Utilisateurs et groupes locaux](#local-users-and-groups) | Afficher et modifier les utilisateurs et groupes locaux |
| [Network](#network) | Afficher et modifier des périphériques réseau |
| [PowerShell](#powershell) | Interagir avec le serveur via PowerShell |
| [Processus](#processes) | Afficher et modifier les processus en cours d’exécution |
| [Du](#registry) | Afficher et modifier des entrées de Registre |
| [Bureau à distance](#remote-desktop) | Interagir avec le serveur via Bureau à distance |
| [Rôles et fonctionnalités](#roles-and-features) | Afficher et modifier des rôles et des fonctionnalités |
| [Tâches planifiées](#scheduled-tasks) | Afficher et modifier des tâches planifiées |
| [Services](#services) | Afficher et modifier des services |
| [Réglages](#settings) | Afficher et modifier des services |
| [Stockage](#storage) | Afficher et modifier les périphériques de stockage |
| [Service de migration de stockage](#storage-migration-service) | Migrer des serveurs et des partages de fichiers vers Azure ou Windows Server 2019 |
| [Réplica de stockage](#storage-replica) | Utiliser le réplica de stockage pour gérer la réplication de stockage de serveur à serveur |
| [Insights système](#system-insights) | System Insights vous offre une meilleure compréhension du fonctionnement de votre serveur. |
| [Mises à jour](#updates) | Affichage installé et recherche de nouvelles mises à jour |
| [Machines virtuelles](manage-virtual-machines.md) | Afficher et gérer des machines virtuelles |
| [Commutateurs virtuels](#virtual-switches) | Afficher et gérer les commutateurs virtuels |

## <a name="overview"></a>Vue d'ensemble

La **vue d’ensemble** vous permet de voir l’état actuel de l’UC, de la mémoire et des performances réseau, ainsi que d’effectuer des opérations et de modifier les paramètres sur un ordinateur ou serveur cible.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans Gestionnaire de serveur vue d’ensemble :

- Afficher les détails du serveur
- Afficher l’activité de l’UC
- Afficher l’activité de la mémoire
- Afficher l’activité réseau
- Redémarrer le serveur
- Arrêter le serveur
- Activer les métriques de disque sur le serveur
- Modifier l’ID d’ordinateur sur le serveur
- Affichez l’adresse IP du contrôleur BMC avec le lien hypertexte (nécessite un contrôleur BMC compatible IPMI).

[**Affichez les commentaires et les fonctionnalités proposées pour la vue d’ensemble du serveur**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## <a name="active-directory-preview"></a>Active Directory (version préliminaire)

**Active Directory** est une version préliminaire qui est disponible dans le [flux d’extension](../configure/using-extensions.md).

### <a name="features"></a>Fonctionnalités

La gestion des Active Directory suivante est disponible :

- Créer un utilisateur
- Créer un groupe
- Rechercher des utilisateurs, des ordinateurs et des groupes
- Volet d’informations pour les utilisateurs, les ordinateurs et les groupes lorsqu’ils sont sélectionnés dans la grille
- Actions de grille globale utilisateurs, ordinateurs et groupes (désactiver/activer, supprimer)
- Réinitialiser le mot de passe de l’utilisateur
- Objets utilisateur : configurer des propriétés de base & les appartenances aux groupes
- Objets ordinateur : configurer la délégation sur un seul ordinateur
- Objets de groupe : gérer l’appartenance (ajouter/supprimer 1 utilisateur à la fois)  

[**Affichez les commentaires et les fonctionnalités proposées pour Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## <a name="backup"></a>Sauvegarde

La **sauvegarde** vous permet de protéger votre serveur Windows contre les altérations, les attaques ou les catastrophes en sauvegardant votre serveur directement sur Microsoft Azure.
[En savoir plus sur sauvegarde Azure.](https://aka.ms/windows-admin-center-backup)

[Fournir des commentaires pour la sauvegarde dans le centre d’administration Windows](https://aka.ms/backup-wac-feedback)

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans la sauvegarde :

- Afficher une vue d’ensemble de l’état de la sauvegarde Azure
- Configurer les éléments et la planification de sauvegarde
- Démarrer ou arrêter un travail de sauvegarde
- Afficher l’historique et l’état des travaux de sauvegarde
- Afficher les points de récupération et récupérer les données
- Supprimer les données de sauvegarde

## <a name="certificates"></a>Certificats

Les **certificats** vous permettent de gérer des magasins de certificats sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les certificats :

- Parcourir et Rechercher des certificats existants
- Afficher les détails du certificat
- Exporter des certificats
- Renouveler les certificats
- Demander de nouveaux certificats
- Supprimer des certificats

[**Affichez les commentaires et les fonctionnalités proposées pour les certificats**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## <a name="containers"></a>Containers

Les **conteneurs** vous permettent d’afficher les conteneurs sur un hôte de conteneur Windows Server. Dans le cas d’un conteneur Windows Server Core en cours d’exécution, vous pouvez afficher les journaux des événements et accéder à l’interface CLI du conteneur.

[**Affichez les commentaires et les fonctionnalités proposées pour les conteneurs**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## <a name="devices"></a>Appareils

Les **appareils** vous permettent de gérer les appareils connectés sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les appareils :

- Parcourir et Rechercher des appareils
- Afficher les détails de l’appareil
- Désactiver un appareil
- Mettre à jour le pilote sur un appareil

[**Affichez les commentaires et les fonctionnalités proposées pour les appareils**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## <a name="dhcp"></a>DHCP

**DHCP** vous permet de gérer les appareils connectés sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

- Créer/configurer/afficher des étendues IPV4 et IPV6
- Créer des exclusions d’adresses et configurer l’adresse IP de début et de fin
- Créer des réservations d’adresses et configurer l’adresse MAC du client (IPV4), DUID et IAID (IPV6)

[**Affichez les commentaires et les fonctionnalités proposées pour DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## <a name="dns"></a>DNS

**DNS** vous permet de gérer les appareils connectés sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

- Afficher les détails des zones de recherche directe DNS, des zones de recherche inversée et des enregistrements DNS
- Créer des zones de recherche directe (principal, secondaire ou stub) et configurer les propriétés de la zone de recherche directe
- Créer un hôte (A ou AAAA), CNAMe ou MX types d’enregistrements DNS
- Configurer les propriétés des enregistrements DNS
- Créer des zones de recherche inversée IPV4 et IPV6 (principale, secondaire et stub), configurer les propriétés de la zone de recherche inversée
- Créez le type PTR, CNAMe des enregistrements DNS sous la zone de recherche inversée.

[**Affichez les commentaires et les fonctionnalités proposées pour DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## <a name="events"></a>Events

Les **événements** vous permettent de gérer les journaux des événements sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les événements :

- Parcourir et Rechercher des événements
- Afficher les détails de l’événement
- Effacer les événements du journal
- Exporter les événements à partir du journal

[**Affichez les commentaires et les fonctionnalités proposées pour les événements**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## <a name="files"></a>Fichiers

**Fichiers** vous permet de gérer des fichiers et des dossiers sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les fichiers :

- Parcourir les fichiers et les dossiers
- Rechercher un fichier ou un dossier
- Créer un nouveau dossier
- Supprimer un fichier ou un dossier
- Télécharger un fichier ou un dossier
- Charger un fichier ou un dossier
- Renommer un fichier ou un dossier
- Extraire un fichier zip
- Afficher les propriétés d’un fichier ou d’un dossier
- Ajouter, modifier ou supprimer des partages de fichiers
- Modifier les autorisations d’accès sur les partages de fichiers

[**Affichez les commentaires et les fonctionnalités proposées pour les fichiers**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## <a name="firewall"></a>Pare-feu

Le **pare-feu** vous permet de gérer les paramètres du pare-feu et les règles sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le pare-feu :

- Afficher une vue d’ensemble des paramètres de pare-feu
- Afficher les règles de pare-feu entrantes
- Afficher les règles de pare-feu sortantes
- Rechercher dans les règles de pare-feu
- Afficher les détails de la règle de pare-feu
- Créer une règle de pare-feu
- Activer ou désactiver une règle de pare-feu
- Supprimer une règle de pare-feu
- Modifier les propriétés d’une règle de pare-feu

[**Affichez les commentaires et les fonctionnalités proposées pour le pare-feu**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## <a name="installed-apps"></a>Applications installées

Les **applications installées** vous permettent de répertorier et de désinstaller les applications qui sont installées.

[**Affichez les commentaires et les fonctionnalités proposées pour les applications installées**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## <a name="local-users-and-groups"></a>Utilisateurs et groupes locaux

**Utilisateurs et groupes locaux** vous permet de gérer des groupes de sécurité et des utilisateurs qui existent localement sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans utilisateurs et groupes locaux :

- Afficher et Rechercher des utilisateurs et des groupes
- Créer un nouvel utilisateur ou un nouveau groupe
- Gérer l’appartenance aux groupes d’un utilisateur
- Supprimer un utilisateur ou un groupe
- Modifier le mot de passe d’un utilisateur
- Modifier les propriétés d’un utilisateur ou d’un groupe

[**Afficher les commentaires et les fonctionnalités proposées pour les utilisateurs et les groupes locaux**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## <a name="network"></a>Réseau

Le **réseau** vous permet de gérer les périphériques et les paramètres réseau sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le réseau :

- Parcourir et Rechercher des cartes réseau existantes
- Afficher les détails d’une carte réseau
- Modifier les propriétés d’une carte réseau
- Créer une [carte réseau Azure (fonctionnalité en version préliminaire)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Afficher les commentaires et les fonctionnalités proposées pour le réseau**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## <a name="powershell"></a>PowerShell

**PowerShell** vous permet d’interagir avec un ordinateur ou un serveur par le biais d’une session PowerShell.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans PowerShell :

- Créer une session PowerShell interactive sur le serveur
- Se déconnecter de la session PowerShell sur le serveur

[**Afficher les commentaires et les fonctionnalités proposées pour PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## <a name="processes"></a>Processus

Les **processus** vous permettent de gérer les processus en cours d’exécution sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les processus :

- Parcourir et rechercher les processus en cours d’exécution
- Afficher les détails du processus
- Démarrer un processus
- Terminer un processus
- Créer un vidage de processus
- Rechercher les handles de processus

[**Affichez les commentaires et les fonctionnalités proposées pour les processus**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## <a name="registry"></a>Registre

Le **Registre** vous permet de gérer les clés et les valeurs de Registre sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le registre :

- Parcourir les clés et les valeurs de Registre
- Ajouter ou modifier des valeurs de Registre
- Supprimer les valeurs de Registre

[**Affichez les commentaires et les fonctionnalités proposées pour le registre**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## <a name="remote-desktop"></a>Bureau à distance

**Bureau à distance** vous permet d’interagir avec un ordinateur ou un serveur via une session de bureau interactive.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans Bureau à distance :

- Démarrer une session de bureau à distance interactive
- Se déconnecter d’une session Bureau à distance
- Envoyer Ctrl + Alt + Suppr à une session Bureau à distance

[**Affichez les commentaires et les fonctionnalités proposées pour bureau à distance**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## <a name="roles-and-features"></a>Rôles et fonctionnalités

**Rôles et fonctionnalités** vous permet de gérer des rôles et des fonctionnalités sur un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les rôles et les fonctionnalités :

- Parcourir la liste des rôles et des fonctionnalités sur un serveur
- Afficher les détails des rôles ou des fonctionnalités
- Installer un rôle ou une fonctionnalité
- Supprimer un rôle ou une fonctionnalité

[**Affichez les commentaires et les fonctionnalités proposées pour les rôles et les fonctionnalités**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## <a name="scheduled-tasks"></a>TÂCHES PLANIFIÉES

Les **tâches planifiées** vous permettent de gérer les tâches planifiées sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les tâches planifiées :

- Parcourir la bibliothèque du planificateur de tâches
- Modifier les tâches planifiées
- Activer & désactiver les tâches planifiées
- Démarrer & arrêter les tâches planifiées
- Créer des tâches planifiées

[**Affichez les commentaires et les fonctionnalités proposées pour les tâches planifiées**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## <a name="services"></a>Services

**Services** vous permet de gérer des services sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les services :

- Parcourir et Rechercher des services sur un serveur
- Afficher les détails d’un service
- Démarrer un service
- Suspendre un service
- Modifier les propriétés d’un service

[**Affichez les commentaires et les fonctionnalités proposées pour les services**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## <a name="settings"></a>Paramètres

**Settings** est un emplacement central pour gérer les paramètres d’un ordinateur ou d’un serveur.

### <a name="features"></a>Fonctionnalités

- Afficher et modifier les variables d’environnement utilisateur et système
- Afficher la configuration de la surveillance des alertes à partir de [Azure Monitor](azure-monitor.md)
- Afficher et modifier la configuration de l’alimentation
- Afficher et modifier les paramètres de Bureau à distance
- Afficher et modifier les paramètres de contrôle d’accès en fonction du rôle
- Afficher et modifier les paramètres de l’ordinateur hôte Hyper-V, le cas échéant

## <a name="storage"></a>Stockage

Le **stockage** vous permet de gérer les périphériques de stockage sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le stockage :

- Parcourir et Rechercher des disques existants sur un serveur
- Afficher les détails du disque
- Créer un volume
- Initialiser un disque
- Créer, attacher et détacher un disque dur virtuel (VHD)
- Mettre un disque hors connexion
- Mettre en forme un volume
- Redimensionner un volume
- Modifier les propriétés du volume
- Supprimer un volume
- Installer la gestion de quota
- Gérer le serveur de fichiers quotas [de gestionnaire des ressources stockage-> créer/mettre à jour le quota](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management)

[**Afficher les commentaires et les fonctionnalités proposées pour le stockage**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## <a name="storage-migration-service"></a>Service de migration du stockage

Le **service de migration du stockage** vous permet de migrer des serveurs et des partages de fichiers vers Azure ou Windows Server 2019, sans que les applications ou les utilisateurs aient besoin de modifier quoi que ce soit.
[Avoir une vue d’ensemble du service de migration de stockage](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Le service de migration de stockage nécessite Windows Server 2019.

## <a name="storage-replica"></a>Réplica de stockage

Utilisez le **réplica de stockage** pour gérer la réplication de stockage de serveur à serveur.
[En savoir plus sur le réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## <a name="system-insights"></a>Insights système

**System Insights** introduit une analyse prédictive en mode natif dans Windows Server pour vous permettre de mieux comprendre le fonctionnement de votre serveur.
[Avoir une vue d’ensemble de System Insights](http://aka.ms/systeminsights)

>[!NOTE]
>System Insights requiert Windows Server 2019.

## <a name="updates"></a>Mises à jour

Les **mises à jour** vous permettent de gérer les mises à jour Microsoft et/ou Windows sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les mises à jour :

- Afficher les mises à jour Windows ou Microsoft disponibles
- Afficher la liste de l’historique des mises à jour
- Installer les mises à jour
- Rechercher en ligne les mises à jour de Microsoft Update
- Gérer l’intégration d' [Azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Afficher les commentaires et les fonctionnalités proposées pour les mises à jour**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## <a name="virtual-machines"></a>Virtual Machines

Consultez [gestion des machines virtuelles avec le centre d’administration Windows](manage-virtual-machines.md)

## <a name="virtual-switches"></a>Commutateurs virtuels

Les **commutateurs virtuels** vous permettent de gérer des commutateurs virtuels Hyper-V sur un ordinateur ou un serveur.

### <a name="features"></a>Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les commutateurs virtuels :

- Parcourir et Rechercher des commutateurs virtuels sur un serveur
- Créer un commutateur virtuel
- Renommer un commutateur virtuel
- Supprimer un commutateur virtuel existant
- Modifier les propriétés d’un commutateur virtuel

[**Afficher les commentaires et les fonctionnalités proposées pour les commutateurs virtuels**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
