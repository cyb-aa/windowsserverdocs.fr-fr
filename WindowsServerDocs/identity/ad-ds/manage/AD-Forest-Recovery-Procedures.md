---
title: 'Récupération de la forêt Active Directory : procédures'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: da45e3b20c370a2a37b0eab31a78216434dd60be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827720"
---
# <a name="ad-forest-recovery---procedures"></a>Récupération de la forêt Active Directory : procédures

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette section contient des procédures liées au processus de récupération de forêt. Les procédures sont applicables pour Windows Server 2016, 2012 R2, 2012 et sont également applicables à Windows Server 2008 R2 et 2008 avec quelques exceptions mineures.

Les procédures qui incluent les étapes varient pour Windows Server 2003 se trouvent dans [récupération de forêt avec des contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

Voici une liste des procédures qui sont utilisés dans la sauvegarde et restauration des contrôleurs de domaine et Active Directory.

- [Sauvegarde d’un serveur complet](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [Sauvegarde des données d’état du système](AD-Forest-Recovery-Backing-up-System-State.md)  
- [Exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [Effectuer une synchronisation faisant autoritée de SYSVOL de réplication DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Effectuez une restauration ne faisant pas autoritée des Services de domaine Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

Ces étapes expliquent comment effectuer une restauration faisant autorité de SYSVOL en même temps.  

- [Configuration du service serveur DNS](AD-Forest-Recovery-Configure-DNS.md)  
- [Suppression du catalogue global](AD-Forest-Recovery-Remove-GC.md)  
- [Déclenchement de la valeur de pools RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [Invalidant le pool RID actuel](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [Prendre un rôle de maître d’opérations](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [Nettoyage après une restauration](AD-Forest-Recovery-Cleanup.md)
- [Nettoyage des métadonnées des contrôleurs de domaine accessible en écriture supprimés](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [Réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [Réinitialiser le mot de passe krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [Réinitialiser un mot de passe d’approbation d’un côté de l’approbation](AD-Forest-Recovery-Reset-Trust.md)  
- [Ajout du catalogue global](AD-Forest-Recovery-Add-GC.md)  
- [Ressources pour vérifier l’utilisation de la réplication](AD-Forest-Recovery-Verify-Replication.md)  

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
