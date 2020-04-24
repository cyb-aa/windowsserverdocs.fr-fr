---
title: Comprendre l’environnement d’hébergement de bureaux
description: Vue d’ensemble d’un déploiement Services Bureau à distance à l’aide d’Azure IaaS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 8fdebcad1370e06c19752944e85363c714f1fbcd
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854692"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Comprendre l’environnement d’hébergement de bureaux

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Les informations suivantes décrivent les composants du service d’hébergement de bureaux.  
  
## <a name="tenant-environment"></a>Environnement de locataire  
Le service d’hébergement de bureaux du fournisseur est implémenté en tant qu’ensemble d’environnements de locataire isolés. L’environnement de chaque client se compose d’un conteneur de stockage, d’un ensemble de machines virtuelles et d’une combinaison de services Azure, qui communiquent tous sur un réseau virtuel isolé. Chaque machine virtuelle contient un ou plusieurs des composants qui constituent l’environnement de bureau hébergé du locataire. Les sous-sections suivantes décrivent les composants qui constituent l’environnement de bureau hébergé de chaque locataire.

## <a name="remote-desktop-services"></a>Services Bureau à distance
Dans un environnement d’hébergement de bureaux, les rôles Services Bureau à distance suivants sont installés entre différentes machines virtuelles :

  - Service Broker pour les connexions Bureau à distance
  - Passerelle des services Bureau à distance
  - Gestionnaire de licences des services Bureau à distance
  - Hôte de session Bureau à distance
  - Accès web au Bureau à distance

Pour obtenir une description complète de chacun de ces rôles et de la façon dont ils interagissent entre eux, veuillez consulter le document sur la [compréhension des rôles Services Bureau à distance](Understanding-RDS-roles.md).
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Active Directory Domain Services  
Il existe plusieurs façons de se connecter à Active Directory Domain Services (AD DS) et de le gérer pour un environnement d’hébergement de bureaux dans Azure :

1. Créer une machine virtuelle dans l’environnement du locataire qui exécute le rôle AD DS
2. Créer une connexion VPN de site à site avec l’environnement local du locataire pour utiliser une instance AD DS existante
3. Utiliser le rôle PaaS Azure AD Domain Services, qui crée un domaine sur le réseau virtuel du locataire en fonction de l’instance Azure Active Directory du locataire

Avec Services Bureau à distance, le locataire doit avoir une instance Active Directory pour gérer l’accès dans l’environnement, le stockage de profil utilisateur et la surveillance dans le déploiement. Lorsque vous utilisez l’instance AD DS standard (autre qu’Azure), les forêts du locataire n’exigent pas de relation d’approbation avec la forêt de gestion du fournisseur. Un compte d’administrateur de domaine peut être configuré dans le domaine du locataire pour autoriser le personnel technique du fournisseur à effectuer des tâches d’administration dans l’environnement du locataire (par exemple, la surveillance de l’état du système et l’application de mises à jour logicielles) et pour faciliter la résolution des problèmes et la configuration.  
    
Informations complémentaires :  
[Azure Active Directory Domain Services Documentation](https://azure.microsoft.com/documentation/services/active-directory-ds/) (Documentation sur Azure Active Directory Domain Services)  
[Install a new Active Directory forest on an Azure virtual network](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/) (Installer une nouvelle forêt Active Directory sur un réseau virtuel Azure)  
[Création d’une connexion de site à site dans le portail Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Base de données SQL Azure  
La base de données SQL Azure permet aux hébergeurs d’étendre leur déploiement Services Bureau à distance sans avoir à déployer ni gérer un cluster SQL Server Always-on complet. La base de données SQL Azure est utilisée par le service Broker de connexion Bureau à distance pour stocker les informations de déploiement, telles que le mappage des connexions des utilisateurs actuels aux serveurs d’hôte final. Comme d’autres services Azure, la base de données SQL Azure suit un modèle de consommation, idéal pour des déploiements de toute taille.   
  
Informations complémentaires :  
[Qu’est-ce que le service Azure SQL Database ?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Proxy d’application Azure Active Directory  
Le proxy d’application Azure Active Directory est un service fourni dans des références SKU payantes d’Azure Active Directory qui permettent aux utilisateurs de se connecter à des applications internes via le service de proxy inverse d’Azure. Ainsi, les points de terminaison de passerelle Bureau à distance et web Bureau à distance peuvent être masqués à l’intérieur du réseau virtuel, ce qui élimine la nécessité d’exposition à Internet par une adresse IP publique. Cela permet aux hébergeurs de condenser le nombre de machines virtuelles dans l’environnement d’un locataire tout en conservant un déploiement complet.
  
Informations complémentaires :  
[Enabling Azure AD Application Proxy](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/) (Activation du proxy d’application Azure AD)  
    
## <a name="file-server"></a>Serveur de fichiers  
Le serveur de fichiers fournit des dossiers partagés en utilisant le protocole Server Message Block (SMB) 3.0. Les dossiers partagés sont utilisés pour créer et stocker des fichiers de disque de profil utilisateur (.vhdx), pour sauvegarder des données et pour offrir aux utilisateurs un emplacement où partager des données avec d’autres utilisateurs dans le réseau virtuel du locataire.
  
La machine virtuelle utilisée pour déployer le serveur de fichiers doit avoir un disque de données Azure attaché et configuré avec des dossiers partagés. Les disques de données Azure utilisent la mise en cache via l’écriture qui garantit que les écritures sur le disque sont conservées après les redémarrages de la machine virtuelle.  
  
Pour les petits locataires, le coût peut être réduit en combinant le serveur de fichiers à la machine virtuelle exécutant les rôles de Gestionnaire de licences Bureau à distance et de service Broker de connexion Bureau à distance sur une seule machine virtuelle dans l’environnement du locataire.  
  
Informations complémentaires  
[Vue d’ensemble des services de stockage et de fichiers](https://technet.microsoft.com/library/hh831487.aspx)  
[Attacher un disque de données managé à une machine virtuelle Windows à l’aide du portail Azure](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Disques de profil utilisateur  
Les disques de profil utilisateur permettent aux utilisateurs d’enregistrer des fichiers et paramètres personnels quand ils sont connectés à une session sur un serveur d’hôte de session Bureau à distance dans une collection, puis d’avoir accès à ces mêmes paramètres et fichiers lorsqu’ils se connectent à un autre serveur d’hôte de session Bureau à distance dans la collection. Quand l’utilisateur se connecte pour la première fois, un disque de profil utilisateur est créé sur le serveur de fichiers du locataire, et ce disque est monté sur le serveur d’hôte de session Bureau à distance auquel l’utilisateur est connecté. Pour chaque connexion suivante, le disque de profil utilisateur est monté sur le serveur d’hôte de session Bureau à distance approprié, et est démonté à chaque déconnexion. Le contenu du disque de profil est uniquement accessible par cet utilisateur.  
  


