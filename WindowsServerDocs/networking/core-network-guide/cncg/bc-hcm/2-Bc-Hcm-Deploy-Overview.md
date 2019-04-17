---
title: Présentation du déploiement du Mode Cache hébergé de BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: feab3156b89637f64d1af0250df459533b1662c7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Présentation du déploiement du Mode Cache hébergé de BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser ce guide pour déployer un serveur de cache hébergé de BranchCache dans une succursale où les ordinateurs sont joints à un domaine. Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble du processus de déploiement de Mode de Cache hébergé BranchCache.

Cette vue d’ensemble inclut l’infrastructure de BranchCache que vous avez besoin, ainsi qu’une simple présentation pas à pas de déploiement.

## <a name="bkmk_components"></a>Infrastructure de déploiement de serveur de Cache hébergé

Dans ce déploiement, le serveur de cache hébergé est déployé à l’aide de points de connexion de service dans les Services de domaine ActiveDirectory \(ADDS\) et vous avez la possibilité avec BranchCache dans Windows Server2016, Windows Server2012R2 et Windows Server2012, à préhacher le contenu partagé sur le Web et le fichier en fonction des serveurs de contenu, puis précharger du contenu sur les serveurs de cache hébergé.

L’illustration suivante montre l’infrastructure qui est requis pour déployer un serveur de cache hébergé de BranchCache.

![Vue d’ensemble du Mode de Cache hébergé BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Bien que ce déploiement illustre les serveurs de contenu dans un centre de données cloud, vous pouvez utiliser ce guide pour déployer un serveur de cache hébergé de BranchCache, quelle que soit votre serveurs de contenu – où vous déployez dans votre siège social ou dans un emplacement cloud.

### <a name="hcs1-in-the-branch-office"></a>HCS1 dans la filiale

Vous devez configurer cet ordinateur comme serveur de cache hébergé. Si vous choisissez de préhacher les données de serveur de contenu, afin que vous puissiez précharger le contenu sur vos serveurs de cache hébergé, vous pouvez importer des packages de données qui contiennent le contenu à partir de vos serveurs Web et le fichier.

### <a name="web1-in-the-cloud-data-center"></a>Web1 dans le centre de données cloud

Web1 est un serveur de contenu prenant en charge BranchCache\. Si vous choisissez de préhacher les données de serveur de contenu, afin que vous puissiez précharger le contenu sur vos serveurs de cache hébergé, vous pouvez préhacher le contenu partagé sur WEB1, puis créer un package de données que vous copiez dans HCS1.

### <a name="file1-in-the-cloud-data-center"></a>File1 dans le centre de données cloud

File1 est un serveur de contenu prenant en charge BranchCache\. Si vous choisissez de préhacher les données de serveur de contenu, afin que vous puissiez précharger le contenu sur vos serveurs de cache hébergé, vous pouvez préhacher le contenu partagé sur FILE1, puis créer un package de données que vous copiez dans HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 dans le siège social

DC1 est un contrôleur de domaine, et vous devez configurer la stratégie de domaine par défaut ou une autre stratégie est la plus appropriée pour votre déploiement, avec les paramètres de stratégie de groupe BranchCache pour activer la découverte de Cache hébergé automatique par le Point de connexion de Service.

Lorsque les ordinateurs clients de la branche ont la stratégie de groupe actualisée et ce paramètre de stratégie est appliqué, ils automatiquement localisent et commencent à utiliser le serveur de cache hébergé dans la filiale.

### <a name="client-computers-in-the-branch-office"></a>Ordinateurs clients dans la filiale

Vous devez actualiser la stratégie de groupe sur les ordinateurs clients pour appliquer les nouveaux paramètres de stratégie de groupe BranchCache et autoriser les clients à localiser et utiliser le serveur de cache hébergé.

## <a name="bkmk_overview"></a>Vue d’ensemble du processus de déploiement serveur de Cache hébergé

>[!NOTE]
>Les détails de l’exécution de ces étapes sont fournies dans la section [déploiement du Mode de Cache hébergé BranchCache](4-Bc-Hcm-Deployment.md).

Le processus de déploiement d’un serveur de Cache hébergé BranchCache se produit dans ces étapes:

>[!NOTE]
>Certaines des étapes ci-dessous sont facultatives, telles que ces étapes montrent comment préhacher et précharger du contenu sur des serveurs de cache hébergé. Lorsque vous déployez BranchCache en mode de cache hébergé, vous n’êtes pas obligé de contenu prehash sur vos serveurs de contenu Web et de fichier, pour créer un package de données et d’importer le package de données préchargement de vos serveurs de cache hébergé avec du contenu. Les étapes sont indiquées comme facultatifs dans cette section et dans la section [déploiement du Mode de Cache hébergé BranchCache](4-Bc-Hcm-Deployment.md) afin que vous pouvez ignorer les si vous préférez.

1. Sur HCS1, utilisez les commandes Windows PowerShell pour configurer l’ordinateur comme serveur de cache hébergé et enregistrer un Point de connexion de Service dans ActiveDirectory.

2. \(Optional\) sur HCS1, si les valeurs par défaut de BranchCache ne correspondent pas à vos objectifs de déploiement pour le serveur et le cache hébergé, configurer la quantité d’espace disque que vous souhaitez allouer au cache hébergé. Configurez également l’emplacement du disque que vous préférez pour le cache hébergé.

3. Contenu de prehash \(Optional\) sur les serveurs de contenu, créer des packages de données et préchargement contenu sur le serveur de cache hébergé.

    > [!NOTE]
    > Hachage préalable et précharger le contenu sur votre serveur de cache hébergé est facultatif, toutefois si vous choisissez de préhacher et préchargement, vous devez effectuer toutes les étapes ci-dessous qui sont applicable à votre déploiement. \ (Par exemple, si vous ne disposez pas de serveurs Web, il est inutile d’effectuer les étapes relatives à un hachage préalable et préchargement du contenu du serveur Web. \)

    1. Sur WEB1, préhacher le contenu du serveur Web et créer un package de données.

    2. Sur FILE1, préhacher le contenu de serveurs de fichiers et créer un package de données.

    3. À partir de WEB1 et FILE1, copiez les packages de données sur le serveur de cache hébergé HCS1.

    4. Sur HCS1, importez les packages de données pour précharger le cache de données.

4. Sur DC1, configurez joint au domaine ordinateurs clients des succursales pour le mode de cache hébergé en configurant la stratégie de groupe avec les paramètres de stratégie de BranchCache.

5. Sur les ordinateurs clients, actualiser la stratégie de groupe.

Pour continuer avec ce guide, consultez [BranchCache hébergé Cache en Mode planification du déploiement](3-Bc-Hcm-Plan.md).