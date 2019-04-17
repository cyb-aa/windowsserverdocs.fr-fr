---
title: Le Mode de Cache hébergé de BranchCache planification du déploiement
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bc44a7db-f7a5-4e95-9d95-ab8d334e885f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e645dd96ec85e3a23df6717cfa43d7627cb938e7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-hosted-cache-mode-deployment-planning"></a>Le Mode de Cache hébergé de BranchCache planification du déploiement

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser cette rubrique pour planifier votre déploiement de BranchCache en mode de Cache hébergé.

>[!IMPORTANT]
>Votre serveur de cache hébergé doive exécuter Windows Server2016, Windows Server2012R2 ou Windows Server2012.

Avant de déployer votre serveur de cache hébergé, vous devez planifier les éléments suivants:

- [Planifier la configuration du serveur de base](#bkmk_basic)

- [Planifier l’accès au domaine](#bkmk_domain)

- [Planifiez l’emplacement et la taille du cache hébergé](#bkmk_cachelocation)

- [Planifier le partage sur lequel les packages du serveur de contenu doivent être copiés](#bkmk_package)

- [Plan préhacher et données de création sur les serveurs de contenu du package](#bkmk_prehash)

## <a name="bkmk_basic"></a>Planifier la configuration du serveur de base
  
Si vous envisagez d’utiliser un serveur existant dans votre filiale en tant que votre serveur de cache hébergé, il est inutile d’effectuer cette étape de planification, car l’ordinateur est déjà nommé et possède une configuration d’adresse IP.

Après avoir installé Windows Server2016 sur votre serveur de cache hébergé, vous devez renommer l’ordinateur et affecter et configurer une adresse IP statique pour l’ordinateur local.

>[!NOTE]
>Dans ce guide, le serveur de cache hébergé est nommé HCS1, toutefois, vous devez utiliser un nom de serveur qui est approprié pour votre déploiement.

## <a name="bkmk_domain"></a>Planifier l’accès au domaine

Si vous envisagez d’utiliser un serveur existant dans votre filiale en tant que votre serveur de cache hébergé, il est inutile d’effectuer cette étape de planification, sauf si l’ordinateur n’est pas actuellement joint au domaine.
  
Pour vous connecter au domaine, l’ordinateur doit être un ordinateur membre du domaine et le compte d’utilisateur doit être créé dans ADDS avant la tentative d’ouverture de session. En outre, vous devez joindre l’ordinateur au domaine avec un compte qui dispose de l’appartenance au groupe approprié.

## <a name="bkmk_cachelocation"></a>Planifiez l’emplacement et la taille du cache hébergé

Sur HCS1, déterminez l’emplacement sur votre serveur de cache hébergé localiser le cache hébergé. Par exemple, choisir le disque dur, le volume et l’emplacement du dossier dans lequel vous envisagez de stocker le cache.

En outre, décider quel est le pourcentage d’espace disque que vous souhaitez allouer au cache hébergé.

## <a name="bkmk_package"></a>Planifier le partage sur lequel les packages du serveur de contenu doivent être copiés

Une fois que vous créez des packages de données sur vos serveurs de contenu, vous devez les copier sur le réseau à un partage sur votre serveur de cache hébergé.

Planifiez l’emplacement du dossier et autorisations de partage pour le dossier partagé. En outre, si vos serveurs de contenu hébergent une grande quantité de données et les packages que vous créez seront des fichiers volumineux, envisagez d’effectuer l’opération de copie pendant les heures off\ – pointe afin que la bande passante réseau étendue n’est pas utilisée par l’opération de copie à un moment lorsque d’autres doivent utiliser la bande passante pour les opérations normales.

## <a name="bkmk_prehash"></a>Plan préhacher et données de création sur les serveurs de contenu du package

Avant de vous préhacher le contenu sur vos serveurs de contenu, vous devez identifier les dossiers et fichiers qui contiennent le contenu que vous souhaitez ajouter au package de données. 

En outre, vous devez planifier sur l’emplacement du dossier local dans lequel vous pouvez stocker les packages de données avant de les copier sur le serveur de cache hébergé.

Pour continuer avec ce guide, consultez [déploiement du Mode de Cache hébergé BranchCache](4-Bc-Hcm-Deployment.md).
