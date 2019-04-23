---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: Exigences en matière de conception des services ADDS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 79c694112f39adf5d37cd28f6bd7a770dedf3976
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857520"
---
# <a name="ad-ds-design-requirements"></a>Exigences en matière de conception des services ADDS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Conception de la structure logique Active Directory  
Avant de déployer Windows Server 2008 Active Directory domaine Services (AD DS), vous devez planifier et concevoir la structure logique AD DS pour votre environnement. La structure logique AD DS détermine la façon dont vos objets d’annuaire sont organisés, et il fournit une méthode efficace pour gérer vos comptes de réseau et les ressources partagées. Lorsque vous concevez votre structure logique AD DS, vous définissez une partie importante de l’infrastructure réseau de votre organisation.  
  
Pour concevoir la structure logique AD DS, déterminer le nombre de forêts exigées par votre organisation et ensuite créer des conceptions de domaines, l’infrastructure du système DNS (Domain Name) et unités d’organisation (UO). L’illustration suivante montre le processus de conception de la structure logique.  
  
![Exigences de conception d’AD DS](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Pour plus d’informations, consultez [concevoir la logique Structure pour Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Conception de la topologie de site  
Une fois que vous concevez la structure logique pour votre infrastructure AD DS, vous devez concevoir la topologie de site pour votre réseau. La topologie de site est une représentation logique de votre réseau physique. Il contient des informations sur l’emplacement des sites AD DS, les contrôleurs de domaine AD DS au sein de chaque site et les liens de site et les ponts entre liens de site qui prennent en charge la réplication entre les sites AD DS. L’illustration suivante montre le processus de conception de topologie de site.  
  
![Exigences de conception d’AD DS](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Pour plus d’informations, consultez [conception du Site topologie pour Windows Server 2008 AD DS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Planification de la capacité du contrôleur de domaine  
Pour garantir des performances d’AD DS efficace, vous devez déterminer le nombre approprié de contrôleurs de domaine pour chaque site et vérifier qu’elles répondent à la configuration matérielle requise pour Windows Server 2008. Faites attention planifier la capacité de vos contrôleurs de domaine garantit que vous ne sous-estimez pas la configuration matérielle requise, ce qui peut entraîner des temps de réponse des performances et des applications de domaine médiocres contrôleur. L’illustration suivante montre le processus de planification de la capacité de contrôleur domaine.  
  
![Exigences de conception d’AD DS](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>L’activation de Windows Server 2008 avancé des fonctionnalités AD DS  
Vous pouvez utiliser AD DS de Windows Server 2008 pour introduire des fonctionnalités avancées dans votre environnement en déclenchant le niveau fonctionnel du domaine ou forêt. Vous pouvez augmenter le niveau fonctionnel vers Windows Server 2008 lorsque tous les contrôleurs de domaine dans le domaine ou forêt exécutent Windows Server 2008.  
  
Pour plus d’informations, consultez [l’activation des fonctionnalités avancées pour AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


