---
title: Bienvenue à distance des Services Bureau dans Windows Server 2016
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
ms.openlocfilehash: 3d148c99911be0cebfc29429d93241f24c2b9606
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453007"
---
# <a name="welcome-to-remote-desktop-services"></a>Bienvenue dans les services Bureau à distance 

Services Bureau à distance (RDS) est la plate-forme de choix pour créer des solutions de virtualisation pour chaque besoin du client final, y compris la fourniture d’applications virtualisées individuelles, en fournissant un accès Bureau à distance sécurisé et en fournissant aux utilisateurs finaux le possibilité d’exécuter leurs applications et les postes de travail à partir du cloud.

![Vue d’ensemble des Services Bureau à distance](./media/rds-overview.png)

Services Bureau à distance offre une souplesse de déploiement, l’efficacité et l’extensibilité de coût, tout remis via une variété d’options de déploiement, y compris Windows Server 2016 pour les déploiements locaux, Microsoft Azure pour les déploiements de cloud et un tableau robust de partenaire solutions.

Selon votre environnement et vos préférences, vous pouvez configurer la solution de services Bureau à distance pour la virtualisation basée sur session, sous la forme d’une infrastructure de bureau virtuelle (VDI) ou une combinaison des deux :

- **Virtualisation basée sur session**: Exploiter la puissance de calcul de Windows Server pour fournir un environnement de session multiples rentable pour susciter de charges de travail quotidien de vos utilisateurs
- **VDI**: Tirez parti de client Windows pour fournir les hautes performances, la compatibilité des applications et la connaissance de vos utilisateurs s’attendent de leur expérience de bureau Windows.

Dans ces environnements de virtualisation, vous avez davantage de flexibilité dans la publication à vos utilisateurs :

- **Ordinateurs de bureau**: Donner à vos utilisateurs une expérience de bureau complète avec un large éventail d’applications que vous installez et que vous gérez. Idéal pour les utilisateurs qui s’appuient sur ces ordinateurs en tant que leurs stations de travail principales ou qui proviennent des clients légers, comme avec MultiPoint Services.
- **RemoteApps**: Spécifier les applications individuelles qui sont hébergés/exécuter sur la machine virtuelle, mais apparaît comme s’ils s’exécutent sur le bureau de l’utilisateur, tel que des applications locales. Les applications ont leur propre entrée de la barre des tâches et peuvent être redimensionnées et déplacées sur moniteurs. Idéal pour déployer et gérer des applications clés dans l’environnement sécurisé et à distance tout en permettant aux utilisateurs de travailler à partir d’et de personnaliser leurs propres ordinateurs de bureau.

Pour les environnements où rentabilité est primordial et que vous souhaitez étendre les avantages du déploiement des postes de travail complètes dans un environnement de virtualisation basée sur session, vous pouvez utiliser [MultiPoint Services](../multipoint-services/multipoint-services.md) afin d’offrir la meilleure valeur. 

Avec ces options et les configurations, vous avez la possibilité de déployer des postes de travail et applications que vos utilisateurs ont besoin dans un à distance, sécurisé et une rentabilité optimale.

## <a name="next-steps"></a>Étapes suivantes

Voici quelques astuces pour vous aider à obtenir une meilleure compréhension des services Bureau à distance et même de commencer le déploiement de votre propre environnement :
-   Comprendre les [configurations prises en charge](rds-supported-config.md) pour les services Bureau à distance avec les versions différentes de Windows et de Windows Server
-   [Planifier et concevoir](rds-plan-and-design.md) un environnement de services Bureau à distance pour prendre en compte des exigences différentes, telles que la haute disponibilité et une authentification multifacteur.
-   Examinez le [modèles d’architecture des Services Bureau à distance](desktop-hosting-logical-architecture.md) qui convient le mieux pour votre environnement de votre choix.
-   Commencer à [déployer votre environnement de services Bureau à distance avec ARM et place de marché Azure](rds-in-azure.md).
