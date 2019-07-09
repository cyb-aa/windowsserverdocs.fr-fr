---
title: Rôles Services Bureau à distance
description: Décrit les composants d’un service d’hébergement de bureaux.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 717f6c714ce95793a9a7b5fcf0ddd304c77daa32
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63753176"
---
# <a name="remote-desktop-services-roles"></a>Rôles Services Bureau à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Cet article décrit les rôles au sein d’un environnement Services Bureau à distance.

## <a name="remote-desktop-session-host"></a>Hôte de session Bureau à distance

L’hôte de session Bureau à distance contient les applications basées sur une session et les bureaux que vous partagez avec des utilisateurs. Les utilisateurs accèdent à ces bureaux et applications via l’un des clients Bureau à distance qui s’exécutent sur Windows, MacOS, iOS et Android. Les utilisateurs peuvent également se connecter via un navigateur pris en charge en utilisant le client web.

Vous pouvez organiser les bureaux et applications dans un ou plusieurs hôtes de session Bureau à distance, appelés « collections ». Vous pouvez personnaliser ces collections pour des groupes spécifiques d’utilisateurs au sein de chaque locataire. Par exemple, vous pouvez créer une collection dans laquelle un groupe d’utilisateurs spécifique peut accéder à certaines applications, tandis que toutes les personnes en dehors du groupe que vous aurez désignées n’y auront pas accès.

Pour les petits déploiements, vous pouvez installer des applications directement sur les serveurs d’hôte de session Bureau à distance. Pour les déploiements plus importants, nous vous recommandons de créer une image de base et d’approvisionner les machines virtuelles à partir de cette image.

Vous pouvez développer des collections en ajoutant des machines virtuelles de serveur d’hôte de session Bureau à distance à une batterie de serveurs de collection avec chaque machine virtuelle d’hôte de session Bureau à distance dans une collection affectée au même groupe de disponibilité. Cela permet de bénéficier d’une plus grande disponibilité pour la collection et d’augmenter l’échelle pour prendre en charge plus d’utilisateurs ou d’applications gourmandes en ressources.

Dans la plupart des cas, plusieurs utilisateurs partagent le même serveur d’hôte de session Bureau à distance, qui utilise plus efficacement les ressources Azure pour une solution d’hébergement de bureaux. Dans cette configuration, les utilisateurs doivent se connecter à des collections avec des comptes non administratifs. Vous pouvez également donner à certains utilisateurs un accès administratif complet à leur bureau à distance en créant des collections de bureaux de session personnels.

Vous pouvez pousser plus loin la personnalisation des bureaux en créant et chargeant un disque dur virtuel avec le système d’exploitation Windows Server que vous pouvez utiliser comme modèle pour la création de machines virtuelles d’hôte de session Bureau à distance.

Pour plus d’informations, consultez les articles suivants :

