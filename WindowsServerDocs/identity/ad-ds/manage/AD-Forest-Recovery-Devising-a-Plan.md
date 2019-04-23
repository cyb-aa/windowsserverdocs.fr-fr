---
title: Plan de récupération de forêt de récupération de forêt AD - concevoir une publicité
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: edfc874fe030c6394bc8bda95c49e61951e78f43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881780"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Plan de récupération de forêt de récupération de forêt AD - concevoir une publicité

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Selon votre environnement et les besoins de l’entreprise, vous pourrez ou vous n’ayez pas à effectuer toutes les étapes décrites dans ce guide pour effectuer une récupération de forêt réussie. Étant donné que ce guide sert uniquement en tant que modèle pour la récupération de forêt, il est essentiel que vous concevoir un plan de récupération personnalisée-forêt qui correspond le mieux à votre environnement et répond aux besoins de votre entreprise.  
  
Par exemple, dans votre plan de récupération de forêt, vous devez avoir un mappage de topologie détaillée de vos forêts. La carte doit répertorier toutes les informations sur les contrôleurs de domaine, comme leurs noms, leurs rôles et état de la sauvegarde et les relations d’approbation entre eux. Pour un outil que vous pouvez utiliser pour créer une carte topologique, consultez [Microsoft Active Directory Générateur de diagrammes topologiques](https://www.microsoft.com/download/details.aspx?id=13380).  
  
Vous devez vous entraîner à votre plan de récupération de forêt au moins une fois par an. En outre, il est judicieux d’effectuer un exercice de récupération de forêt lorsqu’il existe des modifications d’appartenance au groupe Administrateurs de l’entreprise ou Admins du domaine. Cela permet de garantir que votre personnel informatique (IT) prend complètement en charge le plan de récupération de forêt.

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
