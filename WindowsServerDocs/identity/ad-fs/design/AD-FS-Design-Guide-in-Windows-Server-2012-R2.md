---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guide de conception AD FS dans Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e8675c255032bca4623a9649bfc0bcca478008e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854892"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guide de conception de AD FS dans Windows Server 

Services ADFS \(AD FS\) fournit une Fédération d’identités simplifiée et sécurisée, ainsi que des\-de signature unique sur \(\) SSO pour les utilisateurs finaux qui souhaitent accéder aux applications au sein d’une AD FS\-entreprise sécurisée, dans des organisations partenaires de Fédération ou dans le Cloud.  
  
Dans Windows Server&reg; 2012 R2, AD FS inclut un service de rôle de service de Fédération qui agit en tant que fournisseur d’identité \(authentifie les utilisateurs pour fournir des jetons de sécurité aux applications qui approuvent AD FS\) ou en tant que fournisseur de Fédération \(consomme des jetons d’autres fournisseurs d’identité, puis fournit des jetons de sécurité aux applications qui approuvent AD FS\).  
  
La fonction consistant à fournir un accès extranet aux applications et services sécurisés par AD FS dans Windows Server 2012 R2 est désormais effectuée par un nouveau service de rôle d’accès à distance appelé proxy d’application web. Ceci est une nouveauté par rapport aux versions antérieures de Windows Server dans lesquelles cette fonction était traitée par un serveur proxy de fédération AD FS. Le proxy d’application Web est un rôle serveur conçu pour fournir un accès aux AD FS\-scénario extranet associé et à d’autres scénarios extranet. Pour plus d’informations sur le proxy d’application Web, consultez [Guide pas à pas du proxy d’application Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement de AD FS, en fonction des besoins de votre organisation. Ce guide est prévu pour une utilisation par un spécialiste d'infrastructure ou un architecte système. Il met en évidence vos principaux points de décision lors de la planification de votre déploiement AD FS. Avant de lire ce guide, vous devez avoir une bonne compréhension de la façon dont AD FS fonctionne sur un niveau fonctionnel. Pour plus d'informations, voir [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identifier vos objectifs de déploiement d’AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Configuration AD FS requise](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Voir aussi  
[Conception des services AD FS](../../ad-fs/AD-FS-Design.md)  
  

