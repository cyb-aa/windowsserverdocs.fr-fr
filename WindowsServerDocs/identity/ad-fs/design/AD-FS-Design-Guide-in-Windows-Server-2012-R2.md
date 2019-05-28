---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guide de conception AD FS dans Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2d15f680f28c54da75100a03f7b85e880442d9be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191739"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guide de conception AD FS dans Windows Server 

Active Directory Federation Services \(AD FS\) fournit la fédération des identités simplifiées et sécurisées et l’authentification unique Web\-sur \(SSO\) fonctionnalités pour les utilisateurs finaux qui souhaitent accéder aux applications dans un AD FS\-sécurisé d’entreprise, dans les organisations partenaires de fédération ou dans le cloud.  
  
Dans Windows Server® 2012 R2, AD FS inclut un service de rôle de service de fédération qui agit comme fournisseur d’identité \(authentifie les utilisateurs pour fournir des jetons de sécurité aux applications qui approuvent AD FS\) ou comme un fournisseur de fédération \( utilise des jetons d’autres fournisseurs d’identité et fournit des jetons de sécurité aux applications qui approuvent AD FS\).  
  
La fonction consistant à fournir un accès extranet aux applications et services sécurisés par AD FS dans Windows Server 2012 R2 est désormais effectuée par un nouveau service de rôle d’accès à distance appelé proxy d’application web. Ceci est une nouveauté par rapport aux versions antérieures de Windows Server dans lesquelles cette fonction était traitée par un serveur proxy de fédération AD FS. Proxy d’Application Web est un rôle serveur conçu pour fournir l’accès pour les services AD FS\-liés scénario extranet et autres scénarios extranets. Pour plus d’informations sur le Proxy d’Application Web, consultez [Guide de procédure pas à pas de Proxy d’Application Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement d’AD FS, selon les besoins de votre organisation. Ce guide est prévu pour une utilisation par un spécialiste d'infrastructure ou un architecte système. Il met en évidence vos principaux points de décision lorsque vous planifiez votre déploiement AD FS. Avant de lire ce guide, doit avoir une bonne compréhension du fonctionnement des services AD FS sur un niveau fonctionnel. Pour plus d'informations, voir [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identifier vos objectifs de déploiement d’AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Configuration AD FS requise](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Voir aussi  
[Conception des services AD FS](../../ad-fs/AD-FS-Design.md)  
  

