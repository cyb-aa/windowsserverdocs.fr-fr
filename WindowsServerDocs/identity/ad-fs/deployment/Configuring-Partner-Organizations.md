---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configuration des organisations partenaires
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-partner-organizations"></a>Configuration des organisations partenaires

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour déployer une nouvelle organisation partenaire dans ActiveDirectory Federation Services \(ADFS\), effectuez les tâches d’une [liste de vérification: configuration de l’organisation partenaire de ressource](Checklist--Configuring-the-Resource-Partner-Organization.md) ou [liste de vérification: configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md), en fonction de votre conception ADFS.  
  
> [!NOTE]  
> Lorsque vous utilisez une de ces listes de vérification, nous recommandons vivement de lire les références au partenaire de compte ou de partenaire de ressource planification dans le [Guide de conception ADFS dans Windows Server2012](https://technet.microsoft.com/library/dd807036.aspx) avant de poursuivre vers les procédures de configuration de l’organisation partenaire de nouveau. Suivant la liste de vérification de cette façon aidera à fournir une meilleure compréhension de l’article de conception et de déploiement de services ADFS complète pour partenaire ou une ressource organisation partenaire de compte.  
  
## <a name="about-account-partner-organizations"></a>À propos des organisations partenaires de compte  
Un partenaire de compte est l’organisation dans la relation d’approbation de fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’ADFS: la prise en charge un attribut. Le partenaire de compte est responsable de la collecte et l’authentification des informations d’identification d’un utilisateur, créant des revendications pour cet utilisateur et créer un package pour les revendications dans des jetons de sécurité. Ces jetons peuvent ensuite être présentés dans une approbation de fédération pour permettre l’accès aux ressources de console Web qui se trouvent dans l’organisation partenaire de ressource.  
  
En d’autres termes, un partenaire de compte représente l’organisation pour les utilisateurs dont le serveur de fédération account\ côté émet des jetons de sécurité. Le serveur de fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité qui utilise le partenaire de ressource dans les décisions d’autorisation.  
  
En ce qui concerne les magasins d’attributs, le partenaire de compte dans ADFS concept équivaut à une seule forêt ActiveDirectory dont les comptes doivent accéder aux ressources qui se trouvent physiquement dans une autre forêt. Comptes dans cette forêt peuvent accéder aux ressources dans la forêt de ressources uniquement lorsqu’une approbation externe ou forêt relation existe entre les deux forêts et les ressources auxquelles les utilisateurs sont tente d’accéder au ont été définies avec les autorisations appropriées.  
  
## <a name="about-resource-partner-organizations"></a>À propos des organisations partenaires de ressource  
Le partenaire de ressource est l’organisation dans un déploiement d’ADFS où se trouvent les serveurs Web. Le partenaire de ressource approuve le partenaire de compte pour authentifier les utilisateurs. Par conséquent, pour prendre des décisions d’autorisation, le partenaire de ressource consomme les revendications qui sont empaquetées dans des jetons de sécurité qui proviennent d’utilisateurs du partenaire de compte.  
  
En d’autres termes, un partenaire de ressource représente l’organisation dont les serveurs Web sont protégés par le serveur de fédération resource\ côté. Le serveur de fédération du partenaire de ressource utilise les jetons de sécurité produits par le partenaire de compte pour prendre des décisions d’autorisation pour les serveurs Web dans le partenaire de ressource.  
  
De fonctionner comme une ressource ADFS, les serveurs Web dans l’organisation partenaire de ressource doit avoir \(WIF\) WindowsIdentityFoundation installé ou ont les services de rôle Services de fédération ActiveDirectory \(ADFS\) 1.x Claims\-Aware Web Agent installé. Serveurs Web qui fonctionnent en tant que ressource ADFS peuvent héberger des applications de console browser\ Web soit service\ de console Web.  
