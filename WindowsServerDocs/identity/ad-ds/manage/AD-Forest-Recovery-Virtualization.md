---
title: Virtualisation de la récupération de la forêt Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c055445c2d3aecd8c6d92e94799f556962c977bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390133"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualisation de la récupération de la forêt Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette rubrique décrit la fonctionnalité de clonage des contrôleurs de domaine virtualisés dans Windows Server 2016, 2012 R2 et 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Utilisation du clonage des contrôleurs de domaine virtualisés pour accélérer la récupération de la forêt

Le clonage des contrôleurs de domaine virtualisés simplifie et accélère le processus d’installation de contrôleurs de domaine virtualisés supplémentaires dans un domaine, en particulier dans des emplacements centralisés, tels que des centres de serveurs où plusieurs contrôleurs de domaine s’exécutent sur des hyperviseurs. Après la restauration d’un contrôleur de domaine virtuel dans chaque domaine à partir d’une sauvegarde, les contrôleurs de domaine supplémentaires dans chaque domaine peuvent être rapidement mis en ligne à l’aide du processus de clonage de contrôleur de domaine virtualisé. Vous pouvez préparer le premier contrôleur de domaine virtualisé que vous récupérez, l’arrêter, puis copier ce disque dur virtuel autant de fois que nécessaire afin de créer des contrôleurs de domaine virtualisés clonés pour créer le domaine.  
  
La configuration requise pour le clonage de contrôleur de courant virtuel est la suivante :  
  
- L’hyperviseur doit prendre en charge VM-GenerationID. Hyper-V dans Windows Server 2016, 2012 et Windows 8 est un exemple d’hyperviseur qui prend en charge VM-GenerationID. Vérifiez auprès de votre fournisseur d’hyperviseurs si VM-GenerationID est pris en charge.  
- Le contrôleur de domaine virtualisé utilisé comme source pour le clonage doit exécuter Windows Server 2016 ou 2012 et être membre du groupe contrôleurs de domaine clonables. 
- L’émulateur de contrôleur de domaine principal doit exécuter Windows Server 2016 ou 2012. Vous pouvez cloner l’émulateur de contrôleur de domaine principal s’il est virtualisé.  
  
Pour obtenir des instructions pas à pas sur la façon d’effectuer un clonage de contrôleur de périphérique virtualisé, consultez [Présentation de la virtualisation de Active Directory Domain Services (AD DS) (niveau 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Pour plus d’informations sur le fonctionnement du clonage de contrôleur de domaine virtualisé, consultez informations techniques de référence sur les [contrôleurs de domaine virtualisés (niveau 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
