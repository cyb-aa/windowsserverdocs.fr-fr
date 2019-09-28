---
title: Planification du déploiement du mode de cache hébergé BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0fe55bc9971606559af652d592a91db7a89544a7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356364"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Planification du déploiement du mode de cache hébergé BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous pouvez utiliser cette rubrique pour planifier le déploiement de BranchCache en mode de cache hébergé.

>[!IMPORTANT]
>Votre serveur de cache hébergé doit exécuter Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

Avant de déployer votre serveur de cache hébergé, vous devez planifier les éléments suivants :

- [Planifier la configuration du serveur de base](#bkmk_basic)

- [Planifier l’accès au domaine](#bkmk_domain)

- [Planifier l’emplacement et la taille du cache hébergé](#bkmk_cachelocation)

- [Planifier le partage vers lequel les packages de serveur de contenu doivent être copiés](#bkmk_package)

- [Planifier le préhachage et la création de packages de données sur les serveurs de contenu](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planifier la configuration du serveur de base
  
Si vous envisagez d’utiliser un serveur existant dans votre succursale en tant que serveur de cache hébergé, vous n’avez pas besoin d’effectuer cette étape de planification, car l’ordinateur est déjà nommé et a une configuration d’adresse IP.

Après avoir installé Windows Server 2016 sur votre serveur de cache hébergé, vous devez renommer l’ordinateur et affecter et configurer une adresse IP statique pour l’ordinateur local.

>[!NOTE]
>Dans ce guide, le serveur de cache hébergé est nommé HCS1, mais vous devez utiliser un nom de serveur approprié pour votre déploiement.

## <a name="bkmk_domain"></a>Planifier l’accès au domaine

Si vous envisagez d’utiliser un serveur existant dans votre succursale en tant que serveur de cache hébergé, vous n’avez pas besoin d’effectuer cette étape de planification, sauf si l’ordinateur n’est pas actuellement joint au domaine.
  
Pour ouvrir une session sur le domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans AD DS avant la tentative d’ouverture de session. En outre, vous devez joindre l’ordinateur au domaine avec un compte disposant de l’appartenance de groupe appropriée.

## <a name="bkmk_cachelocation"></a>Planifier l’emplacement et la taille du cache hébergé

Sur HCS1, déterminez où vous souhaitez localiser le cache hébergé sur votre serveur de cache hébergé. Par exemple, déterminez l’emplacement du disque dur, du volume et du dossier dans lequel vous envisagez de stocker le cache.

En outre, déterminez le pourcentage d’espace disque que vous souhaitez allouer au cache hébergé.

## <a name="bkmk_package"></a>Planifier le partage vers lequel les packages de serveur de contenu doivent être copiés

Une fois que vous avez créé des packages de données sur vos serveurs de contenu, vous devez les copier sur le réseau vers un partage sur votre serveur de cache hébergé.

Planifiez l’emplacement du dossier et les autorisations de partage pour le dossier partagé. En outre, si vos serveurs de contenu hébergent une grande quantité de données et que les packages que vous créez seront des fichiers volumineux, envisagez d’effectuer l’opération de copie en dehors des heures de pointe, afin que la bande passante du réseau étendu ne soit pas consommée par l’opération de copie pendant le moment où d’autres utilisateurs doivent utiliser  la bande passante pour les opérations commerciales normales.

## <a name="bkmk_prehash"></a>Planifier le préhachage et la création de packages de données sur les serveurs de contenu

Avant de préhacher du contenu sur vos serveurs de contenu, vous devez identifier les dossiers et les fichiers qui contiennent le contenu que vous souhaitez ajouter au package de données. 

En outre, vous devez prévoir l’emplacement du dossier local dans lequel vous pouvez stocker les packages de données avant de les copier sur le serveur de cache hébergé.

Pour continuer ce guide, consultez [déploiement du mode de cache hébergé de BranchCache](4-Bc-Hcm-Deployment.md).
