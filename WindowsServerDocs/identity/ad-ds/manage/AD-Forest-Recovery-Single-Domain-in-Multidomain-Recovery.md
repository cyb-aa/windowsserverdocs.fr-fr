---
title: Récupération de la forêt ActiveDirectory - récupération d’un domaine unique dans une forêt à plusieurs domaines
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adfs
ms.openlocfilehash: 3a1c9d0671a732eee83aa707e061afdbbf106fa5
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Récupération de la forêt ActiveDirectory - récupération d’un domaine unique dans une forêt à plusieurs domaines

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Il peut arriver lorsqu’il est nécessaire récupérer un seul domaine au sein d’une forêt dotée de plusieurs domaines, plutôt que d’une récupération de forêt complète. Cette rubrique décrit les considérations sur la récupération d’un seul domaine et les stratégies possibles pour la récupération.  
  
 Récupération d’un domaine unique présente un défi unique pour la reconstruction des serveurs de catalogue global (GC). Par exemple, si le premier contrôleur de domaine (DC) pour le domaine est restauré à partir d’une sauvegarde qui a été créée une semaine plus tôt, tous les autres catalogues globaux de la forêt auront plus de données à jour pour ce domaine que le contrôleur de domaine restauré. Pour rétablir la cohérence des données dans le catalogue global, il existe deux options:  
  
-   Unhost et hébergez puis la partition de domaines récupérée à partir de tous les catalogues globaux de la forêt, à l’exception de ceux du domaine récupéré, en même temps.  
  
-   Suivez le processus de récupération de forêt pour récupérer le domaine, puis supprimer les objets en attente de catalogues globaux dans d’autres domaines.  
  
 Les sections suivantes fournissent des considérations générales pour chaque option. L’ensemble complet des étapes qui doivent être effectuées pour la récupération peut varier pour différents environnements ActiveDirectory.  
  
## <a name="rehost-all-gcs"></a>Hébergez tous les catalogues globaux  

> [!WARNING]
>  Le mot de passe du compte d’administrateur de domaine pour tous les domaines doit être prêt à être utilisé dans le cas où un problème empêche l’accès à un catalogue global pour l’ouverture de session.  

 Réhébergement tous les catalogues globaux peut être effectué à l’aide de repadmin /rehost commandes repadmin//unhost et (dans le cadre de repadmin /experthelp). Vous devez exécuter les commandes de repadmin sur chaque serveur de catalogue global dans chaque domaine qui n’est pas récupéré. Il doit être assurée, tous les catalogues globaux ne possédant pas une copie du domaine récupérée plus. Pour ce faire, unhost la partition de domaine tout d’abord à partir de tous les contrôleurs de domaine dans tous les domaines none récupérée de la forêt tout d’abord. Une fois que tous les catalogues globaux ne contiennent pas de la partition de plus, vous pouvez le hébergez. Lors du réhébergement, considérez la structure site et la réplication de votre forêt, par exemple, terminer le réhébergement d’un contrôleur de domaine par site avant de réhébergement les autres contrôleurs de domaine de ce site.  
  
 Cette option peut être avantageuse pour une petite organisation disposant uniquement quelques contrôleurs de domaine pour chaque domaine. Tous les catalogues globaux peuvent être reconstruite un soir vendredi et, si nécessaire, les partitions de terminer la réplication de domaine en lecture seule avant le lundi matin. Toutefois, si vous avez besoin récupérer un domaine de grande taille qui couvre les sites dans le monde entier, réhébergement la partition de domaine en lecture seule sur tous les catalogues globaux pour d’autres domaines peuvent considérablement les opérations impact et peuvent nécessiter le temps d’arrêt.  
  
## <a name="remove-lingering-objects"></a>Supprimer des objets en attente  
 Similaire au processus de récupération de forêt, vous restaurez un contrôleur de domaine à partir de la sauvegarde dans le domaine dont vous avez besoin à récupérer, effectuer un nettoyage des métadonnées des contrôleurs de domaine restants, puis réinstaller ADDS pour créer le domaine. Sur les catalogues globaux de tous les autres domaines dans la forêt, vous supprimez les objets en attente pour la partition en lecture seule du domaine restauré.  
  
 La source pour le nettoyage d’objet en attente doit être un contrôleur de domaine dans le domaine restauré. Pour être certain que le contrôleur de domaine source ne dispose pas des objets en attente pour toutes les partitions de domaine, vous pouvez supprimer le catalogue global, s’il s’agissait d’un catalogue global.  
  
 Suppression d’objets en attente est avantageux pour les grandes organisations qui ne risque pas le temps d’arrêt avec les autres options.  
  
 Pour plus d’informations, voir [Repadmin utilisation pour supprimer les objets en attente](https://technet.microsoft.com/library/cc785298.aspx).

## <a name="next-steps"></a>Étapes suivantes
-   [Récupération de la forêt ActiveDirectory - Configuration requise](AD-Forest-Recovery-Prerequisties.md)  
-   [Récupération de la forêt ActiveDirectory - concevoir un plan de récupération de forêt personnalisé](AD-Forest-Recovery-Devising-a-Plan.md)  
-   [Récupération de la forêt ActiveDirectory - identifier le problème](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Récupération de la forêt ActiveDirectory - déterminer comment récupérer](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Récupération de la forêt ActiveDirectory - effectuer une récupération initiale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)  
-   [Récupération de la forêt ActiveDirectory - Forum aux Questions](AD-Forest-Recovery-FAQ.md)  
-   [Récupération de la forêt ActiveDirectory - récupération d’un domaine unique au sein d’une forêt Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Récupération de la forêt ActiveDirectory - récupération de forêt avec des contrôleurs de domaine Windows Server2003](AD-Forest-Recovery-Windows-Server-2003.md)  
