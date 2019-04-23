---
title: Planification du déploiement du mode de cache hébergé BranchCache
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7232f8732e7476b955115741b5582a585dc6068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890680"
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Planification du déploiement du mode de cache hébergé BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette rubrique pour planifier votre déploiement de BranchCache en mode de Cache hébergé.

>[!IMPORTANT]
>Votre serveur de cache hébergé doit être en cours d’exécution pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

Avant de déployer votre serveur de cache hébergé, vous devez planifier les éléments suivants :

- [Planifier la configuration du serveur de base](#bkmk_basic)

- [Planifier l’accès de domaine](#bkmk_domain)

- [Planifier l’emplacement et la taille du cache hébergé](#bkmk_cachelocation)

- [Planifier le partage dans lequel les packages du serveur de contenu doivent être copiés](#bkmk_package)

- [Création sur les serveurs de contenu du paquet plan hachage préalable et données](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planifier la configuration du serveur de base
  
Si vous envisagez d’utiliser un serveur existant dans votre filiale en tant que votre serveur de cache hébergé, il est inutile d’effectuer cette étape de planification, car l’ordinateur est déjà nommé et qu’il dispose d’une configuration d’adresse IP.

Après avoir installé Windows Server 2016 sur votre serveur de cache hébergé, vous devez renommer l’ordinateur et affecter et configurer une adresse IP statique pour l’ordinateur local.

>[!NOTE]
>Dans ce guide, le serveur de cache hébergé est nommé HCS1, toutefois, vous devez utiliser un nom de serveur qui convient à votre déploiement.

## <a name="bkmk_domain"></a>Planifier l’accès de domaine

Si vous envisagez d’utiliser un serveur existant dans votre filiale en tant que votre serveur de cache hébergé, il est inutile d’effectuer cette étape de planification, à moins que l’ordinateur n’est pas actuellement joint au domaine.
  
Pour vous connecter au domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans AD DS avant la tentative d’ouverture de session. En outre, vous devez joindre l’ordinateur au domaine avec un compte qui dispose de l’appartenance au groupe approprié.

## <a name="bkmk_cachelocation"></a>Planifier l’emplacement et la taille du cache hébergé

Sur HCS1, déterminez où vous souhaitez placer le cache hébergé sur votre serveur de cache hébergé. Par exemple, décider le disque dur, le volume et l’emplacement du dossier dans lequel vous envisagez de stocker le cache.

En outre, décider quel pourcentage d’espace disque que vous souhaitez allouer au cache hébergé.

## <a name="bkmk_package"></a>Planifier le partage dans lequel les packages du serveur de contenu doivent être copiés

Une fois que vous créez des packages de données sur vos serveurs de contenu, vous devez les copier sur le réseau à un partage sur votre serveur de cache hébergé.

Planifiez l’emplacement du dossier et autorisations de partage pour le dossier partagé. En outre, si vos serveurs de contenu hébergent une grande quantité de données et les packages que vous créez seront des fichiers volumineux, envisagez d’effectuer l’opération de copie pendant les heures off\ – pointe afin que la bande passante WAN n’est pas utilisée par l’opération de copie pendant une période lorsque d’autres doivent utiliser  la bande passante pour les opérations normales.

## <a name="bkmk_prehash"></a>Création sur les serveurs de contenu du paquet plan hachage préalable et données

Avant de vous préhacher le contenu sur vos serveurs de contenu, vous devez identifier les dossiers et fichiers qui contiennent du contenu que vous souhaitez ajouter au package de données. 

En outre, vous devez planifier sur l’emplacement du dossier local dans lequel vous pouvez stocker les packages de données avant de les copier sur le serveur de cache hébergé.

Pour continuer avec ce guide, consultez [déploiement en Mode Cache hébergé BranchCache](4-Bc-Hcm-Deployment.md).
