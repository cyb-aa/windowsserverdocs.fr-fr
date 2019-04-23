---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Création d’une conception d’unité d’organisation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9ecd0228b50a4e597c3fa1dbd3fdaf1f84ca1d3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830400"
---
# <a name="creating-an-organizational-unit-design"></a>Création d’une conception d’unité d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les propriétaires de forêt sont chargés de créer des conceptions d’unité d’organisation (UO) pour leurs domaines. Création d’une conception d’unité d’organisation implique la conception de la structure d’unité d’organisation, affectez le rôle de propriétaire d’unité d’organisation, création compte et unités d’organisation de ressource.  
  
Initialement, concevoir votre structure d’unité d’organisation pour activer la délégation d’administration. Lors de la conception d’unité d’organisation est terminée, vous pouvez créer des structures d’unités d’organisation supplémentaires pour l’application de stratégie de groupe pour les utilisateurs et les ordinateurs et pour limiter la visibilité des objets. Pour plus d’informations, voir conception d’une Infrastructure de stratégie de groupe ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Rôle de propriétaire d’unité d’organisation  
Le propriétaire de la forêt désigne un propriétaire de l’unité d’organisation pour chaque unité d’organisation que vous concevez pour le domaine. Propriétaires de l’unité d’organisation sont responsables de données qui contrôlent une sous-arborescence d’objets dans les Services de domaine Active Directory (AD DS). Propriétaires de l’unité d’organisation peuvent contrôler comment l’administration est déléguée et comment la stratégie est appliquée aux objets dans leur UO. Ils peuvent également créer des sous-arborescences nouvelle et déléguer l’administration des unités d’organisation au sein de ces sous-arborescences.  
  
Étant donné que les propriétaires de l’unité d’organisation ne pas posséder ou contrôlent le fonctionnement du service d’annuaire, vous pouvez séparer la propriété et l’administration du service d’annuaire à partir de la propriété et l’administration d’objets, ce qui réduit le nombre d’administrateurs de service qui ont des niveaux élevés d’accès.  
  
Unités d’organisation fournissent l’autonomie d’administration et les moyens de contrôler la visibilité des objets dans l’annuaire. Unités d’organisation permettent d’isoler des autres administrateurs de données, mais ils ne fournissent pas d’isolation des administrateurs de service. Bien que les propriétaires de l’unité d’organisation contrôlent une sous-arborescence d’objets, le propriétaire de la forêt conserve un contrôle total sur tous les sous-arborescences. Ainsi, le propriétaire de la forêt pour corriger les erreurs, par exemple une erreur dans une liste de contrôle d’accès (ACL) et pour libérer des sous-arborescences délégués lorsque les administrateurs de données sont terminées.  
  
## <a name="account-ous-and-resource-ous"></a>Unités d’organisation de compte et de ressources  
Unités d’organisation de compte contiennent des objets utilisateur, groupe et ordinateur. Propriétaires de la forêt doivent créer une structure d’unité d’organisation pour gérer ces objets et puis déléguer le contrôle de la structure au propriétaire de l’unité d’organisation. Si vous déployez un nouveau domaine AD DS, créer un unité d’organisation de compte pour le domaine afin que vous pouvez déléguer le contrôle des comptes dans le domaine.  
  
Unités d’organisation de ressources contiennent des ressources et les comptes qui sont chargés de gérer ces ressources. Le propriétaire de la forêt est également responsable de la création d’une structure d’unité d’organisation pour gérer ces ressources et de délégation du contrôle de cette structure pour le propriétaire de l’unité d’organisation. Créer la ressource unités d’organisation en fonction des besoins selon les besoins de chaque groupe au sein de votre organisation d’autonomie de la gestion des données et des équipements.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Documentation de la conception d’unité d’organisation pour chaque domaine  
Création d’une équipe pour concevoir la structure d’unité d’organisation qui vous permet de déléguer le contrôle sur les ressources au sein de la forêt. Le propriétaire de la forêt peut-être être impliqué dans le processus de conception et doit approuver la conception d’unité d’organisation. Vous pouvez également faire intervenir au moins un administrateur de service pour vous assurer que la conception est valide. Autres participants de l’équipe conception peuvent inclure les administrateurs de données qui travaillent sur les unités d’organisation et l’unité d’organisation propriétaires qui seront responsables de leur gestion.  
  
Il est important de documenter votre conception d’unité d’organisation. Répertorier les noms des unités d’organisation que vous souhaitez créer. Et, pour chaque unité d’organisation, le type d’unité d’organisation, le propriétaire de l’unité d’organisation, l’unité d’organisation (le cas échéant) du parent et l’origine de cette unité d’organisation du document.  
  
Pour une feuille de calcul pour vous aider à documenter votre conception d’unité d’organisation, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de la tâche aides pour Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) et ouvrez » Identification des unités d’organisation pour chaque domaine » (DSSLOGI_9.doc).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Examen des Concepts de conception d’unité d’organisation](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Délégation de l’Administration à l’aide des objets de l’unité d’organisation](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


