---
title: Présentation du déploiement du mode de cache hébergé BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc6ade92eb5fe04271033973911ccb98e871d236
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406372"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Présentation du déploiement du mode de cache hébergé BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser ce guide pour déployer un serveur de cache hébergé de BranchCache dans une filiale où les ordinateurs sont joints à un domaine. Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble du processus de déploiement en mode de cache hébergé de BranchCache.

Cette vue d’ensemble comprend l’infrastructure BranchCache dont vous avez besoin, ainsi qu’une vue pas à pas simple du déploiement.

## <a name="bkmk_components"></a>Infrastructure de déploiement du serveur de cache hébergé

Dans ce déploiement, le serveur de cache hébergé est déployé à l’aide de points de connexion de service dans Active Directory Domain Services \(AD DS\), et vous avez l’option avec BranchCache dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, pour préhacher le contenu partagé sur les serveurs de contenu Web et basés sur des fichiers, puis précharger le contenu sur les serveurs de cache hébergé.

L’illustration suivante montre l’infrastructure requise pour déployer un serveur de cache hébergé BranchCache.

![Vue d’ensemble du mode de cache hébergé de BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Bien que ce déploiement représente les serveurs de contenu dans un centre de données Cloud, vous pouvez utiliser ce guide pour déployer un serveur de cache hébergé de BranchCache, quel que soit l’endroit où vous déployez vos serveurs de contenu (dans votre bureau principal ou dans un emplacement Cloud).

### <a name="hcs1-in-the-branch-office"></a>HCS1 dans la filiale

Vous devez configurer cet ordinateur en tant que serveur de cache hébergé. Si vous choisissez de préhacher les données du serveur de contenu afin de pouvoir précharger le contenu sur vos serveurs de cache hébergé, vous pouvez importer des packages de données qui contiennent le contenu de vos serveurs Web et de fichiers.

### <a name="web1-in-the-cloud-data-center"></a>WEB1 dans le centre de données Cloud

WEB1 est un serveur de contenu BranchCache\-activé. Si vous choisissez de préhacher des données de serveur de contenu pour pouvoir précharger le contenu sur vos serveurs de cache hébergé, vous pouvez préhacher le contenu partagé sur WEB1, puis créer un package de données que vous copiez dans HCS1.

### <a name="file1-in-the-cloud-data-center"></a>Fichier1 dans le centre de données Cloud

Fichier1 est un serveur de contenu BranchCache\-activé. Si vous choisissez de préhacher des données de serveur de contenu pour pouvoir précharger le contenu sur vos serveurs de cache hébergé, vous pouvez préhacher le contenu partagé sur fichier1, puis créer un package de données que vous copiez dans HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 dans le siège social

DC1 est un contrôleur de domaine, et vous devez configurer la stratégie de domaine par défaut, ou une autre stratégie plus appropriée pour votre déploiement, avec BranchCache stratégie de groupe paramètres pour activer la découverte automatique du cache hébergé par le point de connexion de service.

Lorsque des ordinateurs clients de la succursale ont stratégie de groupe actualisés et que ce paramètre de stratégie est appliqué, ils détectent et commencent automatiquement à utiliser le serveur de cache hébergé dans la filiale.

### <a name="client-computers-in-the-branch-office"></a>Ordinateurs clients de la filiale

Vous devez actualiser les stratégie de groupe sur les ordinateurs clients pour appliquer les nouveaux paramètres de stratégie de groupe BranchCache et permettre aux clients de localiser et d’utiliser le serveur de cache hébergé.

## <a name="bkmk_overview"></a>Vue d’ensemble du processus de déploiement du serveur de cache hébergé

>[!NOTE]
>Les détails relatifs à l’exécution de ces étapes sont fournis dans la section [déploiement en mode de cache hébergé de BranchCache](4-Bc-Hcm-Deployment.md).

Le processus de déploiement d’un serveur de cache hébergé BranchCache se produit dans les étapes suivantes :

>[!NOTE]
>Certaines des étapes ci-dessous sont facultatives, telles que les étapes qui montrent comment préhacher et précharger du contenu sur des serveurs de cache hébergé. Lorsque vous déployez BranchCache en mode de cache hébergé, vous n’êtes pas obligé de préhacher le contenu sur vos serveurs de contenu Web et de fichiers, de créer un package de données et d’importer le package de données afin de précharger les serveurs de cache hébergé avec du contenu. Les étapes sont indiquées comme facultatives dans cette section et dans la section [déploiement en mode de cache hébergé de BranchCache](4-Bc-Hcm-Deployment.md) afin que vous puissiez les ignorer si vous le souhaitez.

1. Sur HCS1, utilisez les commandes Windows PowerShell pour configurer l’ordinateur en tant que serveur de cache hébergé et pour inscrire un point de connexion de service dans Active Directory.

2. \(\) facultatif sur HCS1, si les valeurs par défaut BranchCache ne correspondent pas à vos objectifs de déploiement pour le serveur et le cache hébergé, configurez la quantité d’espace disque que vous souhaitez allouer au cache hébergé. Configurez également l’emplacement du disque que vous préférez pour le cache hébergé.

3. \(facultative\) contenu de préhachage sur les serveurs de contenu, créer des packages de données et précharger du contenu sur le serveur de cache hébergé.

    > [!NOTE]
    > Le préhachage et le préchargement du contenu sur votre serveur de cache hébergé sont facultatifs. Toutefois, si vous optez pour le préhachage et le préchargement, vous devez effectuer toutes les étapes ci-dessous applicables à votre déploiement. \(par exemple, si vous n’avez pas de serveurs Web, vous n’avez pas besoin d’effectuer les étapes relatives au préhachage et au préchargement du contenu du serveur Web.\)

    1. Sur WEB1, le contenu du serveur Web de préhachage et crée un package de données.

    2. Sur fichier1, le contenu du serveur de fichiers de préhachage et la création d’un package de données.

    3. À partir de WEB1 et fichier1, copiez les packages de données vers le serveur de cache hébergé HCS1.

    4. Sur HCS1, importez les packages de données pour précharger le cache de données.

4. Sur DC1, configurez les ordinateurs clients des succursales joints à un domaine pour le mode de cache hébergé en configurant stratégie de groupe avec les paramètres de stratégie BranchCache.

5. Sur les ordinateurs clients, actualisez stratégie de groupe.

Pour continuer ce guide, consultez [planification du déploiement en mode de cache hébergé de BranchCache](3-Bc-Hcm-Plan.md).