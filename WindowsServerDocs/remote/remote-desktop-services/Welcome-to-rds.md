---
title: Bienvenue dans les services Bureau à distance de Windows Server 2016
description: Fournit une vue d’ensemble des services Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 02/22/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52b9e09f-39e0-41a9-9d3b-4d5f4eacf3e0
author: christianmontoya
manager: scottman
ms.localizationpriority: medium
ms.openlocfilehash: 74ec3d9bfbdb435c5bcb93ea1ef2cd9cdf2dd9d2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871277"
---
# <a name="welcome-to-remote-desktop-services"></a>Bienvenue dans les services Bureau à distance 

Les services Bureau à distance représentent une plateforme de choix pour la création de solutions de virtualisation répondant aux besoins des utilisateurs finaux, notamment la fourniture d’applications virtualisées individuelles, l’accès sécurisé aux services Bureau à distance et aux terminaux mobiles ainsi que l’exécution d’applications et l’utilisation de postes de travail à partir du cloud.

![Vue d’ensemble des services Bureau à distance](./media/rds-overview.png)

Les services Bureau à distance offrent plusieurs avantages : souplesse de déploiement, rentabilité et extensibilité, le tout via diverses options de déploiement, notamment Windows Server 2016 pour les déploiements locaux, Microsoft Azure pour les déploiements cloud ainsi qu’une gamme étendue de solutions partenaires.

En fonction de votre environnement et de vos préférences, vous pouvez configurer la solution des services Bureau à distance en tant que virtualisation basée sur une session ou pour une infrastructure VDI (infrastructure de bureau virtuel), ou les deux à la fois :

- **Virtualisation basée sur une session** : tirez parti de la puissance de calcul de Windows Server pour fournir un environnement multisession rentable permettant de gérer les charges de travail quotidiennes de vos utilisateurs.
- **VDI** : tirez parti du client Windows afin de fournir un haut niveau de performance, un environnement compatible pour les applications et une expérience utilisateur familière de Windows.

Dans ces environnements de virtualisation, vous disposez d’une flexibilité supplémentaire par rapport à ce que vous publiez pour vos utilisateurs :

- **Bureaux** : offrez une expérience utilisateur complète pour toute une variété d’applications que vous installez et gérez. Cela est idéal pour les utilisateurs qui se servent de ces ordinateurs en tant que stations de travail principales ou qui utilisent des clients légers, à l’image des scénarios d’utilisation de MultiPoint Services.
- **Programmes RemoteApp** : spécifiez des applications individuelles hébergées/exécutées sur la machine virtualisée, mais qui donnent l’impression de s’exécuter sur le poste de travail de l’utilisateur comme des applications locales. Les applications ont leur propre entrée dans la barre des tâches et peuvent être redimensionnées et déplacées d’un moniteur à l’autre. Cela est idéal pour déployer et gérer des applications clés dans un environnement distant et sécurisé tout en permettant aux utilisateurs d’utiliser et de personnaliser leurs propres postes de travail.

Quand la rentabilité est cruciale et que vous souhaitez étendre les avantages du déploiement de postes de travail complets dans un environnement de virtualisation basée sur une session, vous pouvez utiliser [MultiPoint Services](../multipoint-services/multipoint-services.md) pour offrir les meilleures prestations possibles. 

Avec ces options et ces configurations, vous pouvez déployer en toute simplicité les postes de travail et les applications dont vos utilisateurs ont besoin de manière sécurisée, rentable et à distance.

## <a name="next-steps"></a>Étapes suivantes

Voici quelques étapes à suivre pour mieux comprendre les services Bureau à distance ainsi que pour commencer éventuellement à déployer votre propre environnement :
-   Comprendre les [configurations prises en charge](rds-supported-config.md) pour les services Bureau à distance en fonction des différentes versions de Windows et de Windows Server
-   [Planifier et concevoir](rds-plan-and-design.md) un environnement des services Bureau à distance pour répondre à diverses exigences, notamment la haute disponibilité et l’authentification multifacteur
-   Passer en revue les [modèles d’architecture des services Bureau à distance](desktop-hosting-logical-architecture.md) qui conviennent le mieux à l’environnement souhaité
-   Commencer à [déployer votre environnement des services Bureau à distance avec ARM et la Place de marché Azure](rds-in-azure.md)
