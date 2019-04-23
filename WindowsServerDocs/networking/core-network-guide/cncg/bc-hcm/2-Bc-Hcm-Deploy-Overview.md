---
title: Présentation du déploiement du mode de cache hébergé BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 55686a9c-60dd-47f4-9f1f-fe72c2873a44
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 930a9b4872a7a79351055841a5d716dd99df0fa9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829510"
---
# <a name="branchcache-hosted-cache-mode-deployment-overview"></a>Présentation du déploiement du mode de cache hébergé BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser ce guide pour déployer un serveur de cache hébergé de BranchCache dans une succursale où les ordinateurs sont joints à un domaine. Vous pouvez utiliser cette rubrique pour obtenir une vue d’ensemble du processus de déploiement Mode de Cache hébergé BranchCache.

Cette présentation inclut l’infrastructure de BranchCache que vous avez besoin, ainsi que d’une simple présentation détaillée du déploiement.

## <a name="bkmk_components"></a>Infrastructure de déploiement de serveur de Cache hébergé

Dans ce déploiement, le serveur de cache hébergé est déployé à l’aide de points de connexion de service dans Active Directory Domain Services \(AD DS\), et vous avez la possibilité avec BranchCache dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012, pour préhacher le contenu partagé sur le Web et de fichier en fonction des serveurs de contenu, puis précharger du contenu sur les serveurs de cache hébergé.

L’illustration suivante montre l’infrastructure qui est nécessaire pour déployer un serveur de cache hébergé de BranchCache.

![Vue d’ensemble du Mode de Cache hébergé BranchCache](../../../media/BranchCache-Hcm-Overview/Bc-Hcm-Overview.jpg)

> [!IMPORTANT]
> Bien que ce déploiement décrit les serveurs de contenu dans un centre de données cloud, vous pouvez utiliser ce guide pour déployer un serveur de cache hébergé de BranchCache, quel que soit l’où vous déployez vos serveurs de contenu – dans votre siège social ou dans un emplacement de cloud.

### <a name="hcs1-in-the-branch-office"></a>HCS1 dans la succursale

Vous devez configurer cet ordinateur comme serveur de cache hébergé. Si vous choisissez de préhacher des données du serveur de contenu afin que vous puissiez précharger le contenu sur vos serveurs de cache hébergé, vous pouvez importer des packages de données qui contiennent le contenu à partir de vos serveurs Web et de fichier.

### <a name="web1-in-the-cloud-data-center"></a>Web1 dans le centre de données cloud

Web1 est un BranchCache\-activé le serveur de contenu. Si vous choisissez de préhacher des données du serveur de contenu afin que vous puissiez précharger le contenu sur vos serveurs de cache hébergé, vous pouvez préhacher le contenu partagé sur WEB1, puis créer un package de données que vous copiez vers HCS1.

### <a name="file1-in-the-cloud-data-center"></a>File1 dans le centre de données cloud

File1 est un BranchCache\-activé le serveur de contenu. Si vous choisissez de préhacher des données du serveur de contenu afin que vous puissiez précharger le contenu sur vos serveurs de cache hébergé, vous pouvez préhacher le contenu partagé sur FILE1, puis créer un package de données que vous copiez vers HCS1.
  
### <a name="dc1-in-the-main-office"></a>DC1 dans le siège social

DC1 est un contrôleur de domaine, et vous devez configurer la stratégie de domaine par défaut ou une autre stratégie qui est plus appropriée pour votre déploiement, avec les paramètres de stratégie de groupe BranchCache à activer automatique de la découverte de Cache hébergé par Point de connexion de Service.

Lorsque les ordinateurs clients dans la branche ont la stratégie de groupe actualisée, et ce paramètre de stratégie est appliqué, ils automatiquement localisent et commencent à utiliser le serveur de cache hébergé de la succursale.

### <a name="client-computers-in-the-branch-office"></a>Ordinateurs clients dans la succursale

Vous devez actualiser la stratégie de groupe sur les ordinateurs clients pour appliquer les nouveaux paramètres de stratégie de groupe BranchCache et autoriser les clients à localiser et utiliser le serveur de cache hébergé.

## <a name="bkmk_overview"></a>Vue d’ensemble du processus de déploiement serveur de Cache hébergé

>[!NOTE]
>Les détails de la façon d’effectuer ces étapes sont fournies dans la section [déploiement en Mode Cache hébergé BranchCache](4-Bc-Hcm-Deployment.md).

Le processus de déploiement d’un serveur de Cache hébergé BranchCache s’effectue en ces étapes :

>[!NOTE]
>Certaines des étapes ci-dessous sont facultatifs, tels que ces étapes montrent comment préhacher et précharger du contenu sur des serveurs de cache hébergé. Lorsque vous déployez BranchCache en mode cache hébergé, vous n’êtes pas obligé de contenu prehash sur vos serveurs de contenu Web et de fichier, pour créer un package de données et importer le package de données afin de précharger vos serveurs de cache hébergé avec du contenu. Les étapes sont indiquées comme facultatifs dans cette section et dans la section [déploiement en Mode Cache hébergé BranchCache](4-Bc-Hcm-Deployment.md) afin que vous pouvez les ignorer si vous préférez.

1. Sur HCS1, utilisez les commandes Windows PowerShell pour configurer l’ordinateur en tant que serveur de cache hébergé et d’enregistrer un Point de connexion de Service dans Active Directory.

2. \(Facultatif\) sur HCS1, si les valeurs par défaut de BranchCache ne correspondent pas à vos objectifs de déploiement pour le serveur et le cache hébergé, configurer la quantité d’espace disque que vous souhaitez allouer au cache hébergé. Configurez également l’emplacement du disque que vous préférez pour le cache hébergé.

3. \(Facultatif\) Préhacher le contenu sur les serveurs de contenu, de créer des packages de données et de précharger du contenu sur le serveur de cache hébergé.

    > [!NOTE]
    > Hachage préalable et précharger le contenu sur votre serveur de cache hébergé est facultatif, toutefois si vous choisissez de préhacher et préchargement, vous devez effectuer toutes les étapes ci-dessous qui s’appliquent à votre déploiement. \(Par exemple, si vous n’avez pas de serveurs Web, il est inutile d’effectuer les étapes relatives à un hachage préalable et préchargement du contenu du serveur Web.\)

    1. Sur WEB1, préhacher le contenu du serveur Web et créer un package de données.

    2. Sur FILE1, préhacher le contenu du serveur de fichiers et créer un package de données.

    3. À partir de WEB1 et FILE1, copiez les packages de données sur le serveur de cache hébergé HCS1.

    4. Sur HCS1, importez les packages de données pour précharger le cache de données.

4. Sur DC1, configurez joints à un domaine ordinateurs clients des succursales pour le mode de cache hébergé en configurant la stratégie de groupe avec les paramètres de stratégie de BranchCache.

5. Sur les ordinateurs clients, actualiser la stratégie de groupe.

Pour continuer avec ce guide, consultez [BranchCache hébergé Cache Mode planification du déploiement](3-Bc-Hcm-Plan.md).