---
title: 'Récupération de la forêt Active Directory : identifier le problème'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: cb3cf45f778ff305c8fe5aad5e23f0257650d6f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865520"
---
# <a name="identify-the-problem"></a>Identifier le problème

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2
  
Symptômes d’un échec de la forêt qui s’affichent, comme dans les journaux des événements ou d’autres solutions de surveillance, fonctionnent avec le Support Microsoft pour déterminer la cause de l’échec et évaluer les solutions possibles.  

## <a name="examples-of-forest-wide-failures"></a>Exemples d’échecs de forêt

- Tous les contrôleurs de domaine ont été endommagées logiquement ou physiquement endommagés à un point que la continuité d’activité est impossible ; par exemple, toutes les applications d’entreprise qui dépendent des services AD DS ne fonctionnent pas.  
- Un administrateur malveillant a compromis l’environnement Active Directory.  
- Une personne malveillante intentionnellement, ou un administrateur par inadvertance, exécute un script qui répartit les données endommagées sur la forêt.  
- Une personne malveillante intentionnellement, ou un administrateur par inadvertance, étend le schéma Active Directory avec les modifications malveillantes ou en conflit.  
- Une personne malveillante a réussi à installer des logiciels malveillants sur les contrôleurs de domaine, et vous avez été informés par le Support Microsoft pour récupérer de la forêt à partir de la sauvegarde.  
  
   > [!IMPORTANT]
   >  Ce document ne couvre pas les recommandations de sécurité sur la récupération d’une forêt qui a été piratée ou compromise. En règle générale, il est recommandé pour les techniques de prévention de suivi Pass-the-Hash pour renforcer l’environnement. Pour plus d’informations, consultez [les attaques Mitigating Pass-the-Hash (PtH) et autres Techniques de vol d’informations d’identification](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Aucun des contrôleurs de domaine peut répliquer avec leurs partenaires de réplication.  
- Ne peut pas être apportées à AD DS à n’importe quel contrôleur de domaine.  
- Nouveaux contrôleurs de domaine ne peut pas être installés dans n’importe quel domaine.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Récupération de forêt AD - conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de forêt AD - concevoir un plan de récupération personnalisée-forêt](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de forêt AD - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de forêt AD - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de forêt AD - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de forêt AD - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de forêt AD - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD - récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
