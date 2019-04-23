---
title: Services Bureau à distance - Build n’importe où
description: Informations de planification pour vous aider à déterminer où héberger votre déploiement des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: cbb8e73d753b1fe4f0293cf4427c634020a23a42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869510"
---
# <a name="remote-desktop-services---build-anywhere"></a>Services Bureau à distance - Build n’importe où

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Déployer en local, dans le cloud, ou un mélange des deux. Modifiez votre déploiement comme l’évolution de vos besoins.

Quel que soit votre avancement, sous-jacent [architecture](desktop-hosting-logical-architecture.md) des Services Bureau à distance environnement reste le même :
- Vous devez quand même utiliser un serveur accessible sur internet d’utiliser l’accès Web Bureau à distance et passerelle Bureau à distance pour les utilisateurs externes
- Vous devez quand même utiliser un annuaire Active Directory et--pour les environnements hautement disponibles, une instance SQL database à l’utilisateur de la maison et les propriétés de bureau à distance
- Vous devez toujours disposer des accès de communication entre les rôles d’infrastructure de bureau à distance (RD Connection Broker, passerelle Bureau à distance, Gestionnaire de licences bureau à distance et Bureau à distance Web Access) et la fin RDSH ou hôtes RDVH pour pouvoir se connecter les utilisateurs finaux à leurs postes de travail et les applications.

Cette flexibilité vous permet d’obtenir le meilleur des deux mondes :
- Les méthodes de simplicité et de paiement à l’utilisation associés avec le cloud et le monde en ligne.
- La convivialité et la simplicité qui permette d’utiliser les ressources les plus élevées qui déjà existent en local.

Pour plus d’informations, voir comment [générer et déployer votre déploiement des Services Bureau à distance](rds-build-and-deploy.md).