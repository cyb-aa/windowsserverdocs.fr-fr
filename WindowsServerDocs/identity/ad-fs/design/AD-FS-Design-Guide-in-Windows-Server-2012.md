---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guide de conception ADFS dans Windows Server2012
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guide de conception ADFS dans Windows Server2012

>S’applique à: Windows Server2012
  
> [!NOTE]  
> Pour plus d’informations sur comment déployer ADFS dans Windows Server2012R2, consultez [Guide de déploiement Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Vous pouvez utiliser ActiveDirectory® Federation Services \(ADFS\) avec le système d’exploitation Windows Server® 2012dans un rôle de fournisseur de services de fédération en toute transparence authentifier vos utilisateurs à des services basés sur la console Web ou des applications qui se trouvent dans une organisation partenaire de ressource, sans avoir besoin pour les administrateurs à créer ou modifier des approbations externes ou des approbations de forêt entre les réseaux des deux organisations et sans avoir besoin pour les utilisateurs à ouvrir une session sur une seconde fois. Le processus d’authentification à un seul réseau lors de l’accès aux ressources dans un autre réseau, sans la charge d’actions répétées d’ouverture de session par les utilisateurs, est appelé \(SSO\) sur connexion unique.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide fournit des recommandations pour vous aider à planifier un nouveau déploiement d’ADFS, en fonction des exigences de votre organisation \ (également appelée dans ce guide en tant que déploiement goals\) et la conception particulière que vous voulez créer. Ce guide est destiné à un infrastructure spécialiste ou un architecte système. Il met en évidence vos points de décision principal lorsque vous planifiez votre déploiement ADFS. Avant de lire ce guide, vous devez avoir une bonne compréhension du fonctionnement des services ADFS sur un niveau fonctionnel. Vous devez également avoir une bonne compréhension des exigences d’organisation qui sera répercutée dans votre conception ADFS.  
  
Ce guide décrit un ensemble d’objectifs de déploiement basés sur trois principales conceptions d’ADFS, et vous aide à choisir la conception la plus appropriée pour votre environnement. Vous pouvez utiliser ces objectifs de déploiement pour les conceptions ADFS complètes suivantes ou une conception personnalisée adaptée aux besoins de votre environnement:  
  
-   SSO de Web fédéré pour prendre en charge les scénarios \(B2B\) business\-celle-ci-business et pour prendre en charge de la collaboration entre des divisions avec des forêts indépendantes  
  
-   Pour prendre en charge d’accès client aux applications dans les scénarios \(B2C\) business\-celle-ci-consommateur SSO de Web  
  
Pour chaque conception, vous trouverez des indications pour rassembler les données nécessaires à votre environnement. Vous pouvez ensuite utiliser ces recommandations pour planifier et concevoir votre déploiement d’ADFS. Après avoir lu ce guide et rassemblé, documenté et ciblé les besoins de votre organisation, vous disposerez des informations nécessaires pour démarrer le déploiement d’ADFS utilisant les instructions de la [Guide de déploiement de Windows Server2012 ADFS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Identification de vos objectifs de déploiement ADFS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mappage de vos objectifs de déploiement à une conception ADFS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Déterminer votre topologie de déploiement ADFS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planification de votre déploiement](Planning-Your-Deployment.md)  
  
-   [Planification du Placement de serveur de fédération](Planning-Federation-Server-Placement.md)  
  
-   [Planification du positionnement des serveurs Proxy de fédération](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planification de la capacité du serveur FS AD](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Annexe a: examen de configuration ADFS requise](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

