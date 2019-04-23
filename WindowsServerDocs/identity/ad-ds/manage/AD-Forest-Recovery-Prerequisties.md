---
title: Conditions préalables pour la planification de la récupération de forêt Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c8945dd5ccccb27826dd96413b56a070a7452789
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842170"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Conditions préalables de récupération de forêt Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Le document suivant décrivent les conditions préalables que vous devez connaître avant de concevoir un plan de récupération de forêt ou de tenter une récupération.

## <a name="assumptions-for-using-this-guide"></a>Hypothèses pour l’utilisation de ce Guide

1. Vous avez travaillé avec un professionnel du Support Microsoft et :
   - Déterminer la cause de l’échec de la forêt. Ce guide ne pas suggérer une cause de l’échec ou recommandons les procédures pour empêcher l’échec.
   - Évaluer les solutions possibles.  
   - Conclu en concertation avec le Support Microsoft, que la restauration toute la forêt à son état avant la défaillance s’est produite est la meilleure façon de récupérer à partir de l’échec. Dans de nombreux cas, récupération de forêt doit être la dernière option.

2. Que vous avez suivi les recommandations de meilleures pratiques de Microsoft pour l’utilisation intégrée à Active Directory système DNS (Domain Name). Plus précisément, il doit y avoir une zone DNS intégrées à Active Directory pour chaque domaine Active Directory. 
   - Si ce n’est pas le cas, vous pouvez toujours utiliser les principes de base de ce guide pour effectuer une récupération de forêt. Toutefois, vous devez prendre des mesures spécifiques pour la récupération DNS en fonction de votre propre environnement. Pour plus d’informations sur l’utilisation de DNS intégré à Active Directory, consultez [création d’une conception d’Infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

3. Bien que ce guide est conçu comme guide pour la récupération de forêt, générique pas couvertes par tous les scénarios possibles. Par exemple, à compter de Windows Server 2008, il y est une version de Server Core, qui est une version complète de Windows Server, mais sans interface GUI complète. Bien qu’il soit certainement possible de récupérer une forêt consistant à simplement les contrôleurs de domaine exécutant Server Core, ce guide contient des instructions détaillées. Toutefois, selon les instructions présentées ici vous serez en mesure de concevoir les actions de ligne de commande requises vous-même.  

> ![!NOTE]
> Bien que les objectifs de ce guide sont à récupérer de la forêt et de mettre à jour ou de restaurer toutes les fonctionnalités DNS, récupération peut entraîner une configuration DNS qui est remplacée par la configuration avant la défaillance. Une fois la forêt est récupérée, vous pouvez revenir à la configuration DNS d’origine. Les recommandations de ce guide ne décrivent pas comment configurer des serveurs DNS pour effectuer la résolution de nom des autres parties de l’espace de noms d’entreprise où il existe des zones DNS qui ne sont pas stockées dans AD DS.  

## <a name="concepts-for-using-this-guide"></a>Concepts pour l’utilisation de ce Guide

Avant de commencer la planification de la récupération d’une forêt Active Directory, vous devez être familiarisé avec les éléments suivants :  
  
- Concepts fondamentaux de Active Directory  
- L’importance des rôles de maître d’opérations (également appelés opérations à maître uniques flottant ou FSMO). Ces rôles sont les suivantes :  
   - Contrôleur de schéma
   - Maître d’attribution de noms de domaine
   - ID maître RID (relative)
   - Maître d’émulateur de contrôleur de domaine principal
   - Maître d’infrastructure

En outre, vous devez avoir sauvegardé et restauré des services AD DS et SYSVOL dans un environnement de laboratoire sur une base régulière. Pour plus d’informations, consultez [la sauvegarde des données d’état du système](AD-Forest-Recovery-Procedures.md) et [effectuer une restauration ne faisant pas autoritée des Services de domaine Active Directory](AD-Forest-Recovery-Procedures.md).

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
