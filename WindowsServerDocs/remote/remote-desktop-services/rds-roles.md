---
title: Rôles de Services Bureau à distance
description: Décrit les composants d’un service d’hébergement de bureau.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 48efbffa4f82b707b63e33e6416da43eb105221f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844720"
---
# <a name="remote-desktop-services-roles"></a>Rôles de Services Bureau à distance

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cet article décrit les rôles au sein d’un environnement de Services Bureau à distance.

## <a name="remote-desktop-session-host"></a>Hôte de session Bureau à distance

L’hôte de Session Bureau à distance (RD Session Host) contient les applications basées sur session et le partage avec d’autres ordinateurs de bureau. Les utilisateurs obtiennent à ces ordinateurs de bureau et les applications via un des clients Bureau à distance qui s’exécutent sur Windows, MacOS, iOS et Android. Les utilisateurs peuvent également se connecter via un navigateur pris en charge en utilisant le client web.

Vous pouvez organiser les ordinateurs de bureau et des applications dans un ou plusieurs serveurs hôtes de Session Bureau à distance, appelés « collections ». Vous pouvez personnaliser ces regroupements pour des groupes spécifiques d’utilisateurs au sein de chaque client. Par exemple, vous pouvez créer une collection dans laquelle un groupe d’utilisateurs spécifique peut accéder à des applications spécifiques, mais tout le monde en dehors du groupe que vous avez désigné ne sera pas en mesure d’accéder à ces applications.

Pour les déploiements de petite taille, vous pouvez installer des applications directement sur les serveurs hôtes de Session Bureau à distance. Pour les déploiements plus importants, nous vous recommandons de créer une image de base et configuration des machines virtuelles à partir de cette image.

Vous pouvez développer des collections en ajoutant des machines virtuelles de serveur hôte de Session Bureau à distance à une batterie de serveurs de collection avec chaque machine virtuelle RDSH dans une collection affectée au même groupe à haute disponibilité. Cela fournit une plus grande disponibilité de la collection et augmente l’échelle pour prendre en charge de plusieurs utilisateurs ou applications comportant beaucoup de ressources.

Dans la plupart des cas, plusieurs utilisateurs partagent le même serveur hôte de Session Bureau à distance, qui utilise plus efficacement les ressources Azure pour une solution d’hébergement bureau. Cette configuration, les utilisateurs doivent se connecter à collections avec des comptes non administratifs. Vous pouvez également fournir certains utilisateurs un accès administratif complet leur bureau à distance en créant des collections de bureaux de session personnelle.

Vous pouvez personnaliser davantage par la création et téléchargement d’un disque dur virtuel avec le système d’exploitation Windows Server que vous pouvez utiliser comme modèle pour la création d’ordinateurs virtuels hôtes de Session Bureau à distance de postes de travail.

Pour plus d’informations, consultez les articles suivants :

