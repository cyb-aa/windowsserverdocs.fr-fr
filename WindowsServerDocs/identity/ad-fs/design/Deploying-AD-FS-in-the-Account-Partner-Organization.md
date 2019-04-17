---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: "Déploiement d’ADFS dans l’organisation partenaire de compte"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5b4ba00aa9fed1022d9c0137d05ac6240b44b276
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Déploiement d’ADFS dans l’organisation partenaire de compte

>S’applique à: Windows Server2016, Windows Server2012R2

Un partenaire de compte dans ActiveDirectory Federation Services \(ADFS\) représente l’organisation dans la relation d’approbation de fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’attributs pris en charge. Pour plus d’informations sur l’attribut magasins sont pris en charge, voir [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Le serveur de fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité qui sont utilisés par le partenaire de ressource dans les décisions d’autorisation. Parties de confiance telles que les sites Web et les services Web sont alors en mesure de s’inscrire avec le serveur de fédération et d’utiliser facilement des jetons pour l’authentification et contrôle d’accès émis.  
  
Dans les scénarios dans lesquels vous devez fournir à vos utilisateurs d’accéder à plusieurs applications ou services fédérés, lorsque chaque application ou le service est hébergé par une autre organisation, vous pouvez configurer le serveur de fédération de partenaire de compte afin que vous pouvez déployer plusieurs parties de confiance.  
  
Pour plus d’informations sur comment installer et configurer une organisation partenaire de compte, voir [liste de vérification: configuration de l’organisation partenaire de compte](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passez en revue le rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Passez en revue le rôle de serveur Proxy de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Préparer les ordinateurs clients du partenaire de compte](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
