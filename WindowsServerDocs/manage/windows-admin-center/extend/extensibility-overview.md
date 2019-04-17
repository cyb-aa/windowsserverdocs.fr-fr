---
title: Extensions de Windows Admin Center
description: Extensions du SDK Windows Admin Center (projet Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fa3d7e75b32f0195346e58db54b7932c8d2fd3b9
ms.sourcegitcommit: 659544db1e19d6eecc52c7de07116ae735280544
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2019
ms.locfileid: "9001841"
---
# Extensions de Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est conçu en tant que plateforme extensible pour permettre aux partenaires et aux développeurs de tirer parti des fonctionnalités existant dans Windows Admin Center, de réaliser une intégration transparente avec d’autres produits et solutions d’administration informatique et de fournir une valeur supplémentaire aux clients. Chaque solution et outil de Windows Admin Center est conçu sous forme d'extension et utilise les mêmes fonctionnalités d’extensibilité disponibles pour les partenaires et les développeurs, afin que vous puissiez créer des outils puissants, comme ceux actuellement proposés dans Windows Admin Center.

Les extensions de Windows Admin Center sont créées à l’aide de technologies web moderne, notamment HTML5, CSS, Angular, TypeScript et jQuery, et peuvent gérer les serveurs cibles via PowerShell ou WMI. Vous pouvez également gérer des serveurs cible, des services ou des périphériques sur différents protocoles tels que REST en créant un plug-in de passerelle Windows Admin Center.

## Raisons pour lesquelles vous devriez envisager de développer une extension pour Windows Admin Center

Voici la valeur que vous pouvez apporter à votre produit et aux clients en développant des extensions pour Windows Admin Center:

- **Intégration avec les outils de Windows Admin Center:** intégrez vos produits et vos services avec les outils de gestion de serveur et de cluster de Windows Admin Center et fournissez des expériences de surveillance, de gestion et de résolution des problèmes de bout en bout, unifiées et transparentes.
- **Tirez parti des fonctionnalités de sécurité, d'identité et de gestion de plateforme:** activez la prise en charge d'Azure Active Directory (AAD), Multi-Factor Authentication, le contrôle d’accès basé sur un rôle (RBAC), la journalisation, l’audit pour votre produit et vos services en exploitant les fonctionnalités de la plateforme Windows Admin Center afin de répondre aux exigences complexes des entreprises informatiques actuelles.
- **Développer à l’aide des technologies web les plus récentes:** créez rapidement des expériences utilisateur exceptionnelles à l’aide de technologies web modernes, notamment HTML5, CSS, Angular, TypeScript et jQuery et les contrôles d’interface utilisateur riches et puissants inclus dans le SDK Windows Admin Center.
- **Développez la portée du produit:** faites partie du nouvel écosystème de Windows Admin Center, atteignez notre clientèle en croissance rapide et tirez parti de la dynamique de lancement de Windows Server2019 dans le courant de l'année.

## Commencer à développer avec le SDK Windows Admin Center

Il est facile de prise en main avec le développement de Windows Admin Center!  Vous trouverez des exemples de code pour le [plug-in de passerelle](develop-gateway-plugin.md) , [la solution](develop-solution.md)et [outil de](develop-tool.md)types d’extension dans notre documentation du Kit de développement logiciel. Il vous serez exploiter Windows Admin Center CLI pour créer un nouveau projet d’extension, puis suivez les guides individuels pour personnaliser votre projet pour répondre à vos besoins.

Nous avons fait un [Kit d’outils de conception SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponibles pour vous aider à rapidement créer une des extensions dans PowerPoint Windows Admin Center à l’aide de styles Windows Admin Center, des contrôles et modèles de page. Consultez votre extension peut être semblable à ceci dans Windows Admin Center avant de commencer à coder!

Nous avons également des exemples de code hébergé sur GitHub: [Outils de développement](https://aka.ms/wacsdk) est un exemple d’extension solution contenant un riche ensemble de contrôles que vous pouvez parcourir et utiliser dans votre propre extension. Les Outils de développement constituent une extension entièrement fonctionnelle qui peut être chargée dans Windows Admin Center en mode développeur.

Consultez les rubriques ci-dessous pour en savoir plus sur le SDK et sa prise en main:

- [Comprendre le fonctionnement des extensions](understand-extensions.md)
- [Développer une extension](developing-extensions.md)
- [Guides](guides.md)
- [Publier votre extension](publish-extensions.md)

## Actualités du partenaire

Découvrez l'extraordinaire valeur apportée par les partenaires à l’écosystème Windows Admin Center et essayez ces extensions dès à présent. Découvrez en davantage sur [l’installation des extensions](../configure/using-extensions.md) à partir de Windows Admin Center.

### DataON

MUST de dataon apporte la surveillance, de gestion et bout en bout pour une visibilité de DataON hyperconvergé infrastructure et systèmes de stockage basé sur Windows Server. L’extension doit ajoute une valeur unique tels que les rapports de données historiques, de mappage de disque, alertes système et de SAN similaires service d’appel personnel, qui complètent les fonctionnalités de gestion infrastructure hyperconvergée par le biais d’un transparente et un serveur Windows Admin Center expérience unifiée. [En savoir plus sur l'extension MUST de DataON et son expérience de développement](case-studies/dataon.md).

![Extension DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### Fujitsu

Les extensions ServerView Health et RAID Health de Fujitsu pour Windows Admin Center fournissent une surveillance et une gestion approfondie de composants matériels critiques tels que les processeurs, la mémoire, les sous-systèmes d’alimentation et de stockage pour les serveurs PRIMERGY de Fujitsu. En utilisant les modèles de conception d'expérience utilisateur et les contrôles d’interface utilisateur de Windows Admin Center, Fujitsu a fait grandement progresser notre vision d'une compréhension de bout en bout des rôles de serveur et services, du système d’exploitation et de la gestion du matériel via la plateforme Windows Admin Center. [En savoir plus sur les extensions de Fujitsu et son expérience de développement](case-studies/fujitsu.md).

![Extension ServerView de Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### Lenovo

Extension de XClarity intégrateur de Lenovo prend la gestion à la vitesse supérieure en intégrant en toute transparence dans plusieurs expériences dans Windows Admin Center. La solution intégrateur XClarity fournit une vue d’ensemble de tous vos serveurs Lenovo et extensions d’outil différentes fournissent des détails de matériel que vous soyez connecté à un seul serveur, cluster de basculement ou un cluster hyperconvergé. [En savoir plus sur l’extension Lenovo XClarity intégrateur](case-studies/lenovo.md).

![Extension Lenovo](../media/extensibility-overview/lenovo-extension.png)

### Stockage pur

Stockage pur fournit d’entreprise, les solutions de stockage flash-toutes les données qui permettent d’architecture centrées sur les données afin d’accélérer vos activités pour un avantage compétitif. L’extension de stockage pur pour Windows Admin Center offre une vue volet unique des produits FlashArray pur et permet aux utilisateurs d’effectuer des tâches de surveillance, afficher les mesures de performances en temps réel et gérer les volumes de stockage et initiateurs par le biais d’une interface utilisateur unique expérience. [En savoir plus sur les extensions de Pure et son expérience de développement](case-studies/purestorage.md).

![Extension du stockage pure](../media/extensibility-overview/purestorage-extension.png)

### Squared Up

Squared Up fournit une expérience de surveillance optimale basée sur System Center Operations Manager et l’intégration avec Azure Log Analytics, Application Insights et d'autres solutions de surveillance. L'[extension Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) intègre des données de performances historiques, ainsi que des topologies et des dépendances d’application dynamique dans le contexte de gestion de serveur et de cluster fourni par Windows Admin Center. Les premiers clients ont plébiscité sa capacité d'intégrer un volume de données considérable provenant de nombreuses sources disparates dans une expérience unique. [En savoir plus sur l'extension de Squared Up et son expérience de développement](case-studies/squared-up.md).

![Extension Squared Up](../media/extensibility-overview/squaredup-extension.png)