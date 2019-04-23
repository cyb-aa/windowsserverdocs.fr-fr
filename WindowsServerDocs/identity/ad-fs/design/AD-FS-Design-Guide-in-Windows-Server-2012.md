---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guide de conception AD FS dans Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890210"
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guide de conception AD FS dans Windows Server 2012

>S'applique à : Windows Server 2012
  
> [!NOTE]  
> Pour plus d’informations sur le déploiement d’AD FS dans Windows Server 2012 R2, consultez [Guide déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Vous pouvez utiliser Active Directory® Federation Services \(AD FS\) dans le Windows Server® 2012 système d’exploitation dans une fédération de services de rôle de fournisseur pour authentifier en toute transparence vos utilisateurs à n’importe quel site Web\-en fonction des services ou applications qui résident dans une organisation partenaire de ressource, sans la nécessité pour les administrateurs à créer ou modifier des approbations externes ou des approbations de forêt entre les réseaux des deux organisations et sans la nécessité pour les utilisateurs pour se connecter sur une deuxième fois. Le processus d’authentification à un seul réseau lors de l’accès aux ressources dans un autre réseau, sans avoir aux actions répétées d’ouverture de session par les utilisateurs, est appelé de manière unique\-sur \(SSO\).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement d’AD FS, selon les besoins de votre organisation \(reflétant dans ce guide en tant que les objectifs de déploiement\) et de la conception que vous souhaitez créer. Ce guide est prévu pour une utilisation par un spécialiste d'infrastructure ou un architecte système. Il met en évidence vos principaux points de décision lorsque vous planifiez votre déploiement AD FS. Avant de lire ce guide, doit avoir une bonne compréhension du fonctionnement des services AD FS sur un niveau fonctionnel. Vous devez également avoir une bonne compréhension des exigences d’organisation qui sera reflétée dans votre conception AD FS.  
  
Ce guide décrit un ensemble d’objectifs de déploiement sont basées sur trois conceptions AD FS principales, et il permet de déterminer la conception la plus appropriée pour votre environnement. Vous pouvez utiliser ces objectifs de déploiement pour former les conceptions AD FS complètes suivantes ou une conception personnalisée adaptée aux besoins de votre environnement :  
  
-   SSO de Web pour prendre en charge d’entreprise fédérée\-à\-business \(B2B\) scénarios et pour prendre en charge la collaboration entre des divisions avec des forêts indépendantes  
  
-   Web SSO pour prendre en charge d’accès client aux applications dans l’entreprise\-à\-consommateur \(B2C\) scénarios  
  
Pour chaque conception, vous trouverez des indications pour rassembler les données nécessaires à votre environnement, Vous pouvez ensuite utiliser ces instructions pour planifier et concevoir votre déploiement AD FS. Une fois que vous lire ce guide et rassemblé, documenté et ciblé les besoins de votre organisation de fin, vous aurez les informations nécessaires pour démarrer le déploiement d’AD FS en utilisant les instructions dans le [Windows Server 2012 AD FS Deployment Guide](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identification de vos objectifs de déploiement AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mappage de vos objectifs de déploiement pour une conception AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Déterminer votre topologie de déploiement AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planification de votre déploiement](Planning-Your-Deployment.md)  
  
-   [Planification du Placement de serveur de fédération](Planning-Federation-Server-Placement.md)  
  
-   [Planification du Placement des serveurs Proxy de fédération](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planification de la capacité du serveur FS AD](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Annexe a : Examen des exigences en matière de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

