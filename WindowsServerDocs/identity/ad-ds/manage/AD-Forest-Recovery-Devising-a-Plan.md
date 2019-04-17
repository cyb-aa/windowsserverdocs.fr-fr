---
title: "Récupération de la forêt ActiveDirectory - concevoir une publicité Plan de récupération de forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adfs
ms.openlocfilehash: 6754509ccac352895b9370e63adf1b69548953bf
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
>
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Récupération de la forêt ActiveDirectory - concevoir une publicité Plan de récupération de forêt

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Selon votre environnement et les besoins de l’entreprise, vous posez ou ayez pas à effectuer toutes les étapes décrites dans ce guide pour effectuer une récupération de forêt réussie. Étant donné que ce guide sert uniquement d’un modèle de récupération de forêt, il est essentiel que vous établir un plan de récupération de forêt personnalisé qui adaptée à votre environnement et répond aux besoins de votre entreprise.  
  
 Par exemple, dans votre plan de récupération de forêt, vous devez avoir une carte topologique détaillées de vos forêts. La carte doit répertorier toutes les informations sur les contrôleurs de domaine, tels que leur nom, leurs rôles et état de la sauvegarde et les relations d’approbation entre eux. Pour un outil que vous pouvez utiliser pour créer un mappage de topologie, voir [MicrosoftActiveDirectory topologie créateur de schéma de](https://www.microsoft.com/download/details.aspx?id=13380).  
  
 Vous devez mettre en pratique votre plan de récupération de forêt au moins une fois par an. En outre, il est conseillé d’effectuer une exploration de récupération de forêt lorsqu’il existe des modifications d’appartenance au groupe Administrateurs de l’entreprise ou Admins du domaine. Cela permet de garantir que votre personnel informatique (IT) comprend parfaitement le plan de récupération de forêt.

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
