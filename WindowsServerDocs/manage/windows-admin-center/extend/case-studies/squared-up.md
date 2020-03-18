---
title: Étude de cas du kit de développement logiciel (SDK) du centre d’administration Windows-disposé en carré
description: Étude de cas du kit de développement logiciel (SDK) du centre d’administration Windows-disposé en carré
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 620d3d9f4b5c3638d49fe9141e83ebdcb9eb245c
ms.sourcegitcommit: 5197a87e659589bcc8d2a32069803ae736b02892
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/16/2020
ms.locfileid: "79429303"
---
# <a name="squared-up-extension"></a>Extension au carré

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Intégration de la surveillance basée sur SCOM, de la visibilité des dépendances du serveur et des informations externes sur les données dans le centre d’administration Windows

Le carré a été créé avec la vision de l’utilisation de la visualisation des données pour aider à résoudre les défis de la complexité informatique de l’entreprise. Les logiciels uniques, légers et de l’interface utilisateur uniquement du carré s’appuient sur la plate-forme puissante de Microsoft System Center Operations Manager, ainsi que sur l’intégration à des sources de données supplémentaires, de Microsoft Azure Log Analytics, Application Insights et système Center Service Manager à des produits tiers tels que ServiceNow, Splunk et bien plus encore, afin de fournir une visibilité sur l’infrastructure d’entreprise et les parcs d’applications à grande échelle, en local et dans des environnements de Cloud hybrides.

> <cite>«Nous avons beaucoup fait appel au centre d’administration Windows tout au long de sa version d’évaluation technique et il s’agit déjà d’un énorme toucher, ce qui nous a permis de résoudre des problèmes tels que nos ingénieurs qui accèdent facilement à nos laboratoires de configuration, et nous avons l’intention d’en faire notre gestion principale une fois qu’elle a atteint la version complète. Nous apprécions le potentiel de l’intégration avec un carré et la capacité à faire apparaître toutes nos données dans un seul et même endroit.»</cite>
>
> --David Acevedo, spécialiste des e/S chez NuStar Energy L.P.

Les clients de niveau carré gèrent des centaines, souvent des milliers de serveurs Windows et les différents portefeuilles d’applications qu’ils proposent, et les deux à la fois, et Microsoft sont en mesure de proposer aux équipes informatiques le meilleur d’une interface utilisateur Web rapide et moderne, afin de fournir les informations dont elles ont besoin. En conséquence, l’équipe a immédiatement vu un alignement passionnant avec le centre d’administration Windows, ce qui permet d’obtenir ces mêmes valeurs et principaux pour la nouvelle génération de l’administration de Windows Server. En particulier, l’équipe a supposé que les données de performances à long terme, les Insights de dépendances de serveur en temps réel et le contexte d’application mis en place par le biais d’un carré supérieur complètent parfaitement les fonctionnalités de gestion de serveur et de données en temps réel élégantes fournies par Centre d’administration Windows.

![Extension au carré](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>« En tant qu’organisation gérant un serveur à grande échelle, l’intégration au centre d’administration Windows ² est le mariage parfait de nos outils localisés et centralisés, et des choses telles que la possibilité de générer un serveur directement en mode de maintenance à partir du centre d’administration Windows, sont très peu gagnantes pour nous »</cite>
>
> --Granson, administrateur des systèmes de virtualisation de Purdue University

Avec une vision claire de la souhait de présenter ces données en toute transparence dans le centre d’administration Windows, le carré a travaillé avec la préversion privée préliminaire du kit de développement logiciel (SDK) du centre d’administration Windows et a été jugé simple, bien documenté et flexible.

À l’aide du kit de développement logiciel (SDK) du centre d’administration Windows, le carré a pu créer une extension qui incorpore dynamiquement les vues carrées pertinentes au sein de l’expérience du centre d’administration Windows. Par exemple, dans le contexte d’un serveur ou d’un cluster spécifique, les vues carrées sont automatiquement incorporées à la visibilité étendue fournie. Les vues incluent les tendances historiques des métriques de performances et de capacité clés (telles que le processeur, la mémoire et le disque), la pile d’hébergement (plateforme Cloud ou la virtualisation de centre de données), les composants d’application tels que les bases de données SQL et les services, et même l’analyse des journaux basés sur le Cloud. et ITSM.

![Extension au carré](../../media/extend-case-study-squared-up/squared-up-2.png)

Le centre d’administration Windows et le centre d’administration Windows partagent un esprit de conception et d’architecture Web moderne, qui a permis une intégration technique simple et une expérience utilisateur transparente. Avec l’administration basée sur le Web devenant de plus en plus la norme, nous pensons que cette méthode d’intégration entre les différents systèmes est la clé de la déverrouillage d’une expérience d’administration moderne et unifiée.

> <cite>«Nous voyons le centre d’administration Windows comme le cœur de l’administration de Windows Server moderne. il s’agit donc d’une expérience remarquable pour nous permettre de travailler en étroite collaboration avec l’équipe et le fait qu’ils travaillent avec une telle vitesse, enthousiasme, flexibilité et, de manière radicale, des paradigmes de développement modernes nous ont rendu compte comme nous, en tant que société de développement de logiciels à la fois plus rapide, agile et rapide.»</cite>
>
> --Richard Benwell, architecte produit au carré

À partir de cet alignement naturel, l’équipe de développement au carré est parvenue à progresser rapidement jusqu’à une intégration de prototype s’affichant en mode natif au sein de l’expérience du centre d’administration Windows et à faire en sorte que les mains de leurs propres prévisualiser les clients. Des réactions des clients, il a été immédiatement clair que l’histoire était gagnante.

> <cite>«L’un des principaux défis liés à la maintenance d’un service remarquable dans notre environnement de plus de 3 500 serveurs consiste à unifier notre paysage diversifié d’outils de gestion et de surveillance, ainsi que l’intégration entre le centre d’administration Windows au carré et le centre d’administration Windows, ce qui apporte ensemble, beaucoup de données, à partir de nombreuses sources disparates, en une console unique, sont massives pour nous.»</cite>
>
> --Martin Ehrnst, responsable technique pour Azure à Intility A/S

Avec ce type d’enthousiasme des clients au carré et avec des tonnes de nouvelles fonctionnalités exceptionnelles à venir dans le centre d’administration Windows, le carré est très enthousiaste pour l’avenir de cette intégration et les possibilités impressionnantes qu’elle ouvre pour ses clients et leurs Voyagez vers une véritable simple vitre pour la gestion des opérations informatiques.

L’intégration au centre d’administration carré/Windows est actuellement en version bêta. Si vous souhaitez accéder à, consultez [la page dédiée du carré](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) pour plus d’informations. Si votre organisation utilise Microsoft System Center Operations Manager et que vous n’avez pas encore de carré (ce qui est essentiel pour que l’extension fonctionne), vous pouvez également faire appel à une version d’évaluation gratuite de 30 jours à partir du même emplacement. 
