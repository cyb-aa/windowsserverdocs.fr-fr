---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: "Création d’une conception d’unité d’organisation"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3a74770a4558c79d1f9250f37181562e1d235d34
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-an-organizational-unit-design"></a>Création d’une conception d’unité d’organisation

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les propriétaires de forêt sont responsables de la création de conceptions d’unité d’organisation (UO) pour leurs domaines. Création d’une conception d’unité d’organisation implique la conception de la structure d’unité d’organisation, attribuer le rôle de propriétaire d’unité d’organisation, création compte et unités d’organisation de ressource.  
  
Initialement, concevez votre structure d’unité d’organisation pour activer la délégation de l’administration. Lors de la conception d’unité d’organisation est terminée, vous pouvez créer des structures d’unité d’organisation supplémentaires pour l’application de stratégie de groupe pour les ordinateurs et les utilisateurs et de limiter la visibilité des objets. Pour plus d’informations, voir conception d’une Infrastructure de stratégie de groupe ([https://go.microsoft.com/fwlink/?LinkId=106655](https://go.microsoft.com/fwlink/?LinkId=106655)).  
  
## <a name="ou-owner-role"></a>Rôle de propriétaire d’unité d’organisation  
Le propriétaire de la forêt désigne un propriétaire de l’unité d’organisation pour chaque unité d’organisation que vous avez créé pour le domaine. Les propriétaires d’unité d’organisation sont responsables de données contrôlent une sous-arborescence d’objets dans les Services de domaine ActiveDirectory (ADDS). Propriétaires de l’unité d’organisation peuvent contrôler comment l’administration est déléguée et comment la stratégie est appliquée aux objets au sein de leur unité d’organisation. Ils peuvent également créer nouveautés sous-arborescences et déléguer l’administration des unités d’organisation au sein de ces sous-arborescences.  
  
Étant donné que les propriétaires de l’unité d’organisation ne possédez ni contrôler le fonctionnement du service d’annuaire, vous pouvez séparer administration du service d’annuaire et propriété à partir de la propriété et l’administration des objets, ce qui réduit le nombre d’administrateurs de service qui ont des niveaux élevés de l’accès.  
  
Unités d’organisation fournissent autonomie administrative et moyens de contrôler la visibilité des objets dans le répertoire. Unités d’organisation fournissent une isolation à partir d’autres administrateurs de données, mais elles ne fournissent pas d’isolation des administrateurs de service. Bien que les propriétaires de l’unité d’organisation contrôlez une sous-arborescence d’objets, le propriétaire de la forêt conserve un contrôle total sur tous les sous-arbres. Ainsi, le propriétaire de la forêt pour corriger les erreurs, par exemple, une erreur dans la liste de contrôle d’accès (ACL) et pour récupérer les sous-arbres délégués lorsque les administrateurs de données sont terminées.  
  
## <a name="account-ous-and-resource-ous"></a>Unités d’organisation de compte et de ressources  
Unités d’organisation de compte contiennent des objets utilisateur, groupe et d’ordinateur. Les propriétaires de forêt doivent créer une structure d’unité d’organisation pour gérer ces objets et puis déléguer le contrôle de la structure au propriétaire de l’unité d’organisation. Si vous déployez un nouveau domaine ADDS, créez un unité d’organisation de compte pour le domaine afin que vous pouvez déléguer le contrôle des comptes dans le domaine.  
  
Unités d’organisation ressources contiennent des ressources et les comptes qui sont chargés de gérer ces ressources. Le propriétaire de la forêt est également responsable de la création d’une structure d’unité d’organisation pour gérer ces ressources et pour déléguer le contrôle de cette structure au propriétaire de l’unité d’organisation. Créer les unités d’organisation de ressource en fonction des besoins sur les exigences de chaque groupe au sein de votre organisation d’autonomie de la gestion des données et de matériel.  
  
## <a name="documenting-the-ou-design-for-each-domain"></a>Documentation de la conception d’unité d’organisation pour chaque domaine  
Création d’une équipe pour concevoir la structure d’unité d’organisation que vous utilisez pour déléguer le contrôle des ressources au sein de la forêt. Le propriétaire de la forêt peut-être être impliqué dans le processus de conception et doit approuver la conception d’unité d’organisation. Vous pouvez également faire intervenir au moins un administrateur de service pour vous assurer que la conception est valide. Autres participants de l’équipe de conception peuvent inclure les administrateurs de données qui fonctionnent sur les unités d’organisation et de l’unité d’organisation propriétaires qui seront chargés de la gestion.  
  
Il est important de documenter votre conception d’unité d’organisation. Répertorier les noms des unités d’organisation que vous envisagez de créer. Et, pour chaque unité d’organisation, le type d’unité d’organisation, le propriétaire de l’unité d’organisation, le parent unité d’organisation (le cas échéant) et l’origine de cette unité d’organisation.  
  
Pour une feuille de calcul pour vous aider à documenter votre conception d’unité d’organisation, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Examen des Concepts de conception d’unité d’organisation](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)  
  
-   [Délégation de l’Administration à l’aide d’objets de l’unité d’organisation](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)  
  


