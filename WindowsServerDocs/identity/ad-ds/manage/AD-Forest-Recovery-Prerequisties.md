---
title: Conditions préalables à la planification de la récupération d’Active Directory forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 12c34afc497131bfe6abdc78c636e6d3b784877e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390368"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Prérequis pour la récupération de la forêt Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Le document suivant traite des conditions préalables que vous devez connaître avant de concevoir un plan de récupération de forêt ou de tenter une récupération.

## <a name="assumptions-for-using-this-guide"></a>Hypothèses pour l’utilisation de ce guide

1. Vous avez travaillé avec un Support Microsoft professionnel et :
   - Déterminez la cause de l’échec à l’ensemble de la forêt. Ce guide ne suggère pas de cause de défaillance ou recommande des procédures pour empêcher la défaillance.
   - Évalué les remèdes possibles.  
   - Conclu, en consultation avec Support Microsoft, le fait de restaurer l’ensemble de la forêt à son état avant la défaillance est la meilleure façon de récupérer après l’échec. Dans de nombreux cas, la récupération de la forêt doit être la dernière option.

2. Vous avez suivi les recommandations de Microsoft en matière de meilleures pratiques pour l’utilisation de DNS (Domain Name System) intégré à Active Directory. Plus précisément, il doit y avoir une zone DNS intégrée à la Active Directory pour chaque domaine Active Directory. 
   - Si ce n’est pas le cas, vous pouvez toujours utiliser les principes de base de ce guide pour effectuer une récupération de forêt. Toutefois, vous devrez prendre des mesures spécifiques pour la récupération DNS en fonction de votre propre environnement. Pour plus d’informations sur l’utilisation de DNS intégré à Active Directory, consultez [création d’une conception d’infrastructure DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

3. Bien que ce guide soit conçu comme un guide générique pour la récupération de forêt, tous les scénarios possibles ne sont pas couverts. Par exemple, à partir de Windows Server 2008, il existe une version Server Core, qui est une version complète de Windows Server, mais sans interface utilisateur graphique complète. Bien qu’il soit possible de récupérer une forêt composée uniquement de contrôleurs de service qui exécutent Server Core, ce guide ne contient aucune instruction détaillée. Toutefois, en fonction des instructions présentées ici, vous serez en mesure de concevoir vous-même les actions de ligne de commande requises.  

> ![!NOTE]
> Bien que les objectifs de ce guide concernent la récupération de la forêt et la maintenance ou la restauration de la fonctionnalité DNS complète, la récupération peut entraîner une configuration DNS qui est modifiée par rapport à la configuration avant la défaillance. Une fois la forêt Récupérée, vous pouvez revenir à la configuration DNS d’origine. Les recommandations de ce guide ne décrivent pas comment configurer des serveurs DNS pour effectuer la résolution de noms d’autres parties de l’espace de noms d’entreprise où des zones DNS ne sont pas stockées dans AD DS.  

## <a name="concepts-for-using-this-guide"></a>Concepts relatifs à l’utilisation de ce guide

Avant de commencer la planification de la récupération d’une forêt Active Directory, vous devez être familiarisé avec les éléments suivants :  
  
- Concepts fondamentaux de Active Directory  
- L’importance des rôles de maître d’opérations (également appelés opérations à maître unique flottant ou FSMO). Ces rôles sont les suivants :  
   - Contrôleur de schéma
   - Maître d’attribution de noms de domaine
   - Maître RID (relative ID)
   - Maître d’émulateur de contrôleur de domaine principal (PDC)
   - Maître d’infrastructure

En outre, vous devez avoir sauvegardé et restauré AD DS et SYSVOL dans un environnement de laboratoire régulièrement. Pour plus d’informations, consultez [sauvegarde des données d’État du système](AD-Forest-Recovery-Procedures.md) et [exécution d’une restauration ne faisant pas autorité de Active Directory Domain Services](AD-Forest-Recovery-Procedures.md).

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
