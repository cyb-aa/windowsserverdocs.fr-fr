---
title: "AD récupération de forêt: identifier le problème"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 1e9ede12dade0b5f1149eae784620520a8866e30
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="identify-the-problem"></a>Identifier le problème

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
  
 Lorsque les symptômes d’un échec de la forêt s’affiche, comme dans les journaux des événements ou d’autres solutions de surveillance, travailler avec le Support Microsoft pour déterminer la cause de l’échec et évaluer les solutions possibles.  
 
## <a name="examples-of-forest-wide-failures"></a>Exemples d’échecs de la forêt 
  
-   Tous les contrôleurs de domaine ont été endommagées logiquement ou physiquement endommagés à un point de que la continuité d’activité est impossible; par exemple, toutes les applications d’entreprise qui dépendent des services ADDS ne fonctionnent pas.  
  
-   Un administrateur non autorisé a compromis l’environnement ActiveDirectory.  
  
-   Une personne malveillante intentionnellement, ou un administrateur accidentellement: exécute un script qui se propage de corruption des données dans la forêt.  
  
-   Une personne malveillante intentionnellement, ou un administrateur accidentellement: étend le schéma ActiveDirectory avec les modifications de logiciels malveillants ou en conflit.  
  
-   Une personne malveillante a réussi à installer des logiciels malveillants sur les contrôleurs de domaine, et vous avez été informés par le Support Microsoft pour récupérer la forêt à partir de la sauvegarde.  
  
    > [!IMPORTANT]
    >  Cet article ne couvre pas les recommandations de sécurité sur la récupération d’une forêt qui a été piratée ou compromise. En règle générale, il est recommandé pour les techniques d’atténuation suivez Pass-the-Hash renforcer l’environnement. Pour plus d’informations, voir [attaques Mitigating Pass-the-Hash (PtH) et autres Techniques de vol d’informations d’identification](https://www.microsoft.com/download/details.aspx?id=36036).  
  
-   Aucun des contrôleurs de domaine ne peut répliquer avec leurs partenaires de réplication.  
  
-   Les services ADDS sur n’importe quel contrôleur de domaine ne peut pas être modifiée.  
  
-   Nouveaux contrôleurs de domaine ne peut pas être installés dans un domaine quelconque.  
  
## <a name="next-steps"></a>Étapes suivantes
-   [Récupération de la forêt ActiveDirectory - Configuration requise](AD-Forest-Recovery-Prerequisties.md)  
-   [Récupération de la forêt ActiveDirectory - concevoir un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt ActiveDirectory - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Récupération de la forêt ActiveDirectory - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Récupération de la forêt ActiveDirectory - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  
-   [Récupération de la forêt ActiveDirectory - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
-   [Récupération de la forêt ActiveDirectory - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Récupération de la forêt ActiveDirectory - récupération de forêt avec des contrôleurs de domaine Windows Server2003](AD-Forest-Recovery-Windows-Server-2003.md) 
