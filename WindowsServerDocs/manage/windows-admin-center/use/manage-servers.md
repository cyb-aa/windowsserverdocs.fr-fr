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
ms.openlocfilehash: 93a40345d05a6230596832b2d455d36eee2401b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296681"
---
# Gérer les serveurs avec Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Vous débutez dans Windows Admin Center?
> [Découvrez-en davantage sur Windows Admin Center](../understand/windows-admin-center.md) ou [téléchargez maintenant](https://aka.ms/windowsadmincenter).

## La gestion de Windows Server

Vous pouvez ajouter des serveurs exécutant Windows Server 2012 ou version ultérieure au centre d’administration de Windows pour gérer le serveur avec un ensemble complet d’outils y compris des certificats, appareils, événements, processus, rôles et fonctionnalités, mises à jour, Machines virtuelles et bien plus encore.

![Écran de vue d’ensemble de connexion de serveur](../media/manage-servers/server-overview.png)

## Ajout d’un serveur pour Windows Admin Center

Pour ajouter un serveur à Windows Admin Center:

1. Cliquez sur **+ Ajouter** sous toutes les connexions.
2. Choisir d’ajouter une **Connexion au serveur**.
3. Tapez le nom du serveur et, si vous y êtes invité, les informations d’identification à utiliser.
4. Cliquez sur **Envoyer** pour terminer.

Le serveur sera ajouté à votre liste de connexion sur la page de présentation. Cliquez dessus pour se connecter au serveur.

> [!NOTE]
> Vous pouvez également ajouter des [clusters de basculement](manage-failover-clusters.md) ou des [clusters hyperconvergés](manage-hyper-converged.md) comme une connexion distincte dans Windows Admin Center.

## Outils

Les outils suivants sont disponibles pour les connexions du serveur:

| Outil | Description |
| ---- | ----------- |
| [Vue d'ensemble](#overview) | Afficher les détails du serveur et de contrôler l’état de serveur |
| [Active Directory](#active-directory-preview) | Gérer Active Directory |
| [Sauvegarde](#backup) | Afficher et configurer la sauvegarde Azure |  
| [Certificats](#certificates) | Afficher et modifier les certificats |
| [Conteneurs](#containers) | Conteneurs de vue |
| [Appareils](#devices) | Afficher et modifier des appareils |
| [DHCP](#dhcp) | Afficher et gérer la configuration du serveur DHCP |
| [DNS](#dns) | Afficher et gérer la configuration du serveur DNS |
| [Événements](#events) | Afficher les événements |
| [Fichiers](#files) | Parcourir les fichiers et dossiers |
| [Pare-feu](#firewall) | Afficher et modifier les règles de pare-feu |
| [Applications installées](#installed-apps) | Afficher et supprimer des applications installées |
| [Utilisateurs et groupes locaux](#local-users-and-groups) | Afficher et modifier des utilisateurs et groupes locaux |
| [Réseau](#network) | Afficher et modifier les périphériques réseau |
| [PowerShell](#powershell) | Interagir avec le serveur à l’aide de PowerShell |
| [Processus](#processes) | Afficher et modifier les processus en cours d’exécution |
| [Registre](#registry) | Afficher et modifier les entrées de Registre |
| [Bureau à distance](#remote-desktop) | Interagir avec le serveur à l’aide des services Bureau à distance |
| [Rôles et fonctionnalités](#roles-and-features) | Afficher et modifier les rôles et fonctionnalités |
| [Tâches planifiées](#scheduled-tasks) | Afficher et modifier des tâches planifiées |
| [Services](#services) | Afficher et modifier des services |
| [Paramètres](#settings) | Afficher et modifier des services |
| [Stockage](#storage) | Afficher et modifier les dispositifs de stockage |
| [Service de migration du stockage](#storage-migration-service) | Migrer des serveurs et des partages de fichiers à Azure ou Windows Server 2019 |
| [Réplica de stockage](#storage-replica) | Le réplica de stockage permet de gérer la réplication de stockage de serveur à serveur |
| [Insights système](#system-insights) | Système Insights vous permet de qu'optimiser des informations sur le fonctionnement de votre serveur. |
| [Mises à jour](#updates) | Vue installé et vérifier les nouvelles mises à jour |
| [Machines virtuelles](manage-virtual-machines.md) | Afficher et gérer des machines virtuelles |
| [Commutateurs virtuels](#virtual-switches) | Afficher et gérer des commutateurs virtuels |

## Vue d'ensemble

**Vue d’ensemble** permet de vous permet d’afficher l’état actuel de processeur, mémoire et performances du réseau, mais aussi effectuent des opérations et modifier les paramètres sur un serveur ou un ordinateur cible.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans la vue d’ensemble du Gestionnaire de serveur:

- Afficher les détails de serveur
- Activité de vue du processeur
- Afficher les activités de mémoire
- Afficher l’activité réseau
- Puis redémarrer le serveur
- Arrêter le serveur
- Activer les métriques de disque sur le serveur
- Modifiez l’ID d’ordinateur sur serveur
- Permet d’afficher l’adresse IP de BMC avec un lien hypertexte (nécessite BMC IPMI compatibles).

[**Commentaires de vue et des fonctionnalités proposées pour une vue d’ensemble du serveur**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BOverview%5D).

## Active Directory (version d’évaluation)

**Active Directory** est une version préliminaire qui est disponible sur le [flux d’extension](../configure/using-extensions.md).

### Fonctionnalités

La gestion d’Active Directory suivante sont disponibles:

- Création d’utilisateur
- Créer le groupe
- Rechercher des utilisateurs, des ordinateurs et des groupes
- Volet d’informations pour les utilisateurs, les ordinateurs et les groupes lorsqu’il est sélectionné dans la grille
- Global Grid actions que les utilisateurs, les ordinateurs et les groupes (Activer/désactiver, supprimer)
- Réinitialiser le mot de passe de l’utilisateur
- Objets utilisateur: configurer les appartenances à des propriétés de base &
- Objets ordinateur: configurer la délégation à un ordinateur unique
- Objets de groupe: gérer l’appartenance (Ajouter/supprimer des 1 utilisateur à la fois)  

[**Commentaires de vue et des fonctionnalités proposées pour Active Directory**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BActive%20Directory%5D).

## Sauvegarde

**Sauvegarde** vous permet de protéger votre serveur Windows contre les endommagements, des attaques ou des catastrophes par sauvegarder votre serveur directement à Microsoft Azure.
[En savoir plus sur Azure sauvegarde.](https://aka.ms/windows-admin-center-backup)

[Fournir des commentaires pour la sauvegarde dans Windows Admin Center](https://aka.ms/backup-wac-feedback)

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge de sauvegarde:

- Afficher une vue d’ensemble de votre état de la sauvegarde Azure
- Configurer des éléments de sauvegarde et de planification
- Démarrer ou arrêter un travail de sauvegarde
- Afficher l’historique de sauvegarde et l’état
- Afficher les points de récupération et récupérer des données
- Supprimer les données de sauvegarde

## Certificats

**Certificats** vous permet de gérer les magasins de certificats sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les certificats:

- Parcourir et rechercher des certificats existants
- Afficher les détails du certificat
- Exporter des certificats
- Renouveler les certificats
- Demander de nouveaux certificats
- Supprimer des certificats

[**Commentaires de vue et des fonctionnalités proposées pour les certificats**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BCertificates%5D).

## Conteneurs

**Conteneurs** vous permet d’afficher les conteneurs sur un hôte de conteneur Windows Server. Dans le cas d’un conteneur Windows Server Core en cours d’exécution, vous pouvez afficher les journaux des événements et accéder à l’interface CLI du conteneur.

[**Commentaires de vue et des fonctionnalités proposées pour les conteneurs**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BContainers%5D).

## Appareils

**Appareils** vous permet de vous permet de gérer les appareils connectés sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge des appareils:

- Périphériques de navigation et de recherche
- Afficher les détails du périphérique
- Désactivation d’un périphérique
- Pilote de mise à jour sur un appareil

[**Commentaires de vue et des fonctionnalités proposées pour les appareils**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDevices%5D).

## DHCP

**DHCP** vous permet de gérer les appareils connectés sur un ordinateur ou un serveur.

### Fonctionnalités

- Créer/configurer/mode IPV4 et IPV6 étendues
- Créez des exclusions de l’adresse et configurer l’adresse IP de début et fin
- Créer des réservations d’adresse et configurer l’adresse MAC client (IPV4), DUID et IAID (IPV6)

[**Commentaires de vue et des fonctionnalités proposées pour DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDHCP%5D).

## DNS

**DNS** vous permet de gérer les appareils connectés sur un ordinateur ou un serveur.

### Fonctionnalités

- Afficher les détails des zones de recherche directe DNS, les zones de recherche inversée et les enregistrements DNS
- Créer des zones de recherche directes (principal, secondaire ou stub) et configurer les propriétés de zone de recherche directe
- Créer l’hôte (A ou AAAA), CNAME ou MX le type d’enregistrements DNS
- Configurer les propriétés des enregistrements DNS
- Créer des zones IPV4 et IPV6 inversée (principal, secondaire et un stub), configurer les propriétés de zone de recherche inversée
- Créer PTR, type CNAME de DNS enregistre sous la zone de recherche inversée.

[**Commentaires de vue et des fonctionnalités proposées pour DHCP**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BDNS%5D).

## Événements

**Événements** vous permet de gérer les journaux des événements sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les événements:

- Événements de navigation et de recherche
- Affichage Détails de l’événement
- Effacer les événements à partir du journal
- Exporter à partir du journal des événements

[**Commentaires de vue et de fonctionnalités proposées aux événements**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BEvents%5D).

## Fichiers

**Fichiers** vous permet de gérer les fichiers et dossiers sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les fichiers:

- Parcourir les fichiers et dossiers
- Recherchez un fichier ou un dossier
- Créez un dossier
- Supprimer un fichier ou un dossier
- Télécharger un fichier ou un dossier
- Charger un fichier ou un dossier
- Renommer un fichier ou un dossier
- Extraire un fichier zip
- Afficher les propriétés de fichier ou un dossier
- Gérer les [quotas](https://docs.microsoft.com/windows-server/storage/fsrm/quota-management) de la File Server Resource Manager
- Ajouter, modifier ou supprimer des partages de fichiers
- Modifier les autorisations d’accès sur les partages de fichiers

[**Commentaires de vue et des fonctionnalités proposées pour les fichiers**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFiles%5D).

## Pare-feu

**Pare-feu** vous permet de gérer les paramètres de pare-feu et les règles sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le pare-feu:

- Afficher une vue d’ensemble des paramètres du pare-feu
- Afficher les règles de pare-feu entrantes
- Afficher les règles de pare-feu de trafic sortant
- Règles de pare-feu de recherche
- Afficher les détails de règle de pare-feu
- Créer une nouvelle règle de pare-feu
- Activer ou désactiver une règle de pare-feu
- Supprimer une règle de pare-feu
- Modifiez les propriétés d’une règle de pare-feu

[**Commentaires de vue et des fonctionnalités proposées pour le pare-feu**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BFirewall%5D).

## Applications installées

**Applications installées** permet de vous permettant de répertorier et de désinstaller l’application qui sont installées.

[**Commentaires de vue et des fonctionnalités proposées pour les applications installées**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BInstalled%20Apps%5D).

## Utilisateurs et groupes locaux

**Utilisateurs et groupes locaux** vous permet de gérer des groupes de sécurité et les utilisateurs qui existent localement sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les utilisateurs et groupes locaux:

- Afficher et rechercher des utilisateurs et groupes
- Créer un nouvel utilisateur ou un groupe
- Gérer l’appartenance à un groupe d’un utilisateur
- Supprimer un utilisateur ou un groupe
- Modifier le mot de passe d’un utilisateur
- Modifiez les propriétés d’un utilisateur ou un groupe

[**Afficher les commentaires et les fonctionnalités proposées pour les utilisateurs et groupes locaux**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BLocal%20users%20and%20Groups%5D)

## Réseau

**Réseau** vous permet de gérer les périphériques réseau et les paramètres sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge de réseau:

- Parcourir et rechercher des cartes réseau existant
- Afficher les détails d’une carte réseau
- Modifier les propriétés d’une carte réseau
- Créer une [Carte de réseau Azure (fonctionnalité d’aperçu)](https://blogs.technet.microsoft.com/networking/2018/09/05/azurenetworkadapter/)

[**Afficher les commentaires et les fonctionnalités proposées pour réseau**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BNetwork%5D)

## PowerShell

**PowerShell** vous permet d’interagir avec un ordinateur ou un serveur à l’aide d’une session PowerShell.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans PowerShell:

- Créer une session PowerShell interactive sur le serveur
- Déconnectez la session PowerShell sur le serveur

[**Afficher les commentaires et les fonctionnalités proposées pour PowerShell**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BPowerShell%5D)

## Processus

**Processus** vous permet de gérer les processus en cours d’exécution sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans des processus:

- Parcourir et rechercher les processus en cours d’exécution
- Afficher les détails de processus
- Démarrer un processus
- Mettre fin à un processus
- Créer un vidage du processus
- Recherchez les descripteurs de processus

[**Commentaires de vue et des fonctionnalités proposées pour les processus**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BProcesses%5D).

## Registre

**Registre** vous permet de gérer les clés de Registre et des valeurs sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le Registre:

- Parcourir les valeurs et les clés de Registre
- Ajouter ou modifier des valeurs de Registre
- Supprimer les valeurs de Registre

[**Commentaires de vue et des fonctionnalités proposées pour le Registre**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRegistry%5D).

## Bureau à distance

**Services Bureau à distance** vous permet d’interagir avec un ordinateur ou un serveur à l’aide d’une session de bureau interactive.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les services Bureau à distance:

- Démarrer une session interactive de bureau à distance
- Se déconnecter à partir d’une session Bureau à distance
- Envoyer Ctrl + Alt + Suppr à une session Bureau à distance

[**Commentaires de vue et des fonctionnalités proposées pour le Bureau à distance**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRemote%20Desktop%5D).

## Rôles et fonctionnalités

**Rôles et des fonctionnalités** permet de vous permettent de gérer des rôles et fonctionnalités sur un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les rôles et des fonctionnalités:

- Parcourir la liste des rôles et fonctionnalités sur un serveur
- Afficher les détails de rôle ou fonctionnalité
- Installer un rôle ou fonctionnalité
- Supprimer un rôle ou fonctionnalité

[**Commentaires de vue et des fonctionnalités proposées pour les rôles et fonctionnalités**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BRoles%20and%20Features%5D).

## Tâches planifiées

**Des tâches planifiées** vous permet de gérer les tâches planifiées sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les tâches planifiées:

- Parcourir la bibliothèque du Planificateur de tâches
- Modifier les tâches planifiées
- Activer les tâches planifiée de désactiver &
- Démarrer les tâches d’arrêt planifié &
- Créer des tâches planifiées

[**Commentaires de vue et des fonctionnalités proposées pour les tâches planifiées**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BScheduled%20Tasks%5D).

## Services

**Services** vous permet de gérer les services sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les Services de:

- Services de navigation et de recherche sur un serveur
- Afficher les détails d’un service
- Démarrer un service
- Suspendre un service
- Modifiez les propriétés d’un service

[**Commentaires de vue et des fonctionnalités proposées pour les Services**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BServices%5D).

## Paramètres

**Paramètres** est un emplacement central pour gérer les paramètres sur un ordinateur ou un serveur.

### Fonctionnalités

- Afficher et modifier les variables d’environnement utilisateur et système
- Afficher la configuration pour l’analyse des alertes à partir du [Moniteur Azure](azure-monitor.md)
- Afficher et modifier la configuration d’alimentation
- Afficher et modifier les paramètres de bureau à distance
- Afficher et modifier les paramètres de contrôle d’accès basé sur un rôle
- Afficher et modifier les paramètres de l’hôte Hyper-V, le cas échéant

## Stockage

**Stockage** vous permet de gérer les périphériques de stockage sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans le stockage:

- Parcourir et rechercher des disques existants sur un serveur
- Afficher les détails du disque
- Créer un volume
- Initialisation d’un disque
- Créer, attacher et détacher un disque dur virtuel (VHD)
- Mettre un disque hors connexion
- Formater un volume
- Redimensionner un volume
- Modifier les propriétés du volume
- Supprimer un volume
- Installer la gestion de Quota

[**Afficher les commentaires et les fonctionnalités proposées pour le stockage**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BStorage%5D)

## Service de migration du stockage

**Service de Migration du stockage** permet de vous permettent de migrer des serveurs et les partages de fichiers pour Azure ou Windows Server 2019, sans avoir besoin d’applications ou aux utilisateurs de modifier quoi que ce soit.
[Obtenir une vue d’ensemble du Service de Migration du stockage](https://go.microsoft.com/fwlink/?linkid=2016155)

>[!NOTE]
>Service de Migration du stockage nécessite Windows Server 2019.

## Réplica de stockage

Utiliser **Le réplica de stockage** pour gérer la réplication du stockage de serveur à serveur.
[En savoir plus sur le réplica de stockage](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui)

## Insights système

**Insights système** introduit prédictive analytique en mode natif dans Windows Server pour aider à vous donner augmenté des informations sur le fonctionnement de votre serveur.
[Obtenir une vue d’ensemble de Insights système](http://aka.ms/systeminsights)

>[!NOTE]
>Insights système nécessite Windows Server 2019.

## Mises à jour

**Mises à jour** vous permet de gérer Microsoft et/ou des mises à jour Windows sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les mises à jour:

- Afficher disponible Windows ou Microsoft Updates
- Afficher une liste de l’historique des mises à jour
- Installer les mises à jour
- Vérifier en ligne si des mises à jour à partir de Microsoft Update
- Gérer l’intégration de la [Gestion des mises à jour Azure](https://docs.microsoft.com/azure/automation/automation-update-management)

[**Afficher les commentaires et les fonctionnalités proposées des mises à jour**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BUpdates%5D)

## Machines virtuelles

Reportez-vous à [la gestion des Machines virtuelles avec Windows Admin Center](manage-virtual-machines.md)

## Commutateurs virtuels

**Commutateurs virtuels** vous permet de gérer les commutateurs virtuels Hyper-V sur un ordinateur ou un serveur.

### Fonctionnalités

Les fonctionnalités suivantes sont prises en charge dans les commutateurs virtuels:

- Parcourir et rechercher des commutateurs virtuels sur un serveur
- Créer un commutateur virtuel
- Renommer un commutateur virtuel
- Supprimer un commutateur virtuel existant
- Modifiez les propriétés d’un commutateur virtuel

[**Afficher les commentaires et les fonctionnalités proposées pour commutateurs virtuels**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5BVirtual%20Switch%5D)
