---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Planification de votre déploiement
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0206197b24f13d80019cbc864057e99e195ebc4b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191141"
---
# <a name="planning-your-deployment"></a>Planification de votre déploiement

Lorsque vous planifiez cross\-d’organisation \(fédération\-en\) collaboration à l’aide d’Active Directory Federation Services \(AD FS\), tout d’abord déterminer si votre organisation hébergera une ressource Web accessible par d’autres organisations via Internet ou si vous souhaitez fournir l’accès à la ressource Web pour les employés de votre organisation. Ce choix affecte la façon dont vous déployez AD FS, et il est fondamental dans la planification de votre infrastructure AD FS.  
  
> [!NOTE]  
> Assurez-vous que le rôle que joue l’organisation dans l’accord de fédération est clairement compris par toutes les parties.  
  
Pour le [Federated Web SSO Design](Federated-Web-SSO-Design.md), ADFS utilise des termes tels que *partenaire de compte* \(également appelé *fournisseur d’identité* dans le composant logiciel enfichable Gestion AD FS\-dans\) et *partenaire de ressource* \(également appelé *confiance* dans le composant logiciel enfichable Gestion AD FS\-dans\) à différencier l’organisation qui héberge les comptes \(le partenaire de compte\) à partir de l’organisation qui héberge le site Web\-ressources \(le partenaire de ressource\).  
  
Dans la [Web SSO Design](Web-SSO-Design.md), l’entreprise intervient dans les rôles de compte partenaire et de ressource partenaire, car elle fournit à ses utilisateurs l’accès aux applications.  
  
Les rubriques suivantes expliquent que certains des services AD FS concepts de l’organisation du partenaire. Elles contiennent également des liens vers des rubriques qui contiennent des informations sur la configuration et la configuration des organisations partenaires de compte et des organisations partenaires de ressource en fonction de vos objectifs de déploiement AD FS dans le Guide de déploiement d’AD FS.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Meilleures pratiques pour sécuriser la planification et le déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planification de l’interopérabilité avec AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quand utiliser la délégation d’identité](When-to-Use-Identity-Delegation.md)  
  
-   [Déploiement des services AD FS dans l’organisation du partenaire de compte](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Déploiement des services de fédération Active Directory (AD FS) dans l’organisation partenaire de ressource](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


