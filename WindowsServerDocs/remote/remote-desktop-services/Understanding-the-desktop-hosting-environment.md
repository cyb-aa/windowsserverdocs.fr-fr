---
title: Présentation de l’environnement d’hébergement bureau
description: Vue d’ensemble d’un deployhment de services Bureau à distance à l’aide d’Azure IaaS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 880fc8f9fa2db5ec56d2117e02c069650c61584a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877920"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Présentation de l’environnement d’hébergement bureau

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les informations suivantes décrivent les composants du bureau qui héberge le service.  
  
## <a name="tenant-environment"></a>Environnement de client  
Service du fournisseur d’hébergement bureau est implémenté comme un ensemble d’environnements isolés de client. Les environnement de chaque client se compose d’un conteneur de stockage, un ensemble de machines virtuelles et une combinaison de services Azure, tous les de communiquer via un réseau virtuel isolé. Chaque machine virtuelle contient un ou plusieurs des composants qui constituent l’environnement du client de bureau hébergé. Les sous-sections suivantes décrivent les composants qui constituent l’environnement de chaque client de bureau hébergé.

## <a name="remote-desktop-services"></a>Services Bureau à distance
Dans un environnement d’hébergement bureau, les rôles Services Bureau à distance suivants sont installés entre différentes machines virtuelles :

  - Service Broker pour les connexions Bureau à distance
  - Passerelle des services Bureau à distance
  - Gestionnaire de licences bureau à distance
  - Hôte de session Bureau à distance
  - Accès Web Bureau à distance

Pour obtenir une description complète de chacune de ces rôles et la façon dont ils interagissent entre eux, veuillez consulter la [les rôles Services Bureau à distance de compréhension](Understanding-RDS-roles.md) document.
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Services de domaine Active Directory  
Il existe plusieurs façons de se connecter à et gérer des Services de domaine Active Directory (AD DS) pour un environnement d’hébergement bureau dans Azure :

1. Créer une machine virtuelle dans l’environnement du client qui exécute le rôle AD DS
2. Créer une connexion VPN de site à site avec environnement de local du locataire à utiliser un existant AD DS
3. Utilisez le rôle Azure AD Domain Services PaaS, ce qui crée un domaine sur le réseau virtuel du locataire en fonction Azure Active Directory du locataire

Services Bureau à distance, le client doit avoir un annuaire Active Directory pour gérer l’accès dans l’environnement, de stockage de profil utilisateur et la surveillance dans le déploiement. Lorsque vous utilisez le service d’annuaire AD (non-Azure) standard, les forêts du locataire ne nécessitent pas de toute relation d’approbation avec la forêt d’administration du fournisseur. Un compte d’administrateur de domaine peut être défini dans le domaine du locataire pour autoriser du personnel technique du fournisseur pour effectuer des tâches d’administration dans l’environnement du client (par exemple, la surveillance de l’état du système et en appliquant des mises à jour logicielles) et pour aider à dépannage et la configuration.  
    
Informations complémentaires :  
[Documentation Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Installer une nouvelle forêt Active Directory sur un réseau virtuel Azure](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[Créer une ressource Gestionnaire de réseau virtuel avec une connexion VPN de Site à l’aide du portail Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Base de données SQL Azure  
Base de données SQL Azure permet aux hébergeurs d’étendre leur déploiement des Services Bureau à distance sans avoir à déployer et maintenir un cluster SQL Server Always-on complet. La base de données SQL Azure est utilisé par l’intermédiaire de connexion Bureau à distance pour stocker les informations de déploiement, telles que le mappage de connexions des utilisateurs actuels aux serveurs hôtes de fin. Comme d’autres services Azure, Azure SQL DB suit un modèle de consommation, idéal pour le déploiement de toute taille.   
  
Informations complémentaires :  
[Qu’est la base de données SQL ?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Proxy d’Application Azure Active Directory  
Azure Active Directory Application Proxy est un service fourni dans payé-références (SKU) d’Azure Active Directory qui permettent aux utilisateurs pour se connecter à des applications internes via le service de proxy inverse d’Azure. Ainsi, les points de terminaison Web de bureau à distance et passerelle Bureau à distance être masquées à l’intérieur du réseau virtuel, en éliminant la nécessité d’être exposée à internet via une adresse IP publique. Cela permet plus aux hébergeurs de condenser le nombre de machines virtuelles dans un environnement du locataire tout en conservant un déploiement complet.
  
Informations complémentaires :  
[Activation du Proxy d’Application Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>Serveur de fichiers  
Le serveur de fichiers fournit des dossiers partagés en utilisant le protocole Server Message Block (SMB) 3.0. Les dossiers partagés sont utilisés pour créer et stocker des fichiers de disque de profil utilisateur (.vhdx), pour sauvegarder des données et pour permettre aux utilisateurs de partager des données avec d’autres utilisateurs dans le réseau virtuel du locataire.
  
La machine virtuelle utilisée pour déployer le serveur de fichiers doit avoir un disque de données Azure attachés et configurés avec des dossiers partagés. Disques de données Azure utilisent écriture via la mise en cache qui conservent des garanties qui écrit sur le disque après un redémarrage de la machine virtuelle.  
  
Pour les petits locataires, le coût peut être réduit en combinant le serveur de fichiers avec la machine virtuelle exécutant les rôles de service Broker pour les connexions Bureau à distance et le Gestionnaire de licences bureau à distance sur une seule machine virtuelle dans un environnement du locataire.  
  
Informations complémentaires  
[Présentation des Services de stockage et de fichier](https://technet.microsoft.com/library/hh831487.aspx)  
[Comment attacher un disque de données à une Machine virtuelle](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Disques de profil utilisateur  
Disques de profil utilisateur permettent aux utilisateurs d’enregistrer des fichiers et paramètres personnels quand ils sont connectés à une session sur un serveur hôte de Session Bureau à distance dans une collection et accéder aux paramètres et aux fichiers même lorsque vous vous connectez à un autre serveur hôte de Session Bureau à distance dans la collection. Lorsque l’utilisateur se connecte pour la première fois, un disque de profil utilisateur est créé sur le serveur de fichiers du client, et ce disque est monté sur le serveur hôte de Session Bureau à distance à laquelle l’utilisateur est connecté. Pour chaque suivantes connectez-vous, le disque de profil utilisateur est monté sur le serveur hôte de Session Bureau à distance approprié, et avec chaque déconnexion, il est non monté. Le contenu du disque de profil est uniquement accessible par l’utilisateur.  
  


