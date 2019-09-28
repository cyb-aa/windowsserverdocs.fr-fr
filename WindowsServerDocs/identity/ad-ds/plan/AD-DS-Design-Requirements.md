---
ms.assetid: f6e76ef0-2217-4cdb-980f-22a780a85ebb
title: Exigences en matière de conception des services ADDS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cb9d4c04bc3fc7bb534e75b80f0947bd225b7b85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368956"
---
# <a name="ad-ds-design-requirements"></a>Exigences en matière de conception des services ADDS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="designing-the-active-directory-logical-structure"></a>Conception de la structure logique Active Directory  
Avant de déployer Windows Server 2008 Active Directory Domain Services (AD DS), vous devez planifier et concevoir la structure logique AD DS pour votre environnement. La structure logique AD DS détermine la façon dont vos objets d’annuaire sont organisés et fournit une méthode efficace pour gérer vos comptes réseau et les ressources partagées. Lorsque vous concevez votre structure logique AD DS, vous définissez une partie importante de l’infrastructure réseau de votre organisation.  
  
Pour concevoir la structure logique AD DS, déterminez le nombre de forêts nécessaires à votre organisation, puis créez des conceptions pour les domaines, l’infrastructure DNS (Domain Name System) et les unités d’organisation (UO). L’illustration suivante montre le processus de conception de la structure logique.  
  
![AD DS exigences de conception](media/AD-DS-Design-Requirements/d5cebae6-a752-4063-a98f-473799c251bd.gif)  
  
Pour plus d’informations, consultez [conception de la structure logique pour Windows Server 2008 AD DS](Designing-the-Logical-Structure.md).  
  
## <a name="designing-the-site-topology"></a>Conception de la topologie de site  
Une fois que vous avez conçu la structure logique pour votre infrastructure AD DS, vous devez concevoir la topologie de site pour votre réseau. La topologie de site est une représentation logique de votre réseau physique. Il contient des informations sur l’emplacement des sites AD DS, les contrôleurs de domaine AD DS au sein de chaque site, ainsi que les liens de sites et les ponts entre liens de sites qui prennent en charge la réplication AD DS entre les sites. L’illustration suivante montre le processus de conception de la topologie de site.  
  
![AD DS exigences de conception](media/AD-DS-Design-Requirements/d34d43c0-437f-47cb-9b64-09c0f9ce6479.gif)  
  
Pour plus d’informations, consultez [conception de la topologie de site pour Windows Server 2008 AD DS](Designing-the-Site-Topology.md).  
  
## <a name="planning-domain-controller-capacity"></a>Planification de la capacité du contrôleur de domaine  
Pour garantir des performances de AD DS efficaces, vous devez déterminer le nombre approprié de contrôleurs de domaine pour chaque site et vérifier qu’ils répondent à la configuration matérielle requise pour Windows Server 2008. Une planification minutieuse de la capacité pour vos contrôleurs de domaine garantit que vous ne sous-estimez pas la configuration matérielle requise, ce qui peut entraîner des performances médiocres du contrôleur de domaine et du temps de réponse des applications L’illustration suivante montre le processus de planification de la capacité du contrôleur de domaine.  
  
![AD DS exigences de conception](media/AD-DS-Design-Requirements/fff6ef22-5c7b-4478-ad76-42b296dcf769.gif)  
  
## <a name="enabling-windows-server-2008-advanced-ad-ds-features"></a>Activation des fonctionnalités de AD DS avancées de Windows Server 2008  
Vous pouvez utiliser Windows Server 2008 AD DS pour introduire des fonctionnalités avancées dans votre environnement en augmentant le niveau fonctionnel du domaine ou de la forêt. Vous pouvez augmenter le niveau fonctionnel à Windows Server 2008 lorsque tous les contrôleurs de domaine du domaine ou de la forêt exécutent Windows Server 2008.  
  
Pour plus d’informations, consultez [activation des fonctionnalités avancées pour AD DS](../../ad-ds/plan/Enabling-Advanced-Features-for-AD-DS.md).  
  


