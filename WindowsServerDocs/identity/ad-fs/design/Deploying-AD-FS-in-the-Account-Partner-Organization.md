---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: Déploiement des services AD FS dans l’organisation du partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5b4ba00aa9fed1022d9c0137d05ac6240b44b276
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837910"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Déploiement des services AD FS dans l’organisation du partenaire de compte

>S'applique à : Windows Server 2016, Windows Server 2012 R2

Un partenaire de compte dans Active Directory Federation Services \(AD FS\) représente l’organisation dans la relation d’approbation de fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’attributs pris en charge. Pour plus d’informations sur l’attribut magasins sont pris en charge, consultez [The Role of Attribute Stores](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Le serveur de fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité qui sont utilisés par le partenaire de ressource dans les décisions d’autorisation. Parties de confiance telles que les sites Web et les services Web peuvent ensuite facilement s’inscrivent auprès du serveur de fédération et consommer des jetons émis pour l’authentification et contrôle d’accès.  
  
Dans les scénarios dans lesquels vous devez fournir à vos utilisateurs d’accéder à plusieurs applications ou services fédérés, lorsque chaque application ou le service est hébergé par une autre organisation, vous pouvez configurer le serveur de fédération de partenaire de compte afin que vous puissiez déployer plusieurs parties de confiance.  
  
Pour plus d’informations sur comment installer et configurer une organisation partenaire de compte, consultez [liste de vérification : Configuration de l’organisation partenaire de compte](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passez en revue le rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Passez en revue le rôle de serveur Proxy de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Préparer les ordinateurs clients dans le partenaire de compte](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
