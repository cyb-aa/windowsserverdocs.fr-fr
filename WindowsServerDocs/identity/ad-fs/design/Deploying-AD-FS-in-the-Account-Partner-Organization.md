---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: Déploiement des services AD FS dans l’organisation du partenaire de compte
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 63c080904482814f9f62451e8e7cfa4862d19927
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359253"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Déploiement des services AD FS dans l’organisation du partenaire de compte

Un partenaire de compte dans Services ADFS \(AD FS @ no__t-1 représente l’organisation dans la relation d’approbation de Fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’attributs pris en charge. Pour plus d’informations sur les magasins d’attributs pris en charge, consultez [rôle des magasins d’attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Le serveur de Fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité qui sont utilisés par le partenaire de ressource pour prendre des décisions d’autorisation. Les parties de confiance, telles que les sites Web et les services Web, sont ensuite en mesure de s’inscrire facilement auprès du serveur de Fédération et de consommer des jetons émis pour l’authentification et le contrôle d’accès.  
  
Dans les scénarios dans lesquels vous devez fournir à vos utilisateurs l’accès à plusieurs applications ou services fédérés, lorsque chaque application ou service est hébergé par une autre organisation, vous pouvez configurer le serveur de Fédération du partenaire de compte afin de pouvoir déployer plusieurs parties de confiance.  
  
Pour plus d’informations sur la configuration et la configuration d’une organisation partenaire de compte, voir [Checklist : Configuration de l’organisation partenaire de compte @ no__t-0.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passer en revue le rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Passer en revue le rôle du serveur proxy de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Préparer les ordinateurs clients dans le partenaire de compte](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
