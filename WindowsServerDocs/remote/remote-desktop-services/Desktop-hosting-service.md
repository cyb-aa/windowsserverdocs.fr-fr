---
title: Service d’hébergement de postes de travail
description: Décrit les composants d’un service d’hébergement de bureaux.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: lizross
ms.openlocfilehash: 64e433ed379ca322996bcfe2d0ddd513e074b85e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854712"
---
# <a name="desktop-hosting-service"></a>Service d’hébergement de postes de travail

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Cet article vous en dit plus sur les composants du service d’hébergement de bureaux.

## <a name="tenant-environment"></a>Environnement de locataire

Comme décrit dans l’article dédié aux [rôles Services Bureau à distance](rds-roles.md), chaque rôle occupe une place distincte dans l’environnement de locataire.

Le service d’hébergement de bureaux du fournisseur est implémenté en tant qu’ensemble d’environnements de locataire isolés. L’environnement de chaque client se compose d’un conteneur de stockage, d’un ensemble de machines virtuelles et d’une combinaison de services Azure, qui communiquent tous sur un réseau virtuel isolé. Chaque machine virtuelle contient un ou plusieurs des composants qui constituent l’environnement de bureau hébergé du locataire. Les sous-sections suivantes décrivent les composants qui constituent l’environnement de bureau hébergé de chaque locataire.

## <a name="active-directory-domain-services"></a>Services de domaine Active Directory

Active Directory Domain Services (AD DS) fournit les informations de domaine et de forêt, de façon que les utilisateurs de locataire puissent se connecter aux bureaux et applications pour exécuter leurs charges de travail. Cela vous permet également de configurer les bases de données et partages de fichiers nécessaires, ou de vous y connecter, comme exigé par les applications Windows.

La forêt du locataire n’exige pas de relation d’approbation avec la forêt de gestion du fournisseur. Un compte d’administrateur de domaine peut être configuré dans le domaine du locataire pour autoriser le personnel technique du fournisseur à effectuer des tâches d’administration dans l’environnement du locataire (par exemple, la surveillance de l’état du système et l’application de mises à jour logicielles) et pour faciliter la résolution des problèmes et la configuration.

Plusieurs méthodes de déploiement d’AD DS s’offrent à vous :

1. Activer Azure Active Directory Domain Services dans un environnement de réseau virtuel du locataire. Cela permettra de créer une instance d’AD DS gérée pour le locataire en fonction des utilisateurs et groupes qui existent dans Azure AD.
2. Configurer un serveur AD DS autonome dans l’environnement de réseau virtuel du locataire. Cela vous offre l’intégralité du contrôle de l’instance AD DS s’exécutant sur des machines virtuelles.
3. Créer une connexion VPN de site à site à un serveur AD DS situé sur le site du locataire. Cela permet au locataire de se connecter à son instance AD DS existante et de réduire la duplication des utilisateurs, groupes, unités d’organisation, etc.

Pour plus d’informations, consultez les articles suivants :

* [Azure Active Directory Domain Services documentation](https://docs.microsoft.com/azure/active-directory-domain-services/) (Documentation sur Azure Active Directory Domain Services)
* [Guide d’architecture de référence pour l’hébergement de postes de travail pour Windows Server 2012 R2](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)
* [Création d’une connexion de site à site dans le portail Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

## <a name="sql-database"></a>Base de données SQL

Une base de données SQL hautement disponible est utilisée par le service Broker de connexion Bureau à distance pour stocker les informations de déploiement, telles que le mappage des connexions des utilisateurs actuels aux serveurs hôtes.

Plusieurs méthodes de déploiement d’une base de données SQL s’offrent à vous :

1. Créer une base de données SQL Azure dans l’environnement du locataire. Cela vous offre la fonctionnalité d’une base de données SQL redondante sans avoir à gérer les serveurs. Cela vous permet également de payer pour ce que vous consommez plutôt que d’investir dans l’infrastructure.
2. Créer un cluster SQL Server AlwaysOn. Cela vous permet d’exploiter l’infrastructure SQL Server existante et de contrôler en intégralité les instances de SQL Server.

Pour plus d’informations sur la configuration d’une infrastructure de base de données SQL hautement disponible, consultez les articles suivants :

* [Qu’est-ce que le service Azure SQL Database ?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview)
* [Informations de référence sur la création et la configuration des groupes de disponibilité Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server?view=sql-server-2017).
* [Ajouter le serveur du service Broker pour les connexions Bureau à distance au déploiement et configurer la haute disponibilité](rds-connection-broker-cluster.md).

## <a name="file-server"></a>Serveur de fichiers

Le serveur de fichiers utilise le protocole Server Message Block (SMB) 3.0 pour proposer des dossiers partagés. Ces dossiers partagés sont utilisés pour créer et stocker des fichiers de disque de profil utilisateur (.vhdx), afin de sauvegarder des données et de permettre aux utilisateurs de partager des données entre eux dans le service cloud du locataire.

La machine virtuelle utilisée pour déployer le serveur de fichiers doit avoir un disque de données Azure attaché et configuré avec des dossiers partagés. Les disques de données Azure utilisent la mise en cache via l’écriture, qui garantit que les écritures sur le disque ne seront pas effacées au redémarrage de la machine virtuelle.

Les petits locataires peuvent réduire les coûts en combinant le serveur de fichiers et le [rôle Gestionnaire de licences des services Bureau à distance](rds-roles.md#remote-desktop-licensing) sur une seule machine virtuelle dans l’environnement du locataire.

Pour plus d’informations, consultez les articles suivants :

* [Storage](../../storage/storage.md) (Stockage)
* [Attacher un disque de données managé à une machine virtuelle Windows à l’aide du portail Azure](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Fclassic%2Ftoc.json)

### <a name="user-profile-disks"></a>Disques de profil utilisateur

Les disques de profil utilisateur permettent aux utilisateurs d’enregistrer des fichiers et paramètres personnels quand ils sont connectés à une session sur un serveur d’hôte de session Bureau à distance dans une collection, puis d’avoir accès à ces mêmes paramètres et fichiers lorsqu’ils se connectent à un autre serveur [d’hôte de session Bureau à distance](rds-roles.md#remote-desktop-session-host) dans la collection. Quand l’utilisateur se connecte pour la première fois, le serveur de fichiers du locataire crée un disque de profil utilisateur qui est monté sur le serveur d’hôte de session Bureau à distance auquel l’utilisateur est actuellement connecté. Pour chaque connexion suivante, le disque de profil utilisateur est monté sur le serveur d’hôte de session Bureau à distance approprié, et est démonté à chaque déconnexion. Seul l’utilisateur peut accéder au contenu du disque du profil.