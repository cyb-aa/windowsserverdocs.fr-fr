---
title: "Récupération de la forêt ActiveDirectory - procédures"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adfs
ms.openlocfilehash: 91e3954c05fe3cd12d35b5db91afd29fb3a31e00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/13/2017
---
# <a name="ad-forest-recovery---procedures"></a>Récupération de la forêt ActiveDirectory - procédures


>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Cette section contient des procédures concernant le processus de récupération de forêt. Les procédures sont applicables pour Windows Server2016, 2012R2, 2012 et sont également applicables à Windows Server2008R2 et 2008 avec quelques exceptions mineures. 

Les procédures qui incluent les étapes varient pour Windows Server2003 se trouvent dans [récupération de forêt avec des contrôleurs de domaine Windows Server2003](AD-Forest-Recovery-Windows-Server-2003.md).  

Voici une liste des procédures qui sont utilisés dans la sauvegarde et restauration des contrôleurs de domaine et ActiveDirectory.
  
-   [Sauvegarde de l’intégralité d’un serveur](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
-   [Sauvegardez les données d’état du système](AD-Forest-Recovery-Backing-up-System-State.md)  
-   [Exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
-   [Effectuer une synchronisation faisant autoritée du répertoire SYSVOL répliqué DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
-   [Effectuer une restauration ne faisant pas autoritée des Services de domaine ActiveDirectory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  
  
     Les étapes suivantes expliquent comment effectuer une restauration faisant autorité de SYSVOL en même temps.  
-   [La configuration du service serveur DNS](AD-Forest-Recovery-Configure-DNS.md)  
-   [Suppression du catalogue global](AD-Forest-Recovery-Remove-GC.md)  
-   [Augmenter la valeur de pools RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md)  
-   [Invalidant le pool RID actuel](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
-   [Prendre un rôle de maître d’opérations](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
-   [Nettoyage après une restauration](AD-Forest-Recovery-Cleanup.md)
-   [Nettoyage des métadonnées des contrôleurs de domaine accessible en écriture supprimés](AD-Forest-Recovery-Cleaning-Metadata.md)  
-   [Réinitialiser le mot de passe du compte ordinateur du contrôleur de domaine](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
-   [Réinitialiser le mot de passe krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
-   [Réinitialiser un mot de passe d’approbation d’un côté de l’approbation](AD-Forest-Recovery-Reset-Trust.md)  
-   [Ajout du catalogue global](AD-Forest-Recovery-Add-GC.md)  
-   [Ressources pour vérifier la réplication fonctionne](AD-Forest-Recovery-Verify-Replication.md)  
  
  
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
