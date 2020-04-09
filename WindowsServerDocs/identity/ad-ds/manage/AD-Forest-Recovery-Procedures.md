---
title: 'Récupération de la forêt Active Directory : procédures'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: 37422ce09d02615a6048142695820200388661a6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823763"
---
# <a name="ad-forest-recovery---procedures"></a>Récupération de la forêt Active Directory : procédures

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Cette section contient des procédures relatives au processus de récupération de forêt. Les procédures s’appliquent à Windows Server 2016, 2012 R2, 2012 et s’appliquent également à Windows Server 2008 R2 et 2008, avec quelques exceptions mineures.

Les procédures qui incluent des étapes qui varient pour Windows Server 2003 se trouvent dans la [récupération de forêt avec les contrôleurs de domaine Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

La liste suivante répertorie les procédures utilisées pour la sauvegarde et la restauration des contrôleurs de domaine et des Active Directory.

- [Sauvegarde d’un serveur complet](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [Sauvegarde des données d’État du système](AD-Forest-Recovery-Backing-up-System-State.md)  
- [Exécution d’une récupération complète du serveur](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [Exécution d’une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Exécution d’une restauration ne faisant pas autorité de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

Ces étapes expliquent comment effectuer en même temps une restauration faisant autorité de SYSVOL.  

- [Configuration du service serveur DNS](AD-Forest-Recovery-Configure-DNS.md)  
- [Suppression du catalogue global](AD-Forest-Recovery-Remove-GC.md)  
- [Augmentation de la valeur des pools RID disponibles](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [Invalidation du pool RID actuel](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [Prise d’un rôle de maître d’opérations](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [Nettoyage après une restauration](AD-Forest-Recovery-Cleanup.md)
- [Nettoyage des métadonnées des contrôleurs de domaine accessibles en écriture supprimés](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [Réinitialisation du mot de passe du compte d’ordinateur du contrôleur de domaine](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [Réinitialisation du mot de passe krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [Réinitialisation d’un mot de passe d’approbation d’un côté de l’approbation](AD-Forest-Recovery-Reset-Trust.md)  
- [Ajout du catalogue global](AD-Forest-Recovery-Add-GC.md)  
- [Ressources pour vérifier le bon fonctionnement de la réplication](AD-Forest-Recovery-Verify-Replication.md)  

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
