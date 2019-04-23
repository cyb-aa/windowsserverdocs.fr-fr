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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885000"
---
# <a name="extensions-for-windows-admin-center"></a>Extensions de Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Windows Admin Center est conçu en tant que plateforme extensible pour permettre aux partenaires et aux développeurs de tirer parti des fonctionnalités existant dans Windows Admin Center, de réaliser une intégration transparente avec d’autres produits et solutions d’administration informatique et de fournir une valeur supplémentaire aux clients. Chaque solution et outil de Windows Admin Center est conçu sous forme d'extension et utilise les mêmes fonctionnalités d’extensibilité disponibles pour les partenaires et les développeurs, afin que vous puissiez créer des outils puissants, comme ceux actuellement proposés dans Windows Admin Center.

Les extensions de Windows Admin Center sont créées à l’aide de technologies web moderne, notamment HTML5, CSS, Angular, TypeScript et jQuery, et peuvent gérer les serveurs cibles via PowerShell ou WMI. Vous pouvez également gérer des serveurs cible, des services ou des périphériques sur différents protocoles tels que REST en créant un plug-in de passerelle Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Raisons pour lesquelles vous devriez envisager de développer une extension pour Windows Admin Center

Voici la valeur que vous pouvez apporter à votre produit et aux clients en développant des extensions pour Windows Admin Center :

- **Intégrer avec les outils Windows Admin Center :** Intégrer vos produits et services des outils de gestion de serveur et de cluster dans Windows Admin Center et remettre unifié et transparente, end-to-end administration d’analyse, de résolution des problèmes d’expériences à vos clients.
- **Tirer parti des fonctionnalités de sécurité, identité et la gestion de la plateforme :** Activer Azure Active Directory (AAD) prend en charge, l’authentification multifacteur, contrôle d’accès en fonction du rôle (RBAC), journalisation, l’audit pour votre produit et les services en exploitant les fonctionnalités de plateforme Windows Admin Center afin de répondre aux exigences complexes d’aujourd'hui Organisations informatiques.
- **Développer à l’aide des dernières technologies web :** Créez rapidement des expériences utilisateur exceptionnelles à l’aide de technologies web modernes tels que HTML5, CSS, Angular, TypeScript et jQuery et contrôles d’interface utilisateur riches et puissantes inclus dans le Kit de développement logiciel Windows Admin Center.
- **Étendre la proximité de produit :** Deviennent une partie de l’écosystème Windows Admin Center nouvelle avec outreach à notre clientèle rapidement croissante et l’inertie de lancement de tirer parti du 2019 de serveur Windows plus tard cette année.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Commencez à développer avec le Kit de développement logiciel Windows Admin Center

Il est facile de mise en route du développement de Windows Admin Center !  Vous trouverez des exemples de code pour [outil](develop-tool.md), [solution](develop-solution.md), et [plug-in de la passerelle](develop-gateway-plugin.md) types d’extension dans notre documentation du SDK. Il vous serez tirer parti de l’interface CLI de Windows Admin Center pour créer un nouveau projet d’extension, puis suivez les guides individuels pour personnaliser votre projet pour répondre à vos besoins.

Nous avons apporté une de Windows Admin Center [boîte à outils de conception de kit de développement logiciel](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponibles pour vous aider à simuler rapidement extensions dans PowerPoint à l’aide de styles Windows Admin Center, les contrôles et les modèles de page. Consultez ce que votre extension peut ressembler dans Windows Admin Center avant de commencer à coder !

Nous avons également des exemples de code hébergé sur GitHub : [Outils de développement](https://aka.ms/wacsdk) est un exemple d’extension solution contenant une vaste collection de contrôles que vous pouvez parcourir et utiliser dans votre propre extension. Les Outils de développement constituent une extension entièrement fonctionnelle qui peut être chargée dans Windows Admin Center en mode développeur.

Consultez les rubriques ci-dessous pour en savoir plus sur le SDK et sa prise en main :

- [Comprendre le fonctionnement des extensions](understand-extensions.md)
- [Développer une extension](developing-extensions.md)
- [Guides](guides.md)
- [Publier votre extension](publish-extensions.md)

## <a name="partner-spotlight"></a>Actualités du partenaire

Découvrez l'extraordinaire valeur apportée par les partenaires à l’écosystème Windows Admin Center et essayez ces extensions dès à présent. Découvrez en davantage sur [l’installation des extensions](../configure/using-extensions.md) à partir de Windows Admin Center.

### <a name="dataon"></a>DataON

Extension de doit de DataON apporte la surveillance, gestion et bout en bout idée stockage et infrastructure de systèmes hyperconvergés du DataON basés sur Windows Server. L’extension doit ajoute une valeur unique telles que les rapports de données historiques, de mappage de disque, alertes système et de type SAN accueil service d’appel de, qui complètent le server de Windows Admin Center et des fonctionnalités de gestion d’infrastructure Hyper-convergée, via un transparente expérience unifiée. [En savoir plus sur l'extension MUST de DataON et son expérience de développement](case-studies/dataon.md).

![Extension DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

ServerView santé et des extensions de contrôle d’intégrité RAID pour Windows Admin Center Fujitsu fournissent approfondies de surveillance et de gestion des composants matériels critiques tels que de processeurs, mémoire, les sous-systèmes d’alimentation et de stockage pour les serveurs de Fujitsu PRIMERGY. En utilisant les modèles de conception d'expérience utilisateur et les contrôles d’interface utilisateur de Windows Admin Center, Fujitsu a fait grandement progresser notre vision d'une compréhension de bout en bout des rôles de serveur et services, du système d’exploitation et de la gestion du matériel via la plateforme Windows Admin Center. [En savoir plus sur les extensions de Fujitsu et son expérience de développement](case-studies/fujitsu.md).

![Extension ServerView de Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

Extension de XClarity intégrateur de Lenovo prend la gestion du matériel au niveau supérieur en intégrant en toute transparence dans différentes expériences au sein de Windows Admin Center. La solution d’intégration de XClarity fournit une vue d’ensemble de tous vos serveurs de Lenovo et extensions de l’outil différents fournissent des détails sur le matériel si vous êtes connecté à un seul serveur, de cluster de basculement ou d’un cluster hyperconvergé. [En savoir plus sur l’extension de l’intégrateur de Lenovo XClarity](case-studies/lenovo.md).

![Extension de Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

Stockage purs fournit enterprise, les solutions de stockage flash de toutes les données qui offrent une architecture orientée données afin d’accélérer votre entreprise un avantage concurrentiel. L’extension de stockage purs pour Windows Admin Center fournit un affichage simple dans les produits FlashArray Pure et permet aux utilisateurs de réaliser des tâches de surveillance, afficher les métriques de performances en temps réel et gérer des volumes de stockage et les initiateurs via une interface utilisateur unique expérience. [En savoir plus sur les extensions de pur et leur expérience de développement](case-studies/purestorage.md).

![Extension de stockage pure](../media/extensibility-overview/purestorage-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up fournit une expérience de surveillance optimale basée sur System Center Operations Manager et l’intégration avec Azure Log Analytics, Application Insights et d'autres solutions de surveillance. L'[extension Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) intègre des données de performances historiques, ainsi que des topologies et des dépendances d’application dynamique dans le contexte de gestion de serveur et de cluster fourni par Windows Admin Center. Les premiers clients ont plébiscité sa capacité d'intégrer un volume de données considérable provenant de nombreuses sources disparates dans une expérience unique. [En savoir plus sur l'extension de Squared Up et son expérience de développement](case-studies/squared-up.md).

![Extension Squared Up](../media/extensibility-overview/squaredup-extension.png)