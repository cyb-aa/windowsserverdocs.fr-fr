---
title: Déployer le mode de cache hébergé de BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc2cb29f0f00c04c4208bd83d70bc4d966bbad00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839350"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Déployer le mode de cache hébergé de BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le Guide du réseau Windows Server 2016 Core fournit des instructions pour la planification et déploiement des composants centraux requis pour un réseau pleinement fonctionnel et un nouvel annuaire Active Directory&reg; domaine dans une nouvelle forêt.

Ce guide explique comment développer sur le réseau de base en fournissant des instructions pour déployer BranchCache en mode de cache hébergé dans un ou plusieurs succursales avec une lecture\-uniquement de contrôleur de domaine dans lequel les ordinateurs clients exécutent Windows&reg; 10, Windows 8.1 ou Windows 8 et sont joints au domaine.

>[!IMPORTANT]
>N’utilisez pas de ce guide si vous envisagez de déployer ou avez déjà déployé un serveur de cache hébergé de BranchCache qui exécute Windows Server 2008 R2. Ce guide fournit des instructions pour le déploiement du mode de cache hébergé avec un serveur de cache hébergé qui exécute Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.

Ce guide contient les sections suivantes.

- [Conditions préalables pour l’utilisation de ce guide](#bkmk_pre)

- [À propos de ce guide](#bkmk_about)

- [Ce que ce guide ne contient pas](#bkmk_not)

- [Présentations des technologies](#bkmk_tech)

- [Déploiement présentation du Mode de Cache de hébergé de BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Mode de Cache hébergé de BranchCache planification du déploiement](3-Bc-Hcm-Plan.md)

- [BranchCache hébergé le déploiement en Mode de Cache](4-Bc-Hcm-Deployment.md)

- [Ressources supplémentaires](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Conditions préalables pour l’utilisation de ce guide

Il s’agit d’un guide d’accompagnement pour le Guide du réseau Windows Server 2016 Core. Pour déployer BranchCache en mode cache hébergé à l’aide de ce guide, vous devez tout d’abord procéder comme suit.

- Déployez un réseau de base dans votre siège social à l’aide du Guide du réseau de base ou installez les technologies indiquées dans le Guide du réseau de base et faites en sorte qu’elles fonctionnent correctement sur votre réseau. Ces technologies incluent TCP\/IP v4, DHCP, Active Directory Domain Services \(AD DS\)et DNS.

    > [!NOTE]
    > Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) est disponible dans la bibliothèque technique Windows Server 2016.  

- Déployer des serveurs de contenu BranchCache qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 dans votre siège social ou dans un centre de données cloud. Pour plus d’informations sur la façon de déployer des serveurs de contenu BranchCache, consultez [des ressources supplémentaires](11-Bc-Hcm-additional-resources.md).

- Établir le réseau étendu \(WAN\) connexions entre votre filiale, votre siège social et, le cas échéant, vos ressources de Cloud, à l’aide d’un réseau privé virtuel \(VPN\), DirectAccess, ou d’autres méthode de connexion.

- Déployer des ordinateurs clients dans votre filiale qui exécutent l’un des systèmes d’exploitation suivants, qui fournissent de BranchCache avec prise en charge pour le Service de transfert Intelligent en arrière-plan (BITS), Hyper texte HTTP (Transfer Protocol) et bloc de Message serveur (SMB) .
    - Windows 10 Entreprise
    - Windows 10 Éducation
    - Windows 8.1 Entreprise
    - Windows 8 Entreprise

>[!NOTE]
>Dans les systèmes d’exploitation suivants, BranchCache ne prend pas en charge les fonctionnalités HTTP et SMB, mais ne prend pas en charge les fonctionnalités de BITS de BranchCache.
>     - Windows 10 Professionnel, BITS prennent uniquement en charge
>     - Windows 8.1 Pro, BITS prennent uniquement en charge
>     - Windows 8 Professionnel, BITS prennent uniquement en charge

## <a name="bkmk_about"></a>À propos de ce guide

Ce guide est conçu pour les administrateurs système et réseau ayant suivi les instructions dans le Guide du réseau de base Windows Server 2016 ou le Guide du réseau de base Windows Server 2012 pour déployer un réseau de base, ou pour ceux qui ont déjà déployé le technologies incluses dans le Guide réseau de base, y compris les Services de domaine Active Directory \(AD DS\), Service de nom de domaine \(DNS\), Dynamic Host Configuration Protocol \(DHCP\)et TCP\/IP v4.

Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies utilisées dans ce scénario de déploiement. Ces guides peuvent vous aider à déterminer si ce scénario de déploiement fournit les services et la configuration dont vous avez besoin pour le réseau de votre organisation.

## <a name="bkmk_not"></a>Ce que ce guide ne fournit pas

Ce guide ne fournit pas d’informations conceptuelles sur BranchCache, notamment des informations sur ses fonctionnalités et ses modes.  

Il ne fournit pas d’informations sur la façon de déployer des connexions WAN ou d’autres technologies dans votre filiale, telles que DHCP, un contrôleur de domaine en lecture seule ou un serveur VPN.

Ce guide ne donne pas non plus de conseils sur le matériel à utiliser lorsque vous déployez un serveur de cache hébergé. Il est possible d’exécuter d’autres services et applications sur votre serveur de cache hébergé, toutefois, vous devez déterminer, en fonction de la charge de travail, des capacités matérielles et de la taille de la filiale s’il convient d’installer le serveur de cache hébergé BranchCache sur un ordinateur en particulier et la quantité d’espace disque à allouer au cache.  
Ce guide ne fournit pas d’instructions sur la configuration des ordinateurs qui exécutent Windows 7. Si vous avez des ordinateurs clients qui exécutent Windows 7 dans vos filiales, vous devez les configurer à l’aide de procédures qui sont différentes de celles fournies dans ce guide pour les ordinateurs clients qui exécutent Windows 10, Windows 8.1 et Windows 8.
  
En outre, si vous avez des ordinateurs qui exécutent Windows 7, vous devez configurer votre serveur de cache hébergé avec un certificat de serveur émis par une autorité de certification, les ordinateurs clients approuvent. \(Si tous vos ordinateurs clients exécutent Windows 10, Windows 8.1 ou Windows 8, il est inutile de configurer le serveur de cache hébergé avec un certificat de serveur.\) 
> [!IMPORTANT]
> Si vos serveurs de cache hébergé exécutent Windows Server 2008 R2, utilisez Windows Server 2008 R2 [Guide de déploiement BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) au lieu de ce guide pour déployer BranchCache en mode cache hébergé. Appliquez les paramètres de stratégie de groupe qui sont décrites dans ce guide pour tous les clients BranchCache qui exécutent des versions de Windows à partir de Windows 7 vers Windows 10. Impossible de configurer les ordinateurs qui exécutent Windows Server 2008 R2 en suivant les étapes de ce guide.

## <a name="bkmk_tech"></a>Présentations des technologies

Pour ce guide d’accompagnement, BranchCache est la seule technologie que vous avez besoin d’installer et de configurer. Vous devez exécuter les commandes BranchCache Windows PowerShell sur vos serveurs de contenu, tels que les serveurs web et de fichiers. Toutefois, il est inutile de modifier ou de reconfigurer les serveurs de contenu de toute autre manière. En outre, vous devez configurer les ordinateurs clients à l’aide de la stratégie de groupe sur vos contrôleurs de domaine qui exécutent les services AD DS sur Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache est une technologie d’optimisation étendu réseaux (étendus WAN) de la bande passante qui est incluse dans certaines éditions des systèmes d’exploitation Windows 10 et Windows Server 2016, ainsi que dans certaines éditions de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 et Windows 7.

Pour optimiser la bande passante WAN lors de l’accès au contenu sur des serveurs distants, BranchCache télécharge du contenu de client a demandé à partir de votre siège social ou les serveurs de contenu cloud hébergé et met en cache aux emplacements des filiales, permettant ainsi aux autres ordinateurs client à filiales d’accéder au contenu même localement plutôt que via le réseau étendu.

Lorsque vous déployez BranchCache en mode cache hébergé, vous devez configurer les ordinateurs clients de la filiale en tant que clients en mode cache hébergé, puis déployer un serveur de cache hébergé dans la filiale. Ce guide explique comment déployer votre serveur de cache hébergé avec du contenu préhaché et préchargé à partir de votre serveur Web et le fichier\-en fonction des serveurs de contenu.

### <a name="group-policy"></a>Stratégie de groupe

Stratégie de groupe dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 est une infrastructure utilisée pour fournir et appliquer une ou plusieurs configurations souhaitées ou les paramètres de stratégie à un ensemble d’ordinateurs dans un environnement Active Directory et les utilisateurs ciblés. 

Cette infrastructure comprend un moteur de stratégie de groupe et de client plusieurs\-extensions côté \(CSE\) sont pris en charge pour la lecture des paramètres de stratégie sur les ordinateurs clients cibles.

Dans ce scénario, la stratégie de groupe permet de configurer les ordinateurs clients membres d’un domaine avec le mode cache hébergé de BranchCache.

Pour continuer avec ce guide, consultez [BranchCache hébergé Cache Mode présentation du déploiement](2-Bc-Hcm-Deploy-Overview.md).
