---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: "Planification de votre déploiement"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c7cec9ad92605f3dc98f8ce8fb7853a7ae61299
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-your-deployment"></a>Planification de votre déploiement

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Lorsque vous planifiez des collaboration \(federation\-based\) une nouveauté d’organisation à l’aide d’ActiveDirectory Federation Services \(ADFS\), d’abord déterminer si votre organisation hébergera une ressource Web accessible par d’autres organisations via Internet ou si vous fournirez l’accès à la ressource Web pour les employés de votre organisation. Ce choix affecte la façon dont vous déployez ADFS, et il est fondamental dans la planification de votre infrastructure ADFS.  
  
> [!NOTE]  
> Assurez-vous que le rôle que joue l’organisation dans l’accord de fédération est clairement compris par toutes les parties.  
  
Pour le [Federated Web SSO Design](Federated-Web-SSO-Design.md), ADFS utilise des termes tels que *partenaire de compte* \ (également appelés *fournisseur d’identité* dans les composants de gestion ADFS-ou) et *partenaire de ressource* \ (également appelés *partie de confiance* dans les composants de gestion ADFS-ou) pour différencier l’organisation qui héberge les comptes \(the account partner\) à partir de l’organisation qui héberge les ressources de la console Web \(the resource partner\).  
  
Dans le [conception SSO de Web](Web-SSO-Design.md), l’entreprise intervient dans les rôles de compte partenaire et de ressource partenaire, car il fournit ses utilisateurs d’accéder à ses applications.  
  
Les rubriques suivantes expliquent que certains les services ADFS concepts d’organisation de partenaire. Ils contiennent également des liens vers des rubriques qui contiennent des informations sur l’installation et la configuration des organisations partenaires de compte et des organisations partenaires de ressource en fonction de vos objectifs de déploiement ADFS dans le Guide de déploiement d’ADFS.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Meilleures pratiques pour sécuriser la planification et déploiement d’ADFS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planification de l’interopérabilité avec ADFS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quand utiliser la délégation d’identité](When-to-Use-Identity-Delegation.md)  
  
-   [Déploiement d’ADFS dans l’organisation partenaire de compte](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Déploiement d’ADFS dans l’organisation partenaire de ressource](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


