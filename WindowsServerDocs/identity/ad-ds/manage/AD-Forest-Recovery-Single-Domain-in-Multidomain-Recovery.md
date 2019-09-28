---
title: Récupération de forêt Active Directory-récupération d’un domaine unique dans une forêt à plusieurs domaines
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: 2582ffacb169b59692c0510469131b58be09d5f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368934"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Récupération de forêt Active Directory-récupération d’un domaine unique dans une forêt à plusieurs domaines

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Il peut arriver qu’il soit nécessaire de récupérer un seul domaine au sein d’une forêt comportant plusieurs domaines, plutôt qu’une récupération complète de la forêt. Cette rubrique traite des considérations relatives à la récupération d’un domaine unique et aux stratégies possibles pour la récupération.  
  
Une récupération de domaine unique présente un défi unique pour la reconstruction des serveurs de catalogue global (GC). Par exemple, si le premier contrôleur de domaine du domaine est restauré à partir d’une sauvegarde créée une semaine plus tôt, alors tous les autres catalogues globaux de la forêt auront des données plus récentes pour ce domaine que le contrôleur de domaine restauré. Pour rétablir la cohérence des données de GC, il existe deux options :  
  
- Déshébergez, puis réhébergez la partition de domaines récupérés à partir de tous les catalogues globaux de la forêt, à l’exception de ceux du domaine récupéré, en même temps.  
- Suivez le processus de récupération de forêt pour récupérer le domaine, puis supprimez les objets en attente des catalogues globaux dans d’autres domaines.  
  
Les sections suivantes fournissent des considérations générales pour chaque option. L’ensemble complet des étapes à effectuer pour la récupération varie en fonction des environnements de Active Directory.  
  
## <a name="rehost-all-gcs"></a>Réhéberger tous les catalogues globaux  

> [!WARNING]
> Le mot de passe du compte d’administrateur de domaine pour tous les domaines doit être prêt à être utilisé en cas de problème empêchant l’accès à un GC pour l’ouverture de session.  

Pour réhéberger tous les catalogues globaux, vous pouvez utiliser les commandes repadmin/Unhost et repadmin/Rehost (qui font partie de repadmin/experthelp). Vous exécutez les commandes repadmin sur chaque GC dans chaque domaine qui n’est pas récupéré. Elle doit être assurée, car tous les catalogues globaux ne contiennent plus de copie du domaine récupéré. Pour ce faire, vous devez d’abord déshéberger la partition de domaine de tous les contrôleurs de domaine de tous les domaines récupérés dans la forêt. Une fois que tous les catalogues globaux ne contiennent plus la partition, vous pouvez la réhéberger. Lors du réhébergement, tenez compte de la structure de site et de réplication de votre forêt, par exemple, finissez le réhébergement d’un contrôleur de site par site avant de réhéberger les autres contrôleurs de ce site.  
  
Cette option peut être avantageuse pour une petite organisation qui n’a que quelques contrôleurs de domaine pour chaque domaine. Tous les catalogues globaux peuvent être reconstruits le vendredi soir et, si nécessaire, terminer la réplication pour toutes les partitions de domaine en lecture seule avant le lundi matin. Toutefois, si vous devez récupérer un grand domaine qui couvre les sites dans le monde entier, le réhébergement de la partition de domaine en lecture seule sur tous les catalogues globaux pour les autres domaines peut avoir un impact significatif sur les opérations et éventuellement nécessiter un temps d’attente.  
  
## <a name="remove-lingering-objects"></a>Supprimer les objets en attente

À l’instar du processus de récupération de forêt, vous restaurez un contrôleur de domaine à partir d’une sauvegarde dans le domaine que vous devez récupérer, effectuez le nettoyage des métadonnées des contrôleurs de domaine restants, puis réinstallez AD DS pour créer le domaine. Sur les catalogues globaux de tous les autres domaines de la forêt, vous supprimez les objets en attente pour la partition en lecture seule du domaine récupéré.  

La source du nettoyage d’objets en attente doit être un contrôleur de domaine dans le domaine récupéré. Pour être certain que le contrôleur de domaine source n’a pas d’objets en attente pour les partitions de domaine, vous pouvez supprimer le catalogue global s’il s’agit d’un catalogue global.  

La suppression des objets en attente est avantageuse pour les organisations de plus grande taille qui ne peuvent pas prendre en risque le temps d’inactivité associé aux autres options.  

Pour plus d’informations, consultez [utiliser repadmin pour supprimer des objets en attente](https://technet.microsoft.com/library/cc785298.aspx).

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
