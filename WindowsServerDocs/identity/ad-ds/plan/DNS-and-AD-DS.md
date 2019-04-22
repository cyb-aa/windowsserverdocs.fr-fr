---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS et services AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d75a78119d76a0f8380967292b1d0abc720597
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813140"
---
# <a name="dns-and-ad-ds"></a>DNS et services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Services de domaine Active Directory (AD DS) utilise les services de résolution de noms système DNS (Domain Name) pour le rendre possible pour les clients rechercher les contrôleurs de domaine et pour les contrôleurs de domaine qui hébergent le service d’annuaire pour communiquer entre eux.  
  
Les services AD DS s’intègre facilement de l’espace de noms Active Directory dans un espace de noms DNS. Fonctionnalités telles que les zones DNS intégrées à Active Directory facilitent l’en éliminant la nécessité de configurer des zones secondaires DNS de déploiement, puis configurez les transferts de zone.  
  
Pour plus d’informations sur la manière dont DNS prend en charge les services AD DS, consultez la section [prise en charge de DNS pour les informations de référence technique Active Directory](https://go.microsoft.com/fwlink/?LinkID=48147).  
  
> [!NOTE]  
> Si vous implémentez un espace de noms disjoint dans lequel le nom de domaine AD DS diffère le suffixe DNS principal utilisé par les clients, l’intégration des services AD DS avec DNS est plus complexe. Pour plus d’informations, consultez [Namespace disjointes](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
- [Emplacement de contrôleur de domaine](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Les Zones DNS Active Directory-intégré](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [Nom d’ordinateur](../../ad-ds/plan/Computer-Naming.md)  
- [Disjoint Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
