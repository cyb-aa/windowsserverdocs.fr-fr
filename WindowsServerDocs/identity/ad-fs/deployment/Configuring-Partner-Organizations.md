---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configuration des organisations partenaires
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ac83d9754365f3ceea5b363af4df93862bdb59e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855492"
---
# <a name="configuring-partner-organizations"></a>Configuration des organisations partenaires

Pour déployer une nouvelle organisation partenaire dans Services ADFS \(AD FS\), effectuez les tâches décrites dans l’une des [listes de vérification : configuration de l’organisation du partenaire de ressource](Checklist--Configuring-the-Resource-Partner-Organization.md) ou [liste de vérification : configuration de l’organisation partenaire de compte](Checklist--Configuring-the-Account-Partner-Organization.md), en fonction de votre conception de AD FS.  
  
> [!NOTE]  
> Lorsque vous utilisez l’une de ces listes de vérification, nous vous recommandons vivement de lire au préalable les références à l’aide du partenaire de compte ou de la planification des partenaires de ressources dans la [AD FS Guide de conception de Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de passer aux procédures de configuration de la nouvelle organisation partenaire. Le fait de suivre la liste de contrôle de cette façon permet de mieux comprendre la conception complète de AD FS et le déploiement pour le partenaire de compte ou l’organisation partenaire de ressource.  
  
## <a name="about-account-partner-organizations"></a>À propos des organisations partenaires de compte  
Un partenaire de compte est l’organisation dans la relation d’approbation de Fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’attributs pris en charge par AD FS. Le partenaire de compte est chargé de collecter et d’authentifier les informations d’identification d’un utilisateur, de créer des revendications pour cet utilisateur et d’empaqueter les revendications dans des jetons de sécurité. Ces jetons peuvent ensuite être présentés dans une approbation de Fédération pour permettre l’accès aux ressources Web\-qui se trouvent dans l’organisation du partenaire de ressource.  
  
En d’autres termes, un partenaire de compte représente l’organisation dont les utilisateurs le compte\-serveur de Fédération côté serveur émet des jetons de sécurité. Le serveur de Fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité utilisés par le partenaire de ressource pour prendre des décisions d’autorisation.  
  
En ce qui concerne les magasins d’attributs, le partenaire de compte dans AD FS est conceptuellement équivalent à une seule forêt Active Directory dont les comptes ont besoin d’accéder aux ressources qui se trouvent physiquement dans une autre forêt. Les comptes de cette forêt peuvent accéder aux ressources de la forêt de ressources uniquement lorsqu’une relation d’approbation externe ou de forêt existe entre les deux forêts et que les ressources auxquelles les utilisateurs essaient d’accéder ont été définies avec les autorisations d’autorisation appropriées.  
  
## <a name="about-resource-partner-organizations"></a>À propos des organisations partenaires de ressources  
Le partenaire de ressource est l’organisation dans un déploiement AD FS où se trouvent les serveurs Web. Le partenaire de ressource approuve le partenaire de compte pour authentifier les utilisateurs. Par conséquent, pour prendre des décisions d’autorisation, le partenaire de ressource consomme les revendications qui sont empaquetées dans des jetons de sécurité provenant d’utilisateurs du partenaire de compte.  
  
En d’autres termes, un partenaire de ressource représente l’organisation dont les serveurs Web sont protégés par le serveur de Fédération côté\-des ressources. Le serveur de Fédération du partenaire de ressource utilise les jetons de sécurité produits par le partenaire de compte pour prendre des décisions d’autorisation pour les serveurs Web du partenaire de ressource.  
  
Pour fonctionner en tant que ressource AD FS, les serveurs Web de l’organisation partenaire de ressource doivent avoir Windows Identity Foundation \(WIF\) installé, ou disposer de la Services ADFS \(AD FS\) 1. x revendications\-services de rôle de l’agent Web pris en charge. Les serveurs Web qui fonctionnent comme une ressource de AD FS peuvent héberger des applications Web\-Browser\-ou des applications basées sur le service\-Web\-.  
