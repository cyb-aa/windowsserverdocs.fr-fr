---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: "Exigences de conception de service d’annuaire ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 17338cd00fecec098865095dd9613f62beb3a457
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-ds-design-requirements"></a>Exigences de conception de service d’annuaire ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Conception de la structure logique ActiveDirectory  
Avant de déployer Windows Server2008 ActiveDirectory Services de domaine (ADDS), vous devez planifier et concevoir la structure logique de domaine ActiveDirectory pour votre environnement. La structure logique de domaine ActiveDirectory détermine comment vos objets ActiveDirectory sont organisés, et fournit une méthode efficace pour gérer vos comptes de réseau et les ressources partagées. Lorsque vous concevez la structure logique d’ADDS, vous définissez une partie importante de l’infrastructure réseau de votre organisation.  
  
Pour concevoir la structure logique de domaine ActiveDirectory, déterminer le nombre de forêts requis par votre organisation et puis créer des conceptions pour les domaines, l’infrastructure du système DNS (Domain Name) et des unités d’organisation (UO). L’illustration suivante montre le processus de conception de la structure logique.  
  
![Exigences de conception d’ADDS](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Pour plus d’informations, voir [conception de la logique Structure pour Windows Server2008 ADDS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Conception de la topologie de site  
Une fois que vous concevez la structure logique de votre infrastructure ADDS, vous devez concevoir la topologie de site pour votre réseau. La topologie de site est une représentation logique de votre réseau physique. Il contient des informations sur l’emplacement des sites ADDS, les contrôleurs de domaine ADDS au sein de chaque site et les liens de sites et les ponts entre liens de site qui prennent en charge la réplication entre sites ADDS. L’illustration suivante montre le processus de conception de topologie de site.  
  
![Exigences de conception d’ADDS](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Pour plus d’informations, voir [conception du Site topologie pour Windows Server2008 ADDS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Planification de la capacité des contrôleurs de domaine  
Pour garantir des performances ADDS, vous devez déterminer le nombre approprié de contrôleurs de domaine pour chaque site et vérifiez qu’elles répondent à la configuration matérielle requise pour Windows Server2008. Attention planification de capacité pour vos contrôleurs de domaine permet de s’assurer que vous ne sous-estimez pas la configuration matérielle requise, qui peut entraîner des temps de réponse des applications et les performances de domaine médiocre contrôleur. L’illustration suivante montre le processus de planification de la capacité contrôleur de domaine.  
  
![Exigences de conception d’ADDS](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>L’activation de Windows Server2008 avancée des fonctionnalités ADDS  
Vous pouvez utiliser ADDS Windows Server2008 pour introduire des fonctionnalités avancées dans votre environnement en augmentant le niveau fonctionnel du domaine ou forêt. Vous pouvez augmenter le niveau fonctionnel de Windows Server2008 lorsque tous les contrôleurs de domaine dans le domaine ou forêt exécutent Windows Server2008.  
  
Pour plus d’informations, voir [l’activation des fonctionnalités avancées pour ADDS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


