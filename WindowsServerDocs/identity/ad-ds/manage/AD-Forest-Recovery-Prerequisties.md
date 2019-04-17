---
title: "Conditions préalables pour la planification de la récupération de forêt ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 672e1f4d0de9bfb2cbe291c5ed715814c8acacd0
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
#<a name="active-directory-forest-recovery-prerequisites"></a>Conditions préalables de récupération de forêt ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Le document suivant décrivent les conditions préalables que vous devez être familiarisé avec avant de concevoir un plan de récupération de forêt ou de tenter une récupération.

## <a name="assumptions-for-using-this-guide"></a>Hypothèses pour l’utilisation de ce Guide 

1.  Vous avez travaillé avec un professionnel du Support Microsoft et:

    - Déterminer la cause de l’échec de la forêt. Ce guide ne pas proposer une cause de l’échec ou vous recommandons de toutes les procédures afin d’éviter l’échec.  
    - Évaluer les solutions possibles.  
    - Conclu, en consultation avec le Support Microsoft, que la restauration de l’ensemble de la forêt à son état avant la défaillance s’est produite est la meilleure façon de récupérer après l’échec. Dans de nombreux cas, récupération de la forêt doit être le dernier recours.  </br></br>

2. Que vous avez suivi les recommandations de meilleures pratiques Microsoft pour l’utilisation d’intégrées à ActiveDirectory système DNS (Domain Name). Plus précisément, il doit exister une zone DNS intégrées à ActiveDirectory pour chaque domaine ActiveDirectory. Si ce n’est pas le cas, vous pouvez toujours utiliser les principes de base de ce guide pour effectuer une récupération de forêt. Toutefois, vous devez prendre des mesures pour la récupération DNS en fonction de votre propre environnement spécifiques. Pour plus d’informations sur l’utilisation de DNS intégré à ActiveDirectory, voir [création d’une conception d’Infrastructure DNS ](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).
3. Bien que ce guide est conçu comme un guide générique pour la récupération de forêt, pas tous les scénarios possibles sont traités. Par exemple, à compter de Windows Server2008, il est une version minimale, qui est une version complète de Windows Server, mais sans interface GUI complète. Même s’il est certainement possible de récupérer une forêt consistant à simplement les contrôleurs de domaine qui exécutent Server Core, ce guide contient des instructions détaillées. Toutefois, selon les recommandations abordées ici vous serez en mesure de concevoir les actions de ligne de commande requises par vous-même.  
 
>![!NOTE]
> Bien que les objectifs de ce guide pour récupérer la forêt et mettre à jour ou restaurer toutes les fonctionnalités DNS, récupération peut entraîner une configuration DNS qui est passée de la configuration avant la défaillance. Après la récupération de la forêt, vous pouvez revenir à la configuration DNS d’origine. Les recommandations de ce guide ne décrivent pas comment configurer les serveurs DNS pour effectuer la résolution de noms d’autres parties de l’espace de noms d’entreprise où il existe des zones DNS qui ne sont pas stockées dans ADDS.  

## <a name="concepts-for-using-this-guide"></a>Concepts relatifs à l’aide de ce Guide
 Avant de commencer la planification de la récupération d’une forêt ActiveDirectory, vous devez être familiarisé avec les éléments suivants:  
  
-   Concepts fondamentaux d’ActiveDirectory  
  
-   L’importance de rôles de maître d’opérations (également appelé opérations à maître uniques flottant ou FSMO). Ces rôles sont les suivants:  
  
    -   Contrôleur de schéma  
  
    -   Maître d’attribution de noms de domaine  
  
    -   ID du maître RID (relative)  
  
    -   Maître d’émulateur de contrôleur de domaine principal  
  
    -   Maître d’infrastructure  
  
 En outre, vous avez sauvegardé et restauré le domaine ActiveDirectory et SYSVOL dans un environnement de laboratoire de façon régulière. Pour plus d’informations, voir [sauvegarde les données d’état système](AD-Forest-Recovery-Procedures.md) et [effectuer une restauration ne faisant pas autoritée des Services de domaine ActiveDirectory ](AD-Forest-Recovery-Procedures.md).

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
