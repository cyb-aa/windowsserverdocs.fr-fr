---
title: Services Bureau à distance - Créer et déployer
description: Étapes à suivre pour créer un déploiement Bureau à distance
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/18/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 176ae424-96e9-4c78-88f5-da418e76c3d7
author: lizap
manager: dongill
ms.openlocfilehash: 9c3962a830d9544e915a96f061e32bb9e037949b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404040"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>Créer et déployer votre déploiement de services Bureau à distance

Un déploiement de services Bureau à distance est une infrastructure utilisée pour partager des applications et des ressources avec les utilisateurs. Selon l'expérience que vous souhaitez offrir, la taille et la complexité de ce déploiement varient. Les déploiements Bureau à distance sont faciles à mettre à l'échelle. Vous pouvez augmenter et diminuer à volonté le nombre de serveurs Bureau à distance de type Accès par le web, Passerelle, Broker pour les connexions et Hôte de session. Vous pouvez utiliser le service Broker pour les connexions Bureau à distance afin de répartir les charges de travail. L'authentification basée sur Active Directory offre un environnement hautement sécurisé. 

Les [clients Bureau à distance](clients/remote-desktop-clients.md) permettent un accès à partir de n'importe quel ordinateur, tablette ou téléphone Windows, Apple ou Android.

Consultez [Architecture des services Bureau à distance](desktop-hosting-logical-architecture.md) pour en savoir plus sur les différents éléments qui composent votre déploiement de services Bureau à distance.

Vous disposez déjà d'un déploiement Bureau à distance basé sur une version antérieure de Windows Server ? Découvrez les options disponibles pour migrer vers Windows Server 2016 afin de tirer parti de nouvelles fonctionnalités et d'améliorations en matière de performances et de mise à l'échelle :

- [Migrer votre déploiement de services Bureau à distance vers Windows Server 2016](migrate-rds-role-services.md)
- [Mettre à niveau votre déploiement de services Bureau à distance vers Windows Server 2016](upgrade-to-rds-2016.md)

Vous souhaitez créer un nouveau déploiement Bureau à distance ? Pour déployer Bureau à distance dans Windows Server 2016, utilisez les informations suivantes :

- [Déployer l'infrastructure de services Bureau à distance](rds-deploy-infrastructure.md)
- [Créer une collection de sessions pour regrouper les applications et ressources que vous souhaitez partager](rds-create-collection.md)
- [Licence de votre déploiement de services Bureau à distance](rds-client-access-license.md)
- Demandez à vos utilisateurs d'installer un [client Bureau à distance](clients/remote-desktop-clients.md) pour qu'ils aient accès aux applications et ressources. 
- Activez la haute disponibilité en ajoutant d'autres services de type Broker pour les connexions et Hôte de session :
   - [Monter en charge une collection existante de services Bureau à distance avec une batterie de serveurs Hôte de session Bureau à distance](rds-scale-rdsh-farm.md)
   - [Ajouter la haute disponibilité à l'infrastructure du service Broker pour les connexions Bureau à distance](rds-connection-broker-cluster.md)
   - [Ajouter la haute disponibilité du site Web des services Bureau à distance et de la passerelle des services Bureau à distance web frontale](rds-rdweb-gateway-ha.md)
   - [Déployer un système de fichiers d’espaces de stockage direct à deux nœuds pour le stockage UPD](rds-storage-spaces-direct-deployment.md)


Si vous êtes un partenaire d'hébergement et que vous souhaitez utiliser les services Bureau à distance pour fournir des applications et ressources à des clients, ou si vous êtes un client à la recherche d'un hébergeur pour vos applications, consultez [Partenaires d'hébergement des services Bureau à distance](rds-hosting-partners.md) pour plus d'informations sur l'évaluation à laquelle vous pouvez vous soumettre afin d'utiliser les services Bureau à distance d'Azure en tant qu'environnement d'hébergement, et pour obtenir la liste des partenaires qui ont réussi cette évaluation.
