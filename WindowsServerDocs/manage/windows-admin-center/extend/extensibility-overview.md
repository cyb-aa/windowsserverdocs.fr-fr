---
title: Extensions de Windows Admin Center
description: Extensions du SDK Windows Admin Center (projet Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: ee8c0203be25b30f173b1887de506844d5b58738
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406916"
---
# <a name="extensions-for-windows-admin-center"></a>Extensions de Windows Admin Center

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Windows Admin Center est conçu en tant que plateforme extensible pour permettre aux partenaires et aux développeurs de tirer parti des fonctionnalités existant dans Windows Admin Center, de réaliser une intégration transparente avec d’autres produits et solutions d’administration informatique et de fournir une valeur supplémentaire aux clients. Chaque solution et outil de Windows Admin Center est conçu sous forme d'extension et utilise les mêmes fonctionnalités d’extensibilité disponibles pour les partenaires et les développeurs, afin que vous puissiez créer des outils puissants, comme ceux actuellement proposés dans Windows Admin Center.

Les extensions de Windows Admin Center sont créées à l’aide de technologies web moderne, notamment HTML5, CSS, Angular, TypeScript et jQuery, et peuvent gérer les serveurs cibles via PowerShell ou WMI. Vous pouvez également gérer des serveurs cible, des services ou des périphériques sur différents protocoles tels que REST en créant un plug-in de passerelle Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Raisons pour lesquelles vous devriez envisager de développer une extension pour Windows Admin Center

Voici la valeur que vous pouvez apporter à votre produit et à vos clients en développant des extensions pour le centre d’administration Windows :

- **Intégrer avec les outils du centre d’administration Windows :** Intégrez vos produits et services aux outils de gestion de serveur et de cluster dans le centre d’administration Windows et fournissez des expériences de dépannage, de gestion et de surveillance de bout en bout, homogènes et transparents à vos clients.
- **Tirez parti des fonctionnalités de sécurité, d’identité et de gestion de la plateforme :** Activez la prise en charge d’Azure Active Directory (AAD), Multi-Factor Authentication, le Access Control basé sur les rôles (RBAC), la journalisation, l’audit pour vos produits et services en tirant parti des fonctionnalités de la plateforme du centre d’administration Windows pour répondre aux exigences complexes des Les organisations informatiques.
- **Développez à l’aide des technologies Web les plus récentes :** Créez rapidement des expériences utilisateur étonnantes à l’aide de technologies Web modernes, notamment HTML5, CSS, angulaires, machine à écrire et jQuery, ainsi que des contrôles d’interface utilisateur riches et puissants, inclus dans le kit de développement logiciel (SDK) Windows Admin Center
- **Étendre le produit :** Devenez un membre du nouvel écosystème du centre d’administration Windows, avec un accès à notre base de clients en constante évolution, et tirez parti de l’inertie de lancement de Windows Server 2019 plus tard cette année.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Commencer à développer avec le kit de développement logiciel (SDK) du centre d’administration Windows

La prise en main du développement du centre d’administration Windows est simple !  Vous trouverez un exemple de code pour les types d’extension de [plug-in](develop-gateway-plugin.md) d' [outil](develop-tool.md), de [solution](develop-solution.md)et de passerelle dans la documentation du SDK. Vous allez utiliser l’interface de commande du centre d’administration Windows pour créer un nouveau projet d’extension, puis suivre les différents guides pour personnaliser votre projet en fonction de vos besoins.

Nous avons mis à disposition un kit de [développement logiciel (SDK) de conception](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) du centre d’administration Windows pour vous aider à simuler rapidement des extensions dans PowerPoint à l’aide des styles, des contrôles et des modèles de pages du centre d’administration Windows. Avant de commencer à coder, consultez à quoi votre extension peut ressembler dans le centre d’administration Windows.

Nous disposons également d’un exemple de code hébergé sur GitHub : [Outils de développement](https://aka.ms/wacsdk) est un exemple d’extension de solution contenant une collection complète de contrôles que vous pouvez parcourir et utiliser dans votre propre extension. Les Outils de développement constituent une extension entièrement fonctionnelle qui peut être chargée dans Windows Admin Center en mode développeur.

Consultez les rubriques ci-dessous pour en savoir plus sur le SDK et sa prise en main :

- [Comprendre le fonctionnement des extensions](understand-extensions.md)
- [Développer une extension](developing-extensions.md)
- [Guides](guides.md)
- [Publier votre extension](publish-extensions.md)

## <a name="partner-spotlight"></a>Actualités du partenaire

Découvrez l'extraordinaire valeur apportée par les partenaires à l’écosystème Windows Admin Center et essayez ces extensions dès à présent. Découvrez en davantage sur [l’installation des extensions](../configure/using-extensions.md) à partir de Windows Admin Center.

### <a name="biitops"></a>BiitOps
L’extension BiitOps changes fournit le suivi des modifications pour le matériel, les logiciels et les paramètres de configuration sur vos machines physiques/virtuelles Windows Server. L’extension BiitOps changes indique précisément ce qui est nouveau, ce qui a changé et ce qui a été supprimé dans un volet unique pour faciliter le suivi des problèmes liés à la conformité, à la fiabilité et à la sécurité. [En savoir plus sur l’extension BiitOps changes](case-studies/biitops.md).

![Extension BiitOps](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

L’extension DataON doit apporter des informations de surveillance, de gestion et de bout en bout à l’infrastructure hyper-convergée de DataON et aux systèmes de stockage basés sur Windows Server. L’extension doit ajouter une valeur unique, telle que la création de rapports d’historique, le mappage de disque, les alertes système et le service d’appel de réseau SAN, en complément du serveur du centre d’administration Windows et des fonctionnalités de gestion de l’infrastructure hyper-convergée, via un expérience unifiée. [En savoir plus sur l'extension MUST de DataON et son expérience de développement](case-studies/dataon.md).

![Extension DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

Les extensions ServerView Health et RAID Health de Fujitsu pour le centre d’administration Windows fournissent une surveillance et une gestion approfondies des composants matériels critiques, tels que les processeurs, la mémoire, les sous-systèmes d’alimentation et de stockage des serveurs Fujitsu PRIMERGY. En utilisant les modèles de conception d'expérience utilisateur et les contrôles d’interface utilisateur de Windows Admin Center, Fujitsu a fait grandement progresser notre vision d'une compréhension de bout en bout des rôles de serveur et services, du système d’exploitation et de la gestion du matériel via la plateforme Windows Admin Center. [En savoir plus sur les extensions de Fujitsu et son expérience de développement](case-studies/fujitsu.md).

![Extension ServerView de Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

L’extension de l’intégrateur XClarity Lenovo met à niveau la gestion du matériel en s’intégrant en toute transparence à différentes expériences au sein du centre d’administration Windows. La solution d’intégrateur XClarity fournit une vue d’ensemble de tous vos serveurs Lenovo, tandis que les différentes extensions outil fournissent des détails sur le matériel, que vous soyez connecté à un serveur unique, à un cluster de basculement ou à un cluster hyper-convergé. [En savoir plus sur l’extension de l’intégrateur XClarity Lenovo](case-studies/lenovo.md).

![Extension Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

Le stockage pur fournit des solutions de stockage de données d’entreprise et toutes les données Flash qui fournissent une architecture orientée données pour accélérer votre activité et bénéficier d’un avantage concurrentiel. L’extension de stockage pur pour le centre d’administration Windows fournit une vue à un seul volet des produits FlashArray purs et permet aux utilisateurs d’effectuer des tâches de surveillance, d’afficher des mesures de performances en temps réel et de gérer les volumes de stockage et les initiateurs via une seule interface utilisateur. final. [En savoir plus sur les extensions de pure et leur expérience de développement](case-studies/purestorage.md).

![Extension de stockage pur](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

L’extension QCT Management suite complète le centre d’administration Windows en fournissant une surveillance et une gestion des serveurs physiques pour les systèmes QCT Azure Stack HCI certifiés. L’extension QCT Management Suite affiche des informations sur le matériel du serveur et fournit une interface utilisateur intuitive pour vous aider à remplacer efficacement les disques physiques, les outils du journal des événements matériels et S.M.A.R.T. gestion de disque prédictive basée sur. [En savoir plus sur l’extension QCT Management Suite](case-studies/qct.md).

![Extension QCT](../media/extensibility-overview/qct-extension.png)

### <a name="squared-up"></a>Squared Up

Squared Up fournit une expérience de surveillance optimale basée sur System Center Operations Manager et l’intégration avec Azure Log Analytics, Application Insights et d'autres solutions de surveillance. L'[extension Squared Up](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) intègre des données de performances historiques, ainsi que des topologies et des dépendances d’application dynamique dans le contexte de gestion de serveur et de cluster fourni par Windows Admin Center. Les premiers clients ont plébiscité sa capacité d'intégrer un volume de données considérable provenant de nombreuses sources disparates dans une expérience unique. [En savoir plus sur l'extension de Squared Up et son expérience de développement](case-studies/squared-up.md).

![Extension Squared Up](../media/extensibility-overview/squaredup-extension.png)