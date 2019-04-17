---
title: "Virtualisation de récupération de forêt ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 08b0c4a4c5ed230f91241fcfb67db1749935c5e2
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualisation de récupération de forêt ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Cette rubrique décrit la fonctionnalité de clonage de contrôleur de domaine virtualisés dans Windows Server2016 et 2012R2, 2012.  
 
## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>À l’aide de clonage de contrôleur de domaine virtualisé afin d’accélérer la récupération de forêt  
 Le clonage de contrôleur de domaine virtualisé simplifie et accélère le processus d’installation d’autres contrôleurs de domaine virtualisés dans un domaine, en particulier dans des emplacements centralisés tels que des centres de données où plusieurs contrôleurs de domaine exécutent sur les hyperviseurs. Une fois que vous restaurez un contrôleur de domaine virtuel dans chaque domaine à partir de la sauvegarde, les contrôleurs de domaine supplémentaires dans chaque domaine peuvent être rapidement mis en ligne à l’aide de processus de clonage du contrôleur de domaine virtualisé. Vous pouvez préparer le contrôleur de domaine virtualisé premier que vous récupérez, arrêtez, puis copiez ce disque dur virtuel comme autant de fois que nécessaire pour créer cloné virtualisés contrôleurs de domaine pour créer le domaine.  
  
 La configuration requise pour le clonage de contrôleur de domaine virtualisés est:  
  
-   L’hyperviseur doit prendre en charge VM-GenerationID. Hyper-V dans Windows Server2016, 2012 et Windows8 est un exemple d’un hyperviseur qui prend en charge VM-GenerationID. Contactez votre fournisseur d’hyperviseur si VM-GenerationID est pris en charge.  
  
-   Le contrôleur de domaine virtualisé qui est utilisé en tant que source pour le clonage doit exécuter Windows Server2016 ou 2012 et être un membre du groupe de contrôleurs de domaine clonables.  
  
-   L’émulateur de contrôleur de domaine principal doit exécuter Windows Server2016 ou 2012. Vous pouvez cloner émulateur PDC si elle est virtualisé.  
  
 Pour obtenir des instructions détaillées sur la réalisation virtualisé le clonage de contrôleur de domaine, voir [Introduction aux Services de domaine ActiveDirectory (ADDS) Virtualization (Level 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Pour plus d’informations sur la façon de virtualisé fonctionnement du clonage du contrôleur de domaine, voir [virtualisés domaine contrôleur techniques de référence (niveau 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md).  

-   [Récupération de la forêt ActiveDirectory - Configuration requise](AD-Forest-Recovery-Prerequisties.md)  
-   [Récupération de la forêt ActiveDirectory - concevoir un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt ActiveDirectory - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Récupération de la forêt ActiveDirectory - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Récupération de la forêt ActiveDirectory - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  
-   [Récupération de la forêt ActiveDirectory - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
-   [Récupération de la forêt ActiveDirectory - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Récupération de la forêt ActiveDirectory - récupération de forêt avec des contrôleurs de domaine Windows Server2003](AD-Forest-Recovery-Windows-Server-2003.md) 

  
