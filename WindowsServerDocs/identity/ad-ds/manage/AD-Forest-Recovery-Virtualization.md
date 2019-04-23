---
title: Virtualisation de récupération de forêt AD
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 23317f55fdce18e78ac3e7e1490f6fc4937fd062
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858450"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualisation de récupération de forêt Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette rubrique décrit la fonctionnalité de clonage de contrôleur de domaine virtualisés dans Windows Server 2016, 2012 R2 et 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>À l’aide de clonage du contrôleur de domaine virtualisés pour accélérer la récupération de forêt

Le clonage de contrôleur de domaine virtualisés simplifie et accélère le processus pour l’installation des autres contrôleurs de domaine virtualisés dans un domaine, en particulier dans des emplacements centralisés tels que les centres de données où plusieurs contrôleurs de domaine s’exécutent sur les hyperviseurs. Une fois que vous restaurez un contrôleur de domaine virtuel dans chaque domaine à partir de la sauvegarde, les contrôleurs de domaine supplémentaires dans chaque domaine peuvent être rapidement mis en ligne à l’aide du contrôleur de domaine virtualisé processus de clonage. Vous pouvez préparer le contrôleur de domaine virtualisé première récupérer, arrêtez-le, puis copiez ce disque dur virtuel comme autant de fois qu’est nécessaire de créer cloné de contrôleurs de domaine pour créer le domaine virtualisés.  
  
La configuration requise pour le clonage de contrôleur de domaine virtualisés est :  
  
- L’hyperviseur doit prendre en charge VM-GenerationID. Hyper-V dans Windows Server 2016, 2012 et Windows 8 est un exemple d’un hyperviseur qui prend en charge VM-GenerationID. Vérifiez auprès de votre fournisseur de l’hyperviseur si VM-GenerationID est pris en charge.  
- Le contrôleur de domaine virtualisé qui est utilisé en tant que source pour le clonage doit exécuter Windows Server 2016 ou 2012 et être membre du groupe de contrôleurs de domaine clonables. 
- L’émulateur de contrôleur de domaine principal doit exécuter Windows Server 2016 ou 2012. Vous pouvez cloner émulateur PDC si elle est virtualisé.  
  
Pour obtenir des instructions pas à pas indiquant comment effectuer virtualisé le clonage de contrôleur de domaine, consultez [Introduction à la virtualisation des Services de domaine Active Directory (AD DS) (niveau 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Pour plus d’informations sur la façon virtualisé fonctionnement du clonage de contrôleur de domaine, consultez [virtualisé domaine contrôleur référence technique (niveau 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
