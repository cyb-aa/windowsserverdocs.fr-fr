---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guide de conception AD FS dans Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a9a3e5c8e32b74e059f7a7bbc64c85092a5f047
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853952"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guide de conception de AD FS dans Windows Server 


  
> [!NOTE]  
> Pour plus d’informations sur le déploiement de AD FS dans Windows Server 2012 R2, consultez le [Guide de déploiement de Windows server 2012 r2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Vous pouvez utiliser Active Directory Services de Fédération de&reg; \(AD FS\) avec le système d’exploitation Windows Server&reg; 2012 dans un rôle de fournisseur de services de Fédération pour authentifier en toute transparence vos utilisateurs sur des applications ou services Web\-qui résident dans une organisation partenaire de ressource, sans que les administrateurs aient besoin de créer ou de gérer des approbations externes ou des approbations de forêt entre les réseaux des deux organisations et sans que les utilisateurs aient besoin de se connecter une deuxième fois. Le processus d’authentification sur un réseau lors de l’accès aux ressources d’un autre réseau, sans la charge des actions d’ouverture de session répétées par les utilisateurs, est appelé authentification unique\-sur \(\)SSO.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement de AD FS, en fonction des exigences de votre organisation \(également mentionnées dans ce guide comme objectifs de déploiement\) et la conception particulière que vous souhaitez créer. Ce guide est prévu pour une utilisation par un spécialiste d'infrastructure ou un architecte système. Il met en évidence vos principaux points de décision lors de la planification de votre déploiement AD FS. Avant de lire ce guide, vous devez avoir une bonne compréhension de la façon dont AD FS fonctionne sur un niveau fonctionnel. Vous devez également avoir une bonne compréhension des exigences de l’organisation qui seront reflétées dans votre conception de AD FS.  
  
Ce guide décrit un ensemble d’objectifs de déploiement basés sur trois AD FS principales conceptions et vous aide à choisir la conception la plus appropriée pour votre environnement. Vous pouvez utiliser ces objectifs de déploiement pour former l’une des conceptions de AD FS complètes suivantes ou une conception personnalisée qui répond aux besoins de votre environnement :  
  
-   SSO Web fédéré pour la prise en charge des\-d’entreprise pour\-scénarios Business \(B2B\) et pour la prise en charge de la collaboration entre les divisions avec des forêts indépendantes  
  
-   L’authentification unique Web pour la prise en charge de l’accès des clients aux applications dans Business\-pour\-les scénarios \(B2C\)  
  
Pour chaque conception, vous trouverez des indications pour rassembler les données nécessaires à votre environnement, Vous pouvez ensuite utiliser ces instructions pour planifier et concevoir votre déploiement AD FS. Après avoir lu ce guide et terminé la collecte, la documentation et le mappage des exigences de votre organisation, vous disposerez des informations nécessaires pour commencer à déployer AD FS à l’aide des instructions du [Guide de déploiement de Windows Server 2012 AD FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identification de vos objectifs de déploiement d’AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mappage de vos objectifs de déploiement sur une conception AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Déterminer votre topologie de déploiement d’AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planification de votre déploiement](Planning-Your-Deployment.md)  
  
-   [Planification de la sélection élective du serveur de fédération](Planning-Federation-Server-Placement.md)  
  
-   [Planification de la sélection élective du serveur proxy de fédération](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planification de la capacité des serveurs AD FS](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Annexe A : examen des exigences de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

