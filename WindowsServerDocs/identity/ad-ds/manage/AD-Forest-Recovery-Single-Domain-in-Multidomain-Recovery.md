---
title: Récupération de forêt AD - récupération d’un domaine unique dans une forêt multidomaine
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: fae2cc40af0b43dd38d72c2622720a6bb17b0a66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863470"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Récupération de forêt AD - récupération d’un domaine unique dans une forêt multidomaine

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Il peut arriver lorsqu’il est nécessaire récupérer un seul domaine dans une forêt dotée de plusieurs domaines, plutôt que d’une récupération de forêt complète. Cette rubrique traite des considérations pour restaurer un seul domaine et les stratégies possibles pour la récupération.  
  
Récupération d’un domaine unique constitue un défi unique pour la reconstruction des serveurs de catalogue global (GC). Par exemple, si le premier contrôleur de domaine (DC) pour le domaine est restauré à partir d’une sauvegarde qui a été créée une semaine plus tôt, puis tous les autres catalogues globaux de la forêt aura plus de données à jour pour ce domaine que le DC restauré. Pour rétablir la cohérence des données de catalogue global, il existe deux options :  
  
- Unhost et puis ré-héberger la partition de domaines récupérée à partir de tous les catalogues globaux dans la forêt, à l’exception de celles figurant dans le domaine récupéré, en même temps.  
- Suivez le processus de récupération de forêt pour récupérer le domaine et ensuite supprimer les objets en attente de catalogues globaux dans d’autres domaines.  
  
Les sections suivantes fournissent des considérations générales pour chaque option. L’ensemble complet des étapes qui doivent être effectuées pour la récupération peut varier pour différents environnements Active Directory.  
  
## <a name="rehost-all-gcs"></a>Ré-héberger tous les catalogues globaux  

> [!WARNING]
> Le mot de passe du compte d’administrateur de domaine pour tous les domaines doit être prêt à être utilisé dans le cas où un problème empêche l’accès à un catalogue global pour la connexion.  

Réhébergement tous les catalogues globaux peut être effectué à l’aide de repadmin / unhost et les commandes de /rehost repadmin (partie de repadmin /experthelp). Vous exécuteriez les commandes repadmin sur chaque catalogue global dans chaque domaine qui n’est pas récupérée. Il doit être assurée, qui tous les catalogues globaux ne contiennent plus une copie du domaine récupérée. Pour ce faire, unhost la partition de domaine tout d’abord tous les contrôleurs de domaine dans tous les domaines none récupérées de la forêt tout d’abord. Une fois que tous les catalogues globaux ne contiennent pas de la partition de plus, vous pouvez le réhébergement. Lors du réhébergement, considérez la structure site et la réplication de votre forêt, par exemple, terminer le réhébergement d’un contrôleur de domaine par site avant de réhébergement les autres contrôleurs de domaine de ce site.  
  
Cette option peut être avantageuse pour une petite entreprise qui a uniquement quelques contrôleurs de domaine pour chaque domaine. Tous les catalogues globaux pourraient être reconstruits sur un vendredi soir et, si nécessaire, les partitions de terminer la réplication de domaine en lecture seule avant le lundi matin. Mais si vous devez récupérer un grand domaine qui couvre les sites dans le monde entier, réhébergement de la partition de domaine en lecture seule sur tous les catalogues globaux pour les autres domaines peuvent considérablement les opérations d’impact et potentiellement nécessite l’arrêt.  
  
## <a name="remove-lingering-objects"></a>Supprimer des objets en attente

Comme pour le processus de récupération de forêt, vous restaurez un contrôleur de domaine à partir de la sauvegarde dans le domaine dont vous avez besoin pour récupérer, effectuer le nettoyage des métadonnées des contrôleurs de domaine restants et puis réinstaller AD DS pour créer le domaine. Sur les catalogues globaux de tous les autres domaines dans la forêt, vous supprimez les objets en attente pour la partition en lecture seule du domaine récupérée.  

La source pour le nettoyage d’objets en attente doit être un contrôleur de domaine dans le domaine récupéré. Pour être certain que le contrôleur de domaine source ne dispose pas de tous les objets en attente pour les partitions de domaine, vous pouvez supprimer le catalogue global s’il s’agissait d’un catalogue global.  

Suppression d’objets en attente est avantageux pour les grandes organisations qui ne risque pas le temps d’arrêt avec les autres options.  

Pour plus d’informations, consultez [Repadmin d’utilisation pour supprimer des objets en attente](https://technet.microsoft.com/library/cc785298.aspx).

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
