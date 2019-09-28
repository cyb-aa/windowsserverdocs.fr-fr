---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Planification de votre déploiement
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 607dc34c8f44d8d96a8dc0c9d1ed004edc799167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407996"
---
# <a name="planning-your-deployment"></a>Planification de votre déploiement

Lorsque vous planifiez la collaboration entre @ no__t-0organizational \(federation @ no__t-2based @ no__t-3 à l’aide d’Services ADFS \(AD FS @ no__t-5, déterminez d’abord si votre organisation doit héberger une ressource Web accessible par d’autres organisations sur Internet ou si vous fournissez un accès à la ressource Web pour les employés de votre organisation. Cette détermination affecte la façon dont vous déployez AD FS, et il est fondamental dans la planification de votre infrastructure AD FS.  
  
> [!NOTE]  
> Assurez-vous que le rôle que joue l’organisation dans l’accord de fédération est clairement compris par toutes les parties.  
  
Pour la [conception SSO de Web fédéré](Federated-Web-SSO-Design.md), AD FS utilise des termes tels que le *partenaire de compte* \(also appelé « *fournisseur d’identité* » dans le composant logiciel enfichable de gestion des AD FS de gestion @ no__t-fois WVGA @ no__t-5 et le *partenaire de ressource* \(also appelé  *partie de confiance* dans le composant logiciel enfichable de gestion de AD FS @ no__t-9in @ no__t-10 pour aider à différencier l’organisation qui héberge les comptes 1La compte partenaire @ no__t-12 de l’organisation qui héberge les ressources Web @ no__t-13based 4 ressources partenaire @ no__t-15.  
  
Dans la [Web SSO Design](Web-SSO-Design.md), l’entreprise intervient dans les rôles de compte partenaire et de ressource partenaire, car elle fournit à ses utilisateurs l’accès aux applications.  
  
Les rubriques suivantes expliquent certains concepts de l’organisation AD FS partenaire. Ils contiennent également des liens vers des rubriques du Guide de déploiement AD FS qui contiennent des informations sur la configuration et la configuration des organisations partenaires de compte et des organisations partenaires de ressource en fonction de vos objectifs de déploiement AD FS.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Meilleures pratiques pour sécuriser la planification et le déploiement d’AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planification de l’interopérabilité avec AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quand utiliser la délégation d’identité](When-to-Use-Identity-Delegation.md)  
  
-   [Déploiement des services AD FS dans l’organisation du partenaire de compte](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Déploiement des services de fédération Active Directory (AD FS) dans l’organisation partenaire de ressource](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


