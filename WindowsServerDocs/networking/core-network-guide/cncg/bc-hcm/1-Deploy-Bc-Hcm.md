---
title: Déployer le mode de cache hébergé de BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1da6df19933d3a4b9866b0428fb0088ac5f862b9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319095"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Déployer le mode de cache hébergé de BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le Guide du réseau de base Windows Server 2016 fournit des instructions pour la planification et le déploiement des composants principaux requis pour un réseau pleinement fonctionnel et un nouveau Active Directory&reg; domaine dans une nouvelle forêt.

Ce guide explique comment créer sur le réseau de base en fournissant des instructions pour le déploiement de BranchCache en mode de cache hébergé dans une ou plusieurs filiales avec un contrôleur de domaine en lecture\-seul sur lequel les ordinateurs clients exécutent Windows&reg; 10, Windows 8.1 ou Windows 8, et sont joints au domaine.

>[!IMPORTANT]
>N’utilisez pas ce guide si vous envisagez de déployer ou si vous avez déjà déployé un serveur de cache hébergé de BranchCache qui exécute Windows Server 2008 R2. Ce guide fournit des instructions pour déployer le mode de cache hébergé sur un serveur de cache hébergé qui exécute Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.

Ce guide contient les sections suivantes.

- [Conditions préalables à l’utilisation de ce guide](#bkmk_pre)

- [À propos de ce guide](#bkmk_about)

- [Ce que ce guide ne contient pas](#bkmk_not)

- [Vues d’ensemble de la technologie](#bkmk_tech)

- [Vue d’ensemble du déploiement en mode cache hébergé de BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Planification du déploiement en mode de cache hébergé de BranchCache](3-Bc-Hcm-Plan.md)

- [Déploiement en mode de cache hébergé de BranchCache](4-Bc-Hcm-Deployment.md)

- [Ressources supplémentaires](11-Bc-Hcm-additional-resources.md)

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>Conditions préalables à l’utilisation de ce guide

Il s’agit d’un guide d’accompagnement du Guide du réseau de base de Windows Server 2016. Pour déployer BranchCache en mode cache hébergé à l’aide de ce guide, vous devez tout d’abord procéder comme suit.

- Déployez un réseau de base dans votre siège social à l’aide du Guide du réseau de base ou installez les technologies indiquées dans le Guide du réseau de base et faites en sorte qu’elles fonctionnent correctement sur votre réseau. Ces technologies incluent TCP\/IP V4, DHCP, Active Directory Domain Services \(AD DS\)et DNS.

    > [!NOTE]
    > Le [Guide du réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) windows server 2016 est disponible dans la bibliothèque technique de windows server 2016.  

- Déployez des serveurs de contenu BranchCache qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 dans votre siège social ou dans un centre de données Cloud. Pour plus d’informations sur le déploiement de serveurs de contenu BranchCache, consultez [ressources supplémentaires](11-Bc-Hcm-additional-resources.md).

- Établissez un réseau étendu \(des connexions de réseau étendu\) entre votre succursale, votre siège social et, le cas échéant, vos ressources Cloud, à l’aide d’un réseau privé virtuel \(\)VPN, DirectAccess ou une autre méthode de connexion.

- Déployer des ordinateurs clients dans votre filiale qui exécutent l’un des systèmes d’exploitation suivants, qui fournissent BranchCache avec prise en charge de Service de transfert intelligent en arrière-plan (BITS), du protocole HTTP (Hyper Text Transfer Protocol) et du protocole SMB (Server Message Block) .
    - Windows 10 Entreprise
    - Windows 10 Éducation
    - Windows 8.1 Entreprise
    - Windows 8 Entreprise

> [!NOTE]
> Dans les systèmes d’exploitation suivants, BranchCache ne prend pas en charge les fonctionnalités HTTP et SMB, mais prend en charge la fonctionnalité des BITS BranchCache.
>     - Windows 10 professionnel, prise en charge de BITS uniquement
>     - Windows 8.1 Pro, prise en charge de BITS uniquement
>     - Windows 8 professionnel, prise en charge de BITS uniquement

## <a name="about-this-guide"></a><a name="bkmk_about"></a>À propos de ce guide

Ce guide est conçu pour les administrateurs réseau et système qui ont suivi les instructions du Guide du réseau de base Windows Server 2016 ou du Guide du réseau de base Windows Server 2012 pour déployer un réseau de base, ou pour ceux qui ont déjà déployé les technologies incluses dans le Guide du réseau de base, notamment Active Directory Domain Services \(AD DS\), service de nom de domaine \(DNS\), protocole de configuration d’hôte dynamique\(\)\/

Il est recommandé de consulter les guides de conception et de déploiement pour chacune des technologies utilisées dans ce scénario de déploiement. Ces guides peuvent vous aider à déterminer si ce scénario de déploiement fournit les services et la configuration dont vous avez besoin pour le réseau de votre organisation.

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>Ce que ce guide ne fournit pas

Ce guide ne fournit pas d’informations conceptuelles sur BranchCache, notamment des informations sur ses fonctionnalités et ses modes.  

Il ne fournit pas d’informations sur la façon de déployer des connexions WAN ou d’autres technologies dans votre filiale, telles que DHCP, un contrôleur de domaine en lecture seule ou un serveur VPN.

Ce guide ne donne pas non plus de conseils sur le matériel à utiliser lorsque vous déployez un serveur de cache hébergé. Il est possible d’exécuter d’autres services et applications sur votre serveur de cache hébergé, toutefois, vous devez déterminer, en fonction de la charge de travail, des capacités matérielles et de la taille de la filiale s’il convient d’installer le serveur de cache hébergé BranchCache sur un ordinateur en particulier et la quantité d’espace disque à allouer au cache.  
Ce guide ne fournit pas d’instructions pour la configuration d’ordinateurs qui exécutent Windows 7. Si vous avez des ordinateurs clients qui exécutent Windows 7 dans vos filiales, vous devez les configurer à l’aide de procédures différentes de celles fournies dans ce guide pour les ordinateurs clients qui exécutent Windows 10, Windows 8.1 et Windows 8.
  
En outre, si vous avez des ordinateurs exécutant Windows 7, vous devez configurer votre serveur de cache hébergé avec un certificat de serveur émis par une autorité de certification approuvée par les ordinateurs clients. \(si tous vos ordinateurs clients exécutent Windows 10, Windows 8.1 ou Windows 8, vous n’avez pas besoin de configurer le serveur de cache hébergé avec un certificat de serveur.\) 
> [!IMPORTANT]
> Si vos serveurs de cache hébergé exécutent Windows Server 2008 R2, utilisez le [Guide de déploiement](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) de windows Server 2008 R2 BranchCache au lieu de ce guide pour déployer BranchCache en mode de cache hébergé. Appliquez les paramètres de stratégie de groupe décrits dans ce guide à tous les clients BranchCache qui exécutent des versions de Windows à partir de Windows 7 vers Windows 10. Les ordinateurs qui exécutent Windows Server 2008 R2 ne peuvent pas être configurés à l’aide des étapes décrites dans ce guide.

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>Vues d’ensemble de la technologie

Pour ce guide d’accompagnement, BranchCache est la seule technologie que vous avez besoin d’installer et de configurer. Vous devez exécuter les commandes BranchCache Windows PowerShell sur vos serveurs de contenu, tels que les serveurs web et de fichiers. Toutefois, il est inutile de modifier ou de reconfigurer les serveurs de contenu de toute autre manière. En outre, vous devez configurer les ordinateurs clients à l’aide de stratégie de groupe sur vos contrôleurs de domaine qui exécutent AD DS sur Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache est une technologie d’optimisation de la bande passante du réseau étendu (WAN), incluse dans certaines éditions des systèmes d’exploitation Windows Server 2016 et Windows 10, ainsi que dans certaines éditions de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 et Windows 7.

Pour optimiser la bande passante WAN quand les utilisateurs accèdent à du contenu sur des serveurs distants, BranchCache télécharge le contenu demandé par le client à partir de votre bureau principal ou des serveurs de contenu du Cloud hébergé et met en cache le contenu dans les succursales, ce qui permet à d’autres ordinateurs clients à des succursales pour accéder au même contenu localement plutôt que via le réseau étendu.

Lorsque vous déployez BranchCache en mode cache hébergé, vous devez configurer les ordinateurs clients de la filiale en tant que clients en mode cache hébergé, puis déployer un serveur de cache hébergé dans la filiale. Ce guide montre comment déployer votre serveur de cache hébergé avec un contenu préhaché et préchargé à partir de vos serveurs Web et de serveur de fichiers\-basés sur des serveurs de contenu.

### <a name="group-policy"></a>Stratégie de groupe

Stratégie de groupe dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 est une infrastructure utilisée pour fournir et appliquer une ou plusieurs configurations ou paramètres de stratégie souhaités à un ensemble d’utilisateurs et d’ordinateurs ciblés au sein d’un environnement Active Directory. 

Cette infrastructure se compose d’un moteur de stratégie de groupe et de plusieurs extensions côté client\-\(CSE\) qui sont responsables de la lecture des paramètres de stratégie sur les ordinateurs clients cibles.

Dans ce scénario, la stratégie de groupe permet de configurer les ordinateurs clients membres d’un domaine avec le mode cache hébergé de BranchCache.

Pour continuer ce guide, consultez [vue d’ensemble du déploiement en mode de cache hébergé de BranchCache](2-Bc-Hcm-Deploy-Overview.md).
