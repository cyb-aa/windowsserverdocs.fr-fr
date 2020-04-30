---
ms.assetid: b8df1828-5ead-4c90-b0fe-95c675116b7c
title: Création d’une conception d’unité d’organisation
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8e92ad4b280572fc3b44a0161af9a4ea25653c89
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624317"
---
# <a name="creating-an-organizational-unit-design"></a>Création d’une conception d’unité d’organisation

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les propriétaires de forêts sont chargés de créer des conceptions d’unités d’organisation pour leurs domaines. La création d’une unité d’organisation consiste à concevoir la structure d’UO, à attribuer le rôle de propriétaire de l’unité d’organisation et à créer des unités d’organisation de comptes et de ressources.

Au départ, concevez votre structure d’UO pour permettre la délégation de l’administration. Une fois la conception de l’unité d’organisation terminée, vous pouvez créer des structures d’UO supplémentaires pour l’application des stratégie de groupe aux utilisateurs et aux ordinateurs et pour limiter la visibilité des objets. Pour plus d’informations, consultez [conception d’une infrastructure de stratégie de groupe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786524(v=ws.10)).

## <a name="ou-owner-role"></a>Rôle propriétaire de l’unité d’organisation
Le propriétaire de la forêt désigne un propriétaire d’UO pour chaque unité d’organisation que vous concevez pour le domaine. Les propriétaires d’UO sont des gestionnaires de données qui contrôlent une sous-arborescence d’objets dans Active Directory Domain Services (AD DS). Les propriétaires d’unités d’organisation peuvent contrôler la façon dont l’administration est déléguée et comment la stratégie est appliquée aux objets au sein de leur unité d’organisation. Ils peuvent également créer des sous-arborescences et déléguer l’administration des unités d’organisation au sein de ces sous-arborescences.

Étant donné que les propriétaires d’UO ne possèdent pas ou ne contrôlent pas le fonctionnement du service d’annuaire, vous pouvez séparer la propriété et l’administration du service d’annuaire de la propriété et de l’administration des objets, ce qui réduit le nombre d’administrateurs de service disposant de hauts niveaux d’accès.

Les unités d’organisation offrent une autonomie administrative et les moyens de contrôler la visibilité des objets dans l’annuaire. Les unités d’organisation permettent d’isoler les autres administrateurs de données, mais elles ne permettent pas d’isoler des administrateurs de service. Bien que les propriétaires d’UO disposent d’un contrôle sur une sous-arborescence d’objets, le propriétaire de la forêt conserve le contrôle total sur toutes les sous-arborescences. Cela permet au propriétaire de la forêt de corriger des erreurs, telles qu’une erreur dans une liste de contrôle d’accès (ACL), et de récupérer des sous-arborescences déléguées lorsque les administrateurs de données sont arrêtés.

## <a name="account-ous-and-resource-ous"></a>UO de comptes et unités d’organisation de ressource
Les unités d’organisation de compte contiennent des objets utilisateur, groupe et ordinateur. Les propriétaires de forêts doivent créer une structure d’unité d’organisation pour gérer ces objets, puis déléguer le contrôle de la structure au propriétaire de l’unité d’organisation. Si vous déployez un nouveau domaine de AD DS, créez une unité d’organisation de compte pour le domaine afin de pouvoir déléguer le contrôle des comptes dans le domaine.

Les unités d’organisation de ressource contiennent des ressources et les comptes qui sont responsables de la gestion de ces ressources. Le propriétaire de la forêt est également responsable de la création d’une structure d’unité d’organisation pour gérer ces ressources et pour la délégation du contrôle de cette structure au propriétaire de l’unité d’organisation. Créez les unités d’organisation de ressources en fonction des besoins de chaque groupe au sein de votre organisation pour l’autonomie dans la gestion des données et de l’équipement.

## <a name="documenting-the-ou-design-for-each-domain"></a>Documentation de la conception d’UO pour chaque domaine
Assemblez une équipe pour concevoir la structure d’UO que vous utilisez pour déléguer le contrôle des ressources au sein de la forêt. Le propriétaire de la forêt peut être impliqué dans le processus de conception et doit approuver la conception de l’unité d’organisation. Vous pouvez également impliquer au moins un administrateur de service pour vous assurer que la conception est valide. D’autres participants de l’équipe de conception peuvent inclure les administrateurs de données qui travailleront sur les unités d’organisation et les propriétaires d’UO qui seront chargés de les gérer.

Il est important de documenter votre conception d’UO. Répertoriez les noms des unités d’organisation que vous prévoyez de créer. Et, pour chaque unité d’organisation, documentez le type d’unité d’organisation, le propriétaire de l’unité d’organisation, l’unité d’organisation parente (le cas échéant) et l’origine de cette unité d’organisation.

Pour obtenir une feuille de calcul qui vous aide à documenter votre conception d’UO, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des [Outils d’aide pour le kit de déploiement Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) et ouvrez « identification des unités d’organisation pour chaque domaine » (DSSLOGI_9. doc).

## <a name="in-this-section"></a>Contenu de cette section

- [Examen des concepts d’unité d’organisation](../../ad-ds/plan/Reviewing-OU-Design-Concepts.md)

- [Délégation de l’administration avec des objets d’unité d’organisation](../../ad-ds/plan/Delegating-Administration-by-Using-OU-Objects.md)
