---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guide de conception AD FS dans Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 614bc2b4571dd8a1b35c075ae1dec6934e77e148
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408161"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guide de conception de AD FS dans Windows Server 

Services ADFS \(AD FS @ no__t-1 offre des fonctionnalités simplifiées et sécurisées de Fédération des identités et d’authentification unique Web @ no__t-2On \(SSO @ no__t-4 pour les utilisateurs finaux qui souhaitent accéder aux applications au sein d’un AD FS @ no__t-5secured entreprise, dans des organisations partenaires de Fédération ou dans le Cloud.  
  
Dans Windows Server® 2012 R2, AD FS comprend un service de rôle de service de Fédération qui agit en tant que fournisseur d’identité \(authenticates pour fournir des jetons de sécurité aux applications qui approuvent AD FS @ no__t-1 ou en tant que fournisseur de Fédération @no__t les jetons 2consumes d’autres fournisseurs d’identité, puis fournissent des jetons de sécurité aux applications qui approuvent AD FS @ no__t-3.  
  
La fonction consistant à fournir un accès extranet aux applications et services sécurisés par AD FS dans Windows Server 2012 R2 est désormais effectuée par un nouveau service de rôle d’accès à distance appelé proxy d’application web. Ceci est une nouveauté par rapport aux versions antérieures de Windows Server dans lesquelles cette fonction était traitée par un serveur proxy de fédération AD FS. Le proxy d’application Web est un rôle serveur conçu pour fournir un accès au scénario d’extranet AD FS @ no__t-0related et à d’autres scénarios extranet. Pour plus d’informations sur le proxy d’application Web, consultez [Guide pas à pas du proxy d’application Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement de AD FS, en fonction des besoins de votre organisation. Ce guide est prévu pour une utilisation par un spécialiste d'infrastructure ou un architecte système. Il met en évidence vos principaux points de décision lors de la planification de votre déploiement AD FS. Avant de lire ce guide, vous devez avoir une bonne compréhension de la façon dont AD FS fonctionne sur un niveau fonctionnel. Pour plus d'informations, voir [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identifier vos objectifs de déploiement d’AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Configuration AD FS requise](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Voir aussi  
[Conception des services AD FS](../../ad-fs/AD-FS-Design.md)  
  

