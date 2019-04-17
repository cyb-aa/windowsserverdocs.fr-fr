---
title: Déployer le Mode de Cache hébergé de BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 326a1f1edfe6cb763a33ebfc8fd5abdd5b6aab3a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Déployer le Mode de Cache hébergé de BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Le Guide du réseau Windows Server2016 Core fournit des instructions pour la planification et déploiement des composants centraux requis pour un réseau pleinement fonctionnel et un nouveau répertoire Active&reg; domaine dans une nouvelle forêt.

Ce guide explique comment créer sur le réseau de base en fournissant des instructions pour déployer BranchCache en mode de cache hébergé dans un ou plusieurs filiales disposant d’un contrôleur de domaine Read\-Only où les ordinateurs clients exécutent Windows&reg; 10, Windows8.1 ou Windows8 et sont joints au domaine.

>[!IMPORTANT]
>N’utilisez pas de ce guide si vous envisagez de déployer ou ont déjà déployé un serveur de cache hébergé de BranchCache qui exécute Windows Server2008R2. Ce guide fournit des instructions pour le déploiement du mode de cache hébergé avec un serveur de cache hébergé qui exécute Windows Server&reg; 2016, Windows Server2012R2 ou Windows Server2012.

Ce guide contient les sections suivantes.

- [Configuration requise pour l’utilisation de ce guide](#bkmk_pre)

- [À propos de ce guide](#bkmk_about)

- [Ce que ce guide ne fournit pas](#bkmk_not)

- [Vues d’ensemble de la technologie](#bkmk_tech)

- [Présentation du déploiement du Mode Cache hébergé de BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Le Mode de Cache hébergé de BranchCache planification du déploiement](3-Bc-Hcm-Plan.md)

- [Déploiement du Mode de Cache hébergé de BranchCache](4-Bc-Hcm-Deployment.md)

- [Ressources supplémentaires](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Configuration requise pour l’utilisation de ce guide

Il s’agit d’un guide d’accompagnement pour le Guide du réseau Windows Server2016 Core. Pour déployer BranchCache en mode de cache hébergé dans ce guide, vous devez tout d’abord procédez comme suit.

- Déployer un réseau de base dans votre siège social à l’aide du Guide du réseau principal ou déjà les technologies indiquées dans le Guide du réseau de base installé et fonctionne correctement sur votre réseau. Ces technologies incluent TCP\/IP v4, DHCP, les Services de domaine ActiveDirectory \(ADDS\) et DNS.

    > [!NOTE]
    > Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) est disponible dans la bibliothèque technique Windows Server2016.  

- Déployer des serveurs de contenu BranchCache qui exécutent Windows Server2016, Windows Server2012R2 ou Windows Server2012dans votre siège social ou dans un centre de données cloud. Pour plus d’informations sur la façon de déployer des serveurs de contenu BranchCache, consultez [des ressources supplémentaires](11-Bc-Hcm-additional-resources.md).

- Établir des connexions de \(WAN\) de réseau étendu entre votre filiale, votre siège social et, le cas échéant, vos ressources Cloud, à l’aide d’un réseau privé virtuel réseau \(VPN\), DirectAccess ou une autre méthode de connexion.

- Déployer des ordinateurs clients dans votre filiale qui exécutent un des systèmes d’exploitation suivants, qui offrent BranchCache avec prise en charge pour le Service de transfert Intelligent en arrière-plan (BITS), Hyper texte HTTP (Transfer Protocol) et bloc de Message serveur (SMB).
    - Windows10 entreprise
    - Windows10 éducation
    - Windows8.1 entreprise
    - Windows8 entreprise

>[!NOTE]
>Dans les systèmes d’exploitation suivants, BranchCache ne prend pas en charge les fonctionnalités HTTP et SMB, mais ne prend pas en charge la fonctionnalité BranchCache BITS.
>     - Windows10 Professionnel, BITS prise en charge uniquement
>     - Windows8.1Professionnel, BITS prise en charge uniquement
>     - Windows8 Professionnel, BITS prise en charge uniquement

## <a name="bkmk_about"></a>À propos de ce guide

Ce guide est conçu pour les administrateurs système et réseau ayant suivi les instructions dans le Guide du réseau de base Windows Server2016 ou le Guide du réseau de base Windows Server2012 pour déployer un réseau de base, ou pour ceux ayant déjà déployé les technologies incluses dans le Guide du réseau de base, y compris les Services de domaine ActiveDirectory \(ADDS\), \(DNS\) Service de nom de domaine, \(DHCP\) Dynamic Host Configuration Protocol et v4 TCP\/IP.

Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies qui sont utilisés dans ce scénario de déploiement. Ces guides peuvent vous aider à déterminer si ce scénario de déploiement fournit les services et la configuration dont vous avez besoin pour le réseau de votre organisation.

## <a name="bkmk_not"></a>Ce que ce guide ne fournit pas

Ce guide ne fournit pas des informations conceptuelles sur BranchCache, notamment des informations sur les fonctionnalités et modes de BranchCache.  

Ce guide ne fournit pas d’informations sur la façon de déployer des connexions WAN ou autres technologies dans votre filiale, telles que DHCP, un RODC ou un serveur VPN.

En outre, ce guide ne fournit pas de conseils sur le matériel, que vous devez utiliser lorsque vous déployez un serveur de cache hébergé. Il est possible d’exécuter d’autres services et applications sur votre serveur de cache hébergé, toutefois, vous devez vérifier la détermination, en fonction de la charge de travail, les capacités matérielles et taille de la filiale, si vous souhaitez installer le serveur de cache hébergé de BranchCache sur un ordinateur particulier et la quantité d’espace disque à allouer au cache.  
Ce guide ne fournit pas d’instructions pour la configuration des ordinateurs qui exécutent Windows7. Si vous avez des ordinateurs clients qui exécutent Windows7dans vos filiales, vous devez les configurer à l’aide des procédures qui sont différentes de celles fournies dans ce guide pour les ordinateurs clients qui exécutent Windows10, Windows8.1 et Windows8.
  
En outre, si vous avez des ordinateurs qui exécutent Windows7, vous devez configurer votre serveur de cache hébergé avec un certificat de serveur émis par une autorité de certification qui approuvent les ordinateurs clients. \ (Si tous vos ordinateurs clients exécutent Windows10, Windows8.1 ou Windows8, vous n’avez pas besoin de configurer le serveur de cache hébergé avec un certificat de serveur. \) 
> [!IMPORTANT]
> Si vos serveurs de cache hébergé exécutant Windows Server2008R2, utilisez le composant Windows Server2008R2 [Guide de déploiement BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) au lieu de ce guide pour déployer BranchCache en mode de cache hébergé. Appliquer les paramètres de stratégie de groupe qui sont décrites dans ce guide pour tous les clients BranchCache qui exécutent des versions de Windows à partir de Windows7vers Windows10. Les ordinateurs qui exécutent Windows Server2008R2 ne peut pas être configurés en suivant les étapes de ce guide.

## <a name="bkmk_tech"></a>Vues d’ensemble de la technologie

Pour ce guide d’accompagnement, BranchCache est la seule technologie que vous avez besoin pour installer et configurer. Vous devez exécuter les commandes BranchCache Windows PowerShell sur vos serveurs de contenu, par exemple, Web et les serveurs de fichiers, toutefois il est inutile de modifier ou reconfigurer les serveurs de contenu de toute autre manière. En outre, vous devez configurer les ordinateurs clients à l’aide de la stratégie de groupe sur vos contrôleurs de domaine qui exécutent les services ADDS sur Windows Server2016, Windows Server2012R2 ou Windows Server2012.

### <a name="branchcache"></a>BranchCache

BranchCache est une technologie d’optimisation étendu réseau la bande passante (WAN) incluse dans certaines éditions des systèmes d’exploitation Windows Server2016 et Windows10, ainsi que dans certaines éditions de Windows Server2012R2, Windows8.1, Windows Server2012, Windows8, Windows Server2008R2 et Windows7.

Pour optimiser la bande passante réseau étendue lorsque des utilisateurs accéder au contenu sur des serveurs distants, BranchCache télécharge le contenu de demande du client à partir de votre siège social ou les serveurs de contenu cloud hébergé et met en cache dans les filiales, permettant ainsi aux autres ordinateurs client filiales d’accéder au même contenu localement plutôt que via le réseau étendu.

Lorsque vous déployez BranchCache en mode de cache hébergé, vous devez configurer les ordinateurs clients dans la filiale en tant que clients en mode de cache hébergé, et vous devez alors déployer un serveur de cache hébergé dans la filiale. Ce guide explique comment déployer votre serveur de cache hébergé avec du contenu préhaché et préchargé à partir de votre site Web et les serveurs de contenu basé sur le serveur de fichiers.

### <a name="group-policy"></a>Stratégie de groupe

Stratégie de groupe dans Windows Server2016, Windows Server2012R2 et Windows Server2012 est une infrastructure utilisée pour fournir et appliquer un ou plusieurs configurations souhaitées ou les paramètres de stratégie à un ensemble d’utilisateurs ciblés et les ordinateurs dans un environnement ActiveDirectory. 

Cette infrastructure consiste en un moteur de stratégie de groupe et plusieurs \(CSEs\) extensions côté client qui est responsable de lire les paramètres de stratégie sur les ordinateurs clients cibles.

Dans ce scénario, stratégie de groupe permet de configurer les ordinateurs clients membres du domaine avec le mode de cache hébergé de BranchCache.

Pour continuer avec ce guide, consultez [BranchCache hébergé Cache déploiement présentation du Mode](2-Bc-Hcm-Deploy-Overview.md).
