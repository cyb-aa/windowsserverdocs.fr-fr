---
title: Service d’hébergement de postes de travail
description: Décrit les composants d’un service d’hébergement de bureau.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: adbb9fd69bc61d2e877cadb0484a4e42093f262a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840470"
---
# <a name="desktop-hosting-service"></a>Service d’hébergement de postes de travail

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cet article vous en dit plus sur le bureau hébergeant les composants de service.

## <a name="tenant-environment"></a>Environnement de client

Comme décrit dans [rôles des services Bureau à distance](rds-roles.md), chaque rôle joue un rôle distinct dans l’environnement client.

Service du fournisseur d’hébergement bureau est implémenté comme un ensemble d’environnements isolés de client. Les environnement de chaque client se compose d’un conteneur de stockage, un ensemble de machines virtuelles et une combinaison de services Azure, tous les de communiquer via un réseau virtuel isolé. Chaque machine virtuelle contient un ou plusieurs des composants qui constituent l’environnement du client de bureau hébergé. Les sous-sections suivantes décrivent les composants qui constituent l’environnement de chaque client de bureau hébergé.

## <a name="active-directory-domain-services"></a>Services de domaine Active Directory

Services de domaine Active Directory (AD DS) fournit les informations de domaine et de forêt, telles que les utilisateurs du locataire peuvent se connecter les ordinateurs de bureau et les applications pour exécuter leurs charges de travail. Cela vous permet également à configurer ou se connecter aux partages de fichiers requis et les bases de données qui peuvent être nécessaires pour les applications Windows.

Forêt du locataire ne nécessite pas de toute relation d’approbation avec la forêt d’administration du fournisseur. Un compte d’administrateur de domaine peut être défini dans le domaine du locataire pour autoriser du personnel technique du fournisseur pour effectuer des tâches d’administration dans l’environnement du client (par exemple, la surveillance de l’état du système et en appliquant des mises à jour logicielles) et pour aider à dépannage et la configuration.

Il existe plusieurs façons de déployer les services AD DS :

1. Activer Azure Active Directory Domain Services dans un environnement de réseau virtuel du locataire. Cela créera une instance d’AD DS managée pour le locataire selon les utilisateurs et groupes qui existent dans Azure AD.
2. Configurer un serveur AD DS autonome dans un environnement de réseau virtuel du locataire. Cela vous donne tous le contrôle total de l’instance d’AD DS en cours d’exécution sur des machines virtuelles.
3. Créer une connexion VPN de site à site à un serveur AD DS situé dans les locaux du client. Ainsi, le client pour se connecter à leur instance AD DS existante et de réduire la duplication des utilisateurs, groupes, unités d’organisation et ainsi de suite.

Pour plus d’informations, consultez les articles suivants :

* [Documentation des Services de domaine Active Directory Azure](https://docs.microsoft.com/azure/active-directory-domain-services/)
* [Hébergement du Guide d’Architecture de référence pour Windows Server 2012 R2 du bureau](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Créer une connexion de site à site dans le portail Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>base de données SQL

Une base de données SQL hautement disponible est utilisé par l’intermédiaire de connexion Bureau à distance pour stocker les informations de déploiement, telles que le mappage de connexions des utilisateurs actuels pour les serveurs hôtes.

Il existe plusieurs façons de déployer une base de données SQL :

1. Créer une base de données SQL Azure dans un environnement du locataire. Cela vous fournit les fonctionnalités d’une base de données SQL redondante sans avoir à gérer les serveurs eux-mêmes. Cela vous permet également de payer pour ce que vous consommez au lieu d’investir dans l’infrastructure.
2. Créer un cluster SQL Server AlwaysOn. Cela vous permet d’exploiter l’infrastructure existante de SQL Server et vous permet de contrôler totalement les instances de SQL Server.

Pour plus d’informations sur la façon de configurer une infrastructure de base de données SQL à haute disponibilité, consultez les articles suivants :

* [Qu’est le service de base de données SQL Azure ?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Création et configuration des groupes de disponibilité (SQL Server)](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Ajouter le serveur Service Broker pour les connexions Bureau à distance au déploiement et configurer la haute disponibilité](rds-connection-broker-cluster.md).

## <a name="file-server"></a>Serveur de fichiers

Le serveur de fichiers utilise le protocole Server Message Block (SMB) 3.0 pour fournir des dossiers partagés. Ces dossiers partagés sont utilisés pour créer et stocker des fichiers de disque de profil utilisateur (.vhdx) pour sauvegarder les données et permettre aux utilisateurs de partager des données entre eux au sein du service de cloud du locataire.

La machine virtuelle qui déploie le serveur de fichiers doit avoir un disque de données Azure attachés et configurés avec des dossiers partagés. Disques de données Azure utilisent écriture via la mise en cache, ce qui garantit que les écritures sur le disque ne sont pas effacés chaque fois que la machine virtuelle est redémarrée.

Petits locataires peuvent réduire les coûts en combinant le serveur de fichiers et [rôle Gestionnaire de licences bureau à distance](rds-roles.md#remote-desktop-licensing) sur une seule machine virtuelle dans un environnement du locataire.

Pour plus d’informations, consultez les articles suivants :

* [Stockage dans Windows Server](../../storage/storage.md)
* [Comment attacher un disque de données géré à une machine virtuelle Windows dans le portail Azure](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Disques de profil utilisateur

Disques de profil utilisateur permettent aux utilisateurs d’enregistrer des fichiers et paramètres personnels quand ils sont connectés à une session sur un serveur hôte de Session Bureau à distance dans une collection, puis les mêmes paramètres et fichiers d’accès lorsque vous vous connectez à un autre [hôte de Session Bureau à distance](rds-roles.md#remote-desktop-session-host) serveur de la collection. Lorsque l’utilisateur se connecte pour la première fois, le serveur de fichiers du client crée un disque de profil utilisateur obtient monté sur le serveur hôte de Session Bureau à distance par l’utilisateur actuellement connecté à. Pour chaque suivantes connectez-vous, le disque de profil utilisateur est monté sur le serveur hôte de Session Bureau à distance approprié, et il est démonté avec chaque déconnexion. Seul l’utilisateur peut accéder aux contenu du disque de profil.