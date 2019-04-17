---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guide de conception ADFS dans Windows Server2012R2
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Guide de conception ADFS dans Windows Server2012R2

>S’applique à: Windows Server2016, Windows Server2012R2

ActiveDirectory Federation Services \(ADFS\) fournit la fédération des identités simplifiées et sécurisées et seul sur connexion \(SSO\) fonctionnalités Web pour les utilisateurs finaux qui souhaitent accéder aux applications dans une entreprise sécurisée via ADFS\, dans les organisations partenaires de fédération ou dans le cloud.  
  
Dans Windows Server® 2012R2, ADFS inclut un service de rôle de service de fédération qui agit en tant que fournisseur d’identité \ (authentifie les utilisateurs pour fournir des jetons de sécurité aux applications qui approuvent ADFS\) ou en tant qu’un fournisseur de fédération \ (utilise des jetons d’autres fournisseurs d’identité et fournit des jetons de sécurité aux applications qui approuvent ADFS\).  
  
La fonction de fournir un accès extranet aux applications et services sécurisés par ADFS dans Windows Server2012R2 est désormais effectuée par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Ceci est une nouveauté à partir de versions antérieures de Windows Server dans lequel cette fonction était traitée par un serveur proxy de fédération ADFS. Proxy d’Application Web est un rôle serveur conçu pour fournir l’accès pour le scénario d’extranet liés à ADFS\ et d’autres scénarios extranet. Pour plus d’informations sur le Proxy d’Application Web, voir [Guide de procédure pas à pas de Proxy d’Application Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement d’ADFS, en fonction des exigences de votre organisation. Ce guide est destiné à un infrastructure spécialiste ou un architecte système. Il met en évidence vos points de décision principal lorsque vous planifiez votre déploiement ADFS. Avant de lire ce guide, vous devez avoir une bonne compréhension du fonctionnement des services ADFS sur un niveau fonctionnel. Pour plus d’informations, voir [Understanding Key ADFS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identifier vos objectifs de déploiement ADFS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planifier votre topologie de déploiement ADFS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Configuration ADFS requise](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Voir aussi  
[Conception ADFS](../../ad-fs/AD-FS-Design.md)  
  

