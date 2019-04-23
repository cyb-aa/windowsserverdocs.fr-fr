---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configuration des organisations partenaires
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875180"
---
# <a name="configuring-partner-organizations"></a>Configuration des organisations partenaires

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour déployer une nouvelle organisation partenaire dans Active Directory Federation Services \(AD FS\), effectuez les tâches dans le [liste de vérification : Configuration de l’organisation partenaire de ressource](Checklist--Configuring-the-Resource-Partner-Organization.md) ou [liste de vérification : Configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md), en fonction de votre conception AD FS.  
  
> [!NOTE]  
> Lorsque vous utilisez une de ces listes de vérification, nous recommandons fortement de lire les références au partenaire de compte ou partenaire de ressource planification dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de passer à la procédures de paramétrage de la nouvelle organisation partenaire. La liste de vérification de cette façon permet une meilleure compréhension de l’histoire de conception et de déploiement de AD FS complète pour l’organisation partenaire du partenaire ou de la ressource de compte.  
  
## <a name="about-account-partner-organizations"></a>À propos des organisations partenaires de compte  
Un partenaire de compte est l’organisation dans la relation d’approbation de fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’AD FS : pris en charge un attribut. Le partenaire de compte est responsable de la collecte et l’authentification des informations d’identification d’un utilisateur, créant des revendications pour cet utilisateur et empaquetage des déclarations dans les jetons de sécurité. Ces jetons peuvent ensuite être présentés dans une approbation de fédération pour permettre l’accès au Web\-en fonction des ressources qui sont trouvent dans l’organisation partenaire ressource.  
  
En d’autres termes, un partenaire de compte représente l’organisation pour les utilisateurs dont le compte\-serveur de fédération côté émet des jetons de sécurité. Le serveur de fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité qui utilise le partenaire de ressource dans les décisions d’autorisation.  
  
En ce qui concerne les magasins d’attributs, le partenaire de compte dans AD FS est conceptuellement équivalent à une seule forêt Active Directory dont les comptes ont besoin d’accéder aux ressources qui se trouvent physiquement dans une autre forêt. Comptes dans cette forêt peuvent accéder aux ressources dans la forêt de ressources uniquement lorsqu’une approbation externe ou une relation existe entre les deux forêts et les ressources auxquelles les utilisateurs sont essaient d’accéder ont été définies avec l’autorisation appropriée d’approbation de forêt autorisations.  
  
## <a name="about-resource-partner-organizations"></a>À propos des organisations partenaires de ressource  
Le partenaire de ressource est l’organisation dans un déploiement AD FS où se trouvent les serveurs Web. Le partenaire de ressource approuve le partenaire de compte pour authentifier les utilisateurs. Par conséquent, pour prendre les décisions d’autorisation, le partenaire de ressource utilise les déclarations empaquetées dans les jetons de sécurité provenant d’utilisateurs du partenaire de compte.  
  
En d’autres termes, un partenaire de ressource représente l’organisation dont les serveurs Web sont protégés par la ressource\-serveur de fédération de côté. Le serveur de fédération du partenaire de ressource utilise les jetons de sécurité qui sont produites par le partenaire de compte pour prendre des décisions d’autorisation pour les serveurs Web dans le partenaire de ressource.  
  
Pour fonctionner comme ressource ADFS, les serveurs Web dans l’organisation partenaire de ressource doivent avoir Windows Identity Foundation \(WIF\) avez installé ou les Services de fédération Active Directory \(AD FS\) 1.x Revendications\-installés les services de rôle Agent Web prenant en charge. Serveurs qui fonctionnent comme une ressource ADFS peut héberger soit Web Web\-navigateur\-Web ou une\-service\-en fonction des applications.  
