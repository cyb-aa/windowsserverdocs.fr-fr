---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: Déploiement des services AD FS dans l’organisation du partenaire de compte
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 94446ddd8d33c18b6166870a34b65c997d1644ac
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853202"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>Déploiement des services AD FS dans l’organisation du partenaire de compte

Un partenaire de compte dans Services ADFS \(AD FS\) représente l’organisation dans la relation d’approbation de Fédération qui stocke physiquement les comptes d’utilisateur dans un magasin d’attributs pris en charge. Pour plus d’informations sur les magasins d’attributs pris en charge, consultez [rôle des magasins d’attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
Le serveur de Fédération dans l’organisation partenaire de compte authentifie les utilisateurs locaux et crée des jetons de sécurité qui sont utilisés par le partenaire de ressource pour prendre des décisions d’autorisation. Les parties de confiance, telles que les sites Web et les services Web, sont ensuite en mesure de s’inscrire facilement auprès du serveur de Fédération et de consommer des jetons émis pour l’authentification et le contrôle d’accès.  
  
Dans les scénarios dans lesquels vous devez fournir à vos utilisateurs l’accès à plusieurs applications ou services fédérés, lorsque chaque application ou service est hébergé par une autre organisation, vous pouvez configurer le serveur de Fédération du partenaire de compte afin de pouvoir déployer plusieurs parties de confiance.  
  
Pour plus d’informations sur l’installation et la configuration d’une organisation partenaire de compte, voir [Checklist: Configuring the Account Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Passer en revue le rôle du serveur de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [Passer en revue le rôle du serveur proxy de fédération du partenaire de compte](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [Préparer les ordinateurs clients dans le partenaire de compte](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
