---
title: Récupération de la forêt Active Directory-élaboration d’un plan de récupération de forêt Active Directory
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: ebdff0616d0e3a99b710e07e3bff149a275ff4ea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824023"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Récupération de la forêt Active Directory-élaboration d’un plan de récupération de forêt Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Selon les besoins de votre environnement et de votre entreprise, vous pouvez être amené à effectuer toutes les étapes décrites dans ce guide pour effectuer une récupération de forêt réussie. Étant donné que ce guide sert uniquement de modèle pour la récupération de forêt, il est essentiel de concevoir un plan de récupération de forêt personnalisé adapté à votre environnement et répondant aux besoins de votre entreprise.  
  
Par exemple, dans votre plan de récupération de forêt, vous devez disposer d’une carte de topologie détaillée de vos forêts. La carte doit répertorier toutes les informations sur les contrôleurs de schéma, telles que leur nom, leur rôle et l’état de la sauvegarde, ainsi que les relations d’approbation entre eux. Pour obtenir un outil que vous pouvez utiliser pour créer une carte topologique, consultez [diagramme de topologie Microsoft Active Directory](https://www.microsoft.com/download/details.aspx?id=13380).  
  
Vous devez tester votre plan de récupération de forêt au moins une fois par an. En outre, il est judicieux d’effectuer un exercice de récupération de forêt lorsqu’il y a des modifications d’appartenance au groupe administrateurs de l’entreprise ou Admins du domaine. Cela permet de s’assurer que votre personnel informatique comprend entièrement le plan de récupération de forêt.

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
