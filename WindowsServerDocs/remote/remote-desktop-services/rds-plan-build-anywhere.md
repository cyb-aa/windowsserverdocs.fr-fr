---
title: 'Services Bureau à distance : création n’importe où'
description: Les informations de planification vous aident à déterminer où héberger votre déploiement de services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 9b56614e347b36fb86e6e4680f1b179accaef058
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857372"
---
# <a name="remote-desktop-services---build-anywhere"></a>Services Bureau à distance : création n’importe où

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Déployez en local, dans le cloud, ou un mélange des deux. Modifiez votre déploiement, ainsi que l’évolution de vos besoins.

Quel que soit votre avancement, l’[architecture](desktop-hosting-logical-architecture.md) sous-jacente de l’environnement du Services Bureau à distance environnement reste le même :
- Vous devez toujours disposer d'un serveur connecté à Internet pour utiliser l’accès Web des services Bureau à distance et la Passerelle des services Bureau à distance pour les utilisateurs externes
- Vous devez toujours avoir une instance Active Directory et, pour les environnements hautement disponibles, une base de données SQL pour héberger les propriétés de l'utilisateur et du Bureau distant
- Vous devez toujours disposer d'un accès de communication entre les rôles d'infrastructure de Bureau distant (Service Broker pour les connexions Bureau à distance, Passerelle des services Bureau à distance, Gestionnaire de licences des services Bureau à distance et Accès Web des services Bureau à distance), et les hôtes RDSH ou RDVH finaux pour connecter les utilisateurs finaux à leurs bureaux ou leurs applications.

Cette flexibilité vous permet d'obtenir le meilleur des deux mondes :
- La simplicité et les méthodes de paiement à l'utilisation associées au cloud et au monde en ligne.
- La façon familière et sans tracas de tirer parti des ressources importantes qui existent déjà sur place.

Pour en savoir plus, veuillez consulter l’article [Développer et déployer votre déploiement des Services Bureau à distance](rds-build-and-deploy.md).