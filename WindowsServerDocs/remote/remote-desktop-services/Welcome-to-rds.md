---
title: Services Bureau bienvenues à distance dans Windows Server 2016
description: Fournit une vue d’ensemble des Services Bureau à distance
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
ms.openlocfilehash: cd00f92254f9e55f83442f5e68e344e0aa7579a2
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708496"
---
# <a name="welcome-to-remote-desktop-services"></a>Bienvenue à distance des Services Bureau 

Remote Desktop Services (RDS) est la plate-forme de choix pour la création de solutions de virtualisation pour chaque besoin de client final, notamment proposer des applications virtualisées individuelles, fournissant un accès sécurisé de bureau à distance et fournissant aux utilisateurs finaux le possibilité d’exécuter des applications et leurs bureaux du nuage.

![Vue d’ensemble des Services Bureau à distance](.\media\rds-overview.png)

RDS offre la souplesse de déploiement, l’efficacité et l’extensibilité de coût, toutes les remis via un grand nombre d’options de déploiement, notamment Windows Server 2016 pour les déploiements sur site, Microsoft Azure pour les déploiements en nuage et un tableau robust de partenaire solutions.

En fonction de votre environnement et les préférences, vous pouvez configurer la solution RDS pour la virtualisation basée sur la session, sous la forme d’une infrastructure d’ordinateurs virtuels (VDI) ou une combinaison des deux:

- **Virtualisation de session**: tirer parti de la puissance de calcul de Windows Server pour fournir un environnement de session multiples rentable pour piloter les charges de travail quotidien de vos utilisateurs
- **VDI**: client Windows exploiter pour fournir la hautes performances, la compatibilité d’application et connaissance que vos utilisateurs ont attendez de leur bureau Windows.

Dans les environnements de virtualisation, vous avez plus de flexibilité dans ce que vous publiez à vos utilisateurs:

- **Postes de travail**: donner aux utilisateurs une expérience avec de nombreuses applications que vous installez et gérez plein. Idéal pour les utilisateurs qui s’appuient sur ces ordinateurs en tant que leurs stations de travail principales ou qui proviennent de clients légers, comme avec les Services MultiPoint.
- **RemoteApps**: spécifier des applications spécifiques qui sont hébergés/exécution sur l’ordinateur virtuel, mais s’affichent comme s’ils exécutent sur le bureau de l’utilisateur comme applications locales. Les applications ont leur propre entrée de la barre des tâches et peuvent être redimensionnées et déplacées sur moniteurs. Idéal pour le déploiement et la gestion des applications clés dans l’environnement de sécurité et à distance tout en permettant aux utilisateurs de travailler à partir d’et à personnaliser leur propre bureau.

Pour les environnements où il est essentielle de rentabilité et que vous souhaitez étendre les avantages du déploiement des postes de travail complètes dans un environnement de virtualisation de session, vous pouvez utiliser [Services MultiPoint](../multipoint-services/multipoint-services.md) pour fournir la meilleure valeur. 

Avec ces options et les configurations, vous avez la possibilité de déployer les postes de travail et les applications de que vos utilisateurs ont besoin de manière rentable et à distance, sécurisé.

## <a name="next-steps"></a>Étapes suivantes

Voici quelques étapes suivantes pour vous aider à mieux comprendre de RDS et même commencer le déploiement de votre propre environnement:
-   Comprendre les [configurations prises en charge](rds-supported-config.md) de RDS avec les différentes versions de Windows et Windows Server
-   [Planifier et concevoir](rds-plan-and-design.md) un environnement RDS pour s’adapter aux différents besoins en termes de haute disponibilité et de l’authentification multifacteur.
-   Passez en revue les [modèles d’architecture des Services Bureau à distance](desktop-hosting-logical-architecture.md) qui conviennent le mieux pour votre environnement de votre choix.
-   Commencer à [déployer votre environnement RDS avec ARM et Azure Marketplace](rds-in-azure.md).