* [Services Bureau à distance - stockage sécurisé des données](rds-plan-secure-data-storage.md)
* [Charger un disque dur virtuel généralisé et l’utiliser pour créer de nouvelles machines virtuelles dans Azure](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [Mettre à jour RDSH, collection (modèle ARM)](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>Service Broker pour les connexions Bureau à distance

Broker de connexion Bureau à distance (RD Connection Broker) gère les connexions Bureau à distance entrantes aux batteries de serveurs hôte de Session Bureau à distance. Service Broker pour les connexions Bureau à distance gère les connexions pour les deux collections de bureaux complète et les collections d’applications à distance. RD Connection Broker peut équilibrer la charge sur les serveurs de la collection lors de l’établissement de nouvelles connexions. Si une session se déconnecte, service Broker pour les connexions Bureau à distance se reconnectera à l’utilisateur du serveur hôte de Session Bureau à distance et de leur session interrompue, qui existe toujours dans la batterie de serveurs hôte de Session Bureau à distance.

Vous devez installer la mise en correspondance des certificats numériques sur le serveur du service Broker pour les connexions Bureau à distance et le client pour prendre en charge l’authentification unique et publication d’applications. Lors du développement ou test d’un réseau, vous pouvez utiliser un certificat auto-signé généré automatiquement. Toutefois, les services publiés nécessitent un certificat numérique à partir d’une autorité de certification approuvée. Le nom que vous donnez le certificat doit être le même comme l’interne domaine nom complet (FQDN) de la machine virtuelle de service Broker pour les connexions Bureau à distance.

Vous pouvez installer le Broker de connexion Bureau à distance de Windows Server 2016 sur la même machine virtuelle en tant qu’AD DS pour réduire les coûts. Si vous avez besoin faire évoluer à d’autres utilisateurs, vous pouvez également ajouter des machines virtuelles supplémentaires Broker pour les connexions Bureau à distance dans le même groupe à haute disponibilité pour créer un cluster de service Broker pour les connexions Bureau à distance.

Avant de pouvoir créer un cluster de service Broker pour les connexions Bureau à distance, vous devez déployer une base de données SQL Azure dans un environnement du locataire ou créer un groupe de disponibilité AlwaysOn de SQL Server.

Pour plus d’informations, consultez les articles suivants :

* [Ajouter le serveur Service Broker pour les connexions Bureau à distance au déploiement et configurer la haute disponibilité](rds-connection-broker-cluster.md)
* [Base de données SQL](desktop-hosting-service.md#sql-database) dans le service d’hébergement bureau.

## <a name="remote-desktop-gateway"></a>Passerelle des services Bureau à distance

Passerelle des services Bureau à distance (passerelle RD) accorde aux utilisateurs sur les réseaux publics aux ordinateurs de bureau Windows et aux applications hébergées dans les services de cloud de Microsoft Azure.

Le composant de passerelle Bureau à distance utilise Secure Sockets Layer (SSL) pour chiffrer le canal de communication entre les clients et le serveur. La machine virtuelle de passerelle Bureau à distance doit être accessible via une adresse IP publique qui permet les connexions TCP entrantes vers le port 443 et les connexions entrantes UDP au port 3391. Cela permet aux utilisateurs de se connecter via internet en utilisant le protocole de transport de communication HTTPS et le protocole UDP, respectivement.

Les certificats numériques installés sur le serveur et client doivent correspondre pour que cela fonctionne. Lorsque vous êtes développement ou test d’un réseau, vous pouvez utiliser un certificat auto-signé généré automatiquement. Toutefois, un service publié nécessite un certificat auprès d’une autorité de certification approuvée. Le nom du certificat doit correspondre au FQDN utilisé pour accéder à la passerelle Bureau à distance, si le nom de domaine complet est le nom DNS externe de l’adresse IP publique ou de l’enregistrement DNS CNAME qui pointe vers l’adresse IP publique.

Pour les clients avec un nombre d’utilisateurs, les rôles de l’accès Web Bureau à distance et passerelle Bureau à distance peuvent être combinés sur une seule machine virtuelle pour réduire les coûts. Vous pouvez également ajouter plusieurs machines virtuelles de passerelle Bureau à distance à une batterie de serveurs de passerelle Bureau à distance pour accroître la disponibilité du service et montée en puissance à d’autres utilisateurs. Machines virtuelles dans des batteries de serveurs de passerelle Bureau à distance supérieure doit être configurés dans un jeu d’équilibrage de charge. Affinité d’IP n’est pas nécessaire lorsque vous utilisez la passerelle Bureau à distance sur une machine virtuelle Windows Server 2016, mais c’est lorsque vous l’exécutez sur un ordinateur virtuel de Windows Server 2012 R2.

Pour plus d’informations, consultez les articles suivants :

* [Ajouter une haute disponibilité pour le serveur web frontal Web de bureau à distance et passerelle](rds-rdweb-gateway-ha.md)
* [Services Bureau à distance - accès en tout lieu](rds-plan-access-from-anywhere.md)
* [Services Bureau à distance - multi-factor authentication](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>Accès Web Bureau à distance

À distance Bureau Web Access (RD Web Access) permet aux utilisateurs accéder aux ordinateurs et des applications via un portail web et lance les via l’application client Bureau à distance Microsoft native de l’appareil. Vous pouvez utiliser le portail web pour publier des ordinateurs de bureau Windows et des applications pour Windows et les appareils non Windows client, et vous pouvez également sélectionner publier postes de travail ou des applications à des utilisateurs ou groupes.

L’accès Web Bureau à distance doit Internet Information Services (IIS) pour fonctionner correctement. Une connexion de protocole sécurisé HTTPS (Hypertext Transfer) fournit un canal de communications entre les clients et le serveur Web de bureau à distance. La machine virtuelle de l’accès Web Bureau à distance doit être accessible via une adresse IP publique qui permet les connexions TCP entrantes vers le port 443 pour permettre aux utilisateurs du locataire pour vous connecter à partir d’internet à l’aide du protocole de transport de communications HTTPS.

Mise en correspondance des certificats numériques doit être installé sur le serveur et les clients. Pour le développement et à des fins de tests, cela peut être un certificat auto-signé généré automatiquement. Pour un service publié, le certificat numérique doit être obtenu à partir d’une autorité de certification approuvée. Le nom du certificat doit correspondre le domaine nom complet (FQDN) utilisé pour accéder à accès Web de bureau à distance. Noms de domaine complets possibles incluent le nom DNS externe pour l’adresse IP publique et l’enregistrement DNS CNAME qui pointe vers l’adresse IP publique.

Pour les clients avec un nombre d’utilisateurs, vous pouvez réduire les coûts en combinant les charges de travail de l’accès Web Bureau à distance et passerelle Bureau à distance dans une seule machine virtuelle. Vous pouvez également ajouter des machines virtuelles supplémentaires Web Bureau à distance à une batterie de serveurs d’accès Web de bureau à distance pour accroître la disponibilité du service et montée en puissance à d’autres utilisateurs. Dans une batterie de serveurs Bureau à distance Web Access avec plusieurs machines virtuelles, vous devrez configurer les machines virtuelles dans un jeu d’équilibrage de charge.

Pour plus d’informations sur la façon de configurer l’accès Web de bureau à distance, consultez les articles suivants :

* [Configurer le client web Bureau à distance pour vos utilisateurs](clients/remote-desktop-web-client-admin.md)
* [Créer et déployer une collection de Services Bureau à distance](rds-create-collection.md)
* [Créer une collection de Services Bureau à distance pour les ordinateurs de bureau et des applications à exécuter](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>Gestionnaire de licences bureau à distance

Activé à distance (bureau à distance par le Gestionnaire de licences) permettent aux utilisateurs de se connecter aux serveurs hôtes de Session Bureau à distance qui héberge des ordinateurs de bureau et les applications du locataire. Environnements client sont généralement fournis avec le serveur de licences des services Bureau à distance déjà installé, mais pour les environnements hébergés, vous devrez configurer le serveur en mode par utilisateur.

Le fournisseur de service doit suffisamment licences d’accès abonné (SAL) RDS pour couvrir tous les uniques (non simultanés) utilisateurs autorisés à se connecter au service de chaque mois. Fournisseurs de services peuvent acheter des Services d’Infrastructure Microsoft Azure directement et achetez SAL via le programme Microsoft licences accord SPLA (Service Provider). Les clients qui souhaitent une solution de bureau hébergée doivent acheter la solution hébergée complète (Azure et services Bureau à distance) à partir du fournisseur de service.

Petits locataires peuvent réduire les coûts en combinant le serveur de fichiers et les composants de gestion de licences bureau à distance sur une seule machine virtuelle. Pour assurer la disponibilité de service plus élevée, locataires peuvent déployer deux machines virtuelles serveur de licences des services Bureau à distance dans le même groupe à haute disponibilité. Tous les serveurs Bureau à distance dans un environnement du locataire sont associés avec les deux serveurs de licences des services Bureau à distance pour informer les utilisateurs peuvent se connecter aux nouvelles sessions, même si un des serveurs tombe en panne.

Pour plus d’informations, consultez les articles suivants :

* [Licence de votre déploiement des services Bureau à distance avec des licences d’accès client (CAL)](rds-client-access-license.md)
* [Activer le serveur de licences des Services Bureau à distance](rds-activate-license-server.md)
* [Effectuer le suivi de vos licences d’accès client des Services Bureau à distance (RDS CAL)](rds-track-cals.md)
* [Licences en Volume Microsoft : options de licence pour les fournisseurs de services](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)