* [Remote Desktop Services - Secure data storage with UPDs](rds-plan-secure-data-storage.md) (Services Bureau à distance - Sécuriser les données de stockage avec UPD)
* [Charger un disque dur virtuel généralisé et l’utiliser pour créer des machines virtuelles dans Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [Update RDSH collection (modèle ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Service Broker pour les connexions Bureau à distance

Le service Broker de connexion Bureau à distance gère les connexions de bureau à distance entrantes aux batteries de serveurs d’hôte de session Bureau à distance. Le service Broker de connexion Bureau à distance gère les connexions des collections des bureaux complets et celles des applications à distance. Le service Broker de connexion Bureau à distance peut équilibrer la charge sur les serveurs de la collection lors de l’établissement de nouvelles connexions. Si une session se déconnecte, le service Broker de connexion Bureau à distance reconnecte l’utilisateur au serveur d’hôte de session Bureau à distance correct et à sa session interrompue, qui existe toujours dans la batterie de serveurs d’hôte de session Bureau à distance.

Vous devez installer la mise en correspondance des certificats numériques sur le service Broker de connexion Bureau à distance et le client pour prendre en charge l’authentification unique et la publication d’applications. Lors du développement ou du test d’un réseau, vous pouvez utiliser un certificat auto-signé et auto-généré. Toutefois, les services publiés nécessitent un certificat numérique d’une autorité de certification approuvée. Le nom que vous donnez au certificat doit être le même que le nom de domaine complet interne et la machine virtuelle du service Broker de connexion Bureau à distance.

Vous pouvez installer le service Broker de connexion Bureau à distance Windows Server 2016 sur la même machine virtuelle qu’AD DS pour réduire les coûts. Si vous avez besoin de monter en charge pour plus d’utilisateurs, vous pouvez également ajouter des machines virtuelles du service Broker de connexion Bureau à distance au même groupe de disponibilité, afin de créer un cluster de service Broker Connexion Bureau à distance.

Avant de pouvoir créer un cluster de service Broker de connexion Bureau à distance, vous devez déployer une base de données SQL Azure dans l’environnement du locataire ou créer un groupe de disponibilité AlwaysOn SQL Server.

Pour plus d’informations, consultez les articles suivants :

* [Add the RD Connection Broker server to the deployment and configure high availability](rds-connection-broker-cluster.md) (Ajouter le serveur du service Broker de connexion Bureau à distance au déploiement et configurer la haute disponibilité)
* [Base de données SQL](desktop-hosting-service.md#sql-database) dans le service d’hébergement de bureaux.

## <a name="remote-desktop-gateway"></a>Passerelle des services Bureau à distance

La passerelle Bureau à distance accorde aux utilisateurs sur les réseaux publics un accès aux bureaux Windows et aux applications hébergées dans les services cloud de Microsoft Azure.

Le composant de passerelle Bureau à distance utilise Secure Sockets Layer (SSL) pour chiffrer le canal de communication entre les clients et le serveur. La machine virtuelle de passerelle Bureau à distance doit être accessible via une adresse IP publique qui autorise les connexions TCP entrantes vers le port 443 et les connexions UDP entrantes vers le port 3 391. Cela permet aux utilisateurs de se connecter via Internet en utilisant le protocole de transport de communication HTTPS et le protocole UDP, respectivement.

Les certificats numériques installés sur le serveur et client doivent correspondre pour que cela fonctionne. Lors du développement ou du test d’un réseau, vous pouvez utiliser un certificat auto-signé et auto-généré. Toutefois, un service publié nécessite un certificat d’une autorité de certification approuvée. Le nom du certificat doit correspondre au nom de domaine complet utilisé pour accéder à la passerelle Bureau à distance, que le nom de domaine complet soit le nom DNS externe de l’adresse IP publique ou l’enregistrement DNS CNAME qui pointe vers l’adresse IP publique.

Pour les locataires avec moins d’utilisateurs, les rôles de passerelle Bureau à distance et d’accès web Bureau à distance peuvent être associés sur une seule machine virtuelle pour réduire les coûts. Vous pouvez également ajouter plus de machines virtuelles de passerelle Bureau à distance à une batterie de serveurs de passerelle Bureau à distance pour accroître la disponibilité du service et effectuer la montée en charge pour plus d’utilisateurs. Les machines virtuelles dans des batteries de serveurs de passerelle Bureau à distance doivent être configurées dans un ensemble dont la charge est équilibrée. L’affinité d’adresses IP n’est pas nécessaire lorsque vous utilisez la passerelle Bureau à distance sur une machine virtuelle Windows Server 2016, mais elle l’est lorsque vous l’exécutez sur une machine virtuelle Windows Server 2012 R2.

Pour plus d’informations, consultez les articles suivants :

* [Add high availability to the RD Web and Gateway web front](rds-rdweb-gateway-ha.md) (Ajouter la haute disponibilité au serveur frontal d’accès web et de passerelle Bureau à distance)
* [Services Bureau à distance - Accès en tout lieu](rds-plan-access-from-anywhere.md)
* [Remote Desktop Services - Multi-Factor Authentication](rds-plan-mfa.md) (Services Bureau à distance - Authentification multifacteur)

## <a name="remote-desktop-web-access"></a>Accès web au Bureau à distance

L’accès web au Bureau à distance permet aux utilisateurs d’accéder à des bureaux et applications via un portail web et de les lancer via une application de client Bureau à distance Microsoft native de l’appareil. Vous pouvez utiliser le portail web pour publier des applications et bureaux Windows sur des appareils clients Windows et autres, et vous pouvez également publier des bureaux ou applications pour des utilisateurs ou groupes spécifiques.

L’accès web Bureau à distance a besoin d’Internet Information Services (IIS) pour fonctionner correctement. Une connexion Hypertext Transfer Protocol Secure (HTTPS) fournit un canal de communication chiffré entre les clients et le serveur web Bureau à distance. La machine virtuelle d’accès web Bureau à distance doit être accessible via une adresse IP publique qui autorise les connexions TCP entrantes vers le port 443 pour permettre aux utilisateurs du locataire de se connecter à partir d’Internet à l’aide du protocole de transport de communication HTTPS.

Les certificats numériques correspondants doivent être installés sur le serveur et les clients. Pour le développement et le test, un certificat auto-signé et auto-généré peut être utilisé. Pour un service publié, le certificat numérique doit être obtenu auprès d’une autorité de certification approuvée. Le nom du certificat doit correspondre au nom de domaine complet utilisé pour l’accès web Bureau à distance. Les noms de domaine complets incluent le nom DNS externe pour l’adresse IP publique et l’enregistrement DNS CNAME pointant vers l’adresse IP publique.

Pour les locataires avec moins d’utilisateurs, vous pouvez réduire les coûts en combinant les charges de travail de passerelle Bureau à distance et d’accès web Bureau à distance dans une seule machine virtuelle. Vous pouvez également ajouter des machines virtuelles d’accès web Bureau à distance à une batterie de serveurs d’accès web Bureau à distance pour accroître la disponibilité du service et effectuer la montée en charge pour plus d’utilisateurs. Dans une batterie de serveurs d’accès web Bureau à distance avec plusieurs machines virtuelles, vous devrez configurer les machines virtuelles dans un ensemble dont la charge est équilibrée.

Pour plus d’informations sur la façon de configurer l’accès web Bureau à distance, consultez les articles suivants :

* [Configurer le client web de Bureau à distance pour vos utilisateurs](clients/remote-desktop-web-client-admin.md)
* [Créer et déployer une collection de services Bureau à distance](rds-create-collection.md)
* [Créer une collection de Services Bureau à distance pour les ordinateurs de bureau et des applications à exécuter](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Gestionnaire de licences des services Bureau à distance

Les serveurs du Gestionnaire de licences des services Bureau à distance activé permettent aux utilisateurs de se connecter aux serveurs d’hôte de session Bureau à distance hébergeant les applications et bureaux du locataire. Les environnements de locataire sont généralement fournis avec le serveur du Gestionnaire de licences des services Bureau à distance déjà installé, mais pour les environnements hébergés, vous devrez configurer le serveur en mode par utilisateur.

Le fournisseur de services a besoin de suffisamment de licences d’accès abonné Services Bureau à distance pour couvrir tous les utilisateurs uniques (non simultanés) autorisés à se connecter au service chaque mois. Les fournisseurs de services peuvent acheter des services d’infrastructure Microsoft Azure directement et des licences d’accès abonnés via le programme Microsoft SPLA (Service Provider Licensing Agreement). Les clients qui souhaitent une solution de bureau hébergée doivent acheter la solution hébergée complète (Azure et Services Bureau à distance) auprès du fournisseur de service.

Les petits locataires peuvent réduire les coûts en combinant le serveur de fichiers et les composants du Gestionnaire de licences des services Bureau à distance sur une seule machine virtuelle. Pour assurer une meilleure disponibilité de service, les locataires peuvent déployer deux machines virtuelles de serveur de licences Bureau à distance dans le même groupe de disponibilité. Tous les serveurs Bureau à distance dans l’environnement du locataire sont associés à des serveurs de licence Bureau à distance pour que les utilisateurs puissent continuer à se connecter à de nouvelles sessions même si l’un des serveurs tombe en panne.

Pour plus d’informations, consultez les articles suivants :

* [Gérer les licences de votre déploiement Services Bureau à distance avec des licences d’accès client (CAL)](rds-client-access-license.md)
* [Activer le serveur de licences des Services Bureau à distance](rds-activate-license-server.md)
* [Effectuer le suivi de vos licences d’accès client des Services Bureau à distance (RDS CAL)](rds-track-cals.md)
* [Licensing options for service providers](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx) (Options de gestion des licences pour les fournisseurs de services)