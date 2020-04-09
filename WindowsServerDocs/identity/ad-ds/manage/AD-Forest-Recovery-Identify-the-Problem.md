---
title: 'Récupération de la forêt Active Directory : identifier le problème'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 73b7ef8dc6093ae28c4b5076e7b332b704090b4f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823992"
---
# <a name="identify-the-problem"></a>Identifier le problème

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2
  
Lorsque des symptômes d’un échec à l’ensemble de la forêt apparaissent, par exemple dans les journaux des événements ou d’autres solutions de surveillance, travaillez avec Support Microsoft pour déterminer la cause de l’échec et évaluez les remèdes possibles.  

## <a name="examples-of-forest-wide-failures"></a>Exemples de défaillances à l’ensemble de la forêt

- Tous les contrôleurs de contrôleur ont été logiquement endommagés ou physiquement endommagés à un point que la continuité d’activité est impossible ; par exemple, toutes les applications métier qui dépendent de AD DS ne sont pas fonctionnelles.  
- Un administrateur malveillant a compromis l’environnement Active Directory.  
- Une personne malveillante intentionnellement, ou un administrateur, exécute accidentellement un script qui répartit les données endommagées dans la forêt.  
- Une personne malveillante intentionnellement, ou un administrateur, étend accidentellement le schéma Active Directory avec des modifications malveillantes ou conflictuelles.  
- Une personne malveillante a réussi à installer des logiciels malveillants sur les contrôleurs de l’utilisateur, et vous avez été invité par Support Microsoft à récupérer la forêt à partir de la sauvegarde.  
  
   > [!IMPORTANT]
   >  Ce document ne traite pas des recommandations de sécurité relatives à la récupération d’une forêt piratée ou compromise. En général, il est recommandé de suivre les techniques d’atténuation Pass-The-hash pour renforcer l’environnement. Pour plus d’informations, consultez [atténuation des attaques Pass-The-hash (PTH) et autres techniques de vol d’informations d’identification](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Aucun des contrôleurs de contrôle d’un contrôleur ne peut répliquer avec ses partenaires de réplication.  
- Les modifications ne peuvent pas être apportées à AD DS sur un contrôleur de domaine.  
- Impossible d’installer de nouveaux contrôleurs de domaine dans un domaine.  
  
## <a name="next-steps"></a>Étapes suivantes

- [Récupération de la forêt Active Directory : conditions préalables](AD-Forest-Recovery-Prerequisties.md)  
- [Récupération de la forêt Active Directory-élaboration d’un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Récupération de la forêt Active Directory-identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
- [Récupération de la forêt Active Directory-déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Récupération de la forêt Active Directory-effectuer la récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)  
- [Récupération de la forêt Active Directory-Forum aux questions](AD-Forest-Recovery-FAQ.md)  
- [Récupération de la forêt Active Directory-récupération d’un domaine unique au sein d’une forêt à plusieurs domaines](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Récupération de forêt AD-récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
