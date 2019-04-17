---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS et les services ADDS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e7ee494c157396a7d58e9fd1b4b80060c4d99fc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="dns-and-ad-ds"></a>DNS et les services ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Services de domaine ActiveDirectory (ADDS) utilise les services de résolution de noms système DNS (Domain Name) pour rendre possible pour les clients rechercher les contrôleurs de domaine et pour les contrôleurs de domaine qui hébergent le service d’annuaire pour communiquer entre eux.  
  
Les services ADDS permet une intégration facile de l’espace de noms ActiveDirectory dans un espace de noms DNS. Fonctionnalités telles que les zones DNS intégrées à ActiveDirectory faciliter leur déploiement DNS en éliminant la nécessité de configurer des zones secondaires, puis configurez les transferts de zone.  
  
Pour plus d’informations sur comment DNS prend en charge les services ADDS, voir la prise en charge de DNS pour la référence technique ActiveDirectory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
> [!NOTE]  
> Si vous implémentez dans lequel le nom de domaine ADDS diffère le suffixe DNS principal que les clients utilisent un espace de noms disjoint, l’intégration des services ADDS avec le DNS est plus complexe. Pour plus d’informations, voir [Namespace disjoints](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Emplacement de contrôleur de domaine](../../ad-ds/plan/Domain-Controller-Location.md)  
  
-   [Active les Zones DNS intégrées à ActiveDirectory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
  
-   [Nom d’ordinateur](../../ad-ds/plan/Computer-Naming.md)  
  
-   [Namespace disjoint](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
  


