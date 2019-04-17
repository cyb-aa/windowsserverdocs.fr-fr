---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: "Modèles de conception de forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b24eba7173a4878102ac7b7a84f7322425df8a52
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="forest-design-models"></a>Modèles de conception de forêt

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez appliquer un des modèles de conception de trois forêt suivants dans votre environnement Active Directory:  
  
-   Modèle de forêt d’organisation  
  
-   Modèle de forêt de ressources  
  
-   Modèle à accès restreint  
  
Il est probable que vous devez utiliser une combinaison de ces modèles pour répondre aux besoins de différents groupes de votre organisation.  
  
## <a name="organizational-forest-model"></a>Modèle de forêt d’organisation  
Dans le modèle de forêt d’organisation, les comptes d’utilisateur et les ressources sont contenus dans la forêt et gérés indépendamment. La forêt d’organisation peut être utilisée pour fournir l’autonomie du service, l’isolation du service ou l’isolation des données, si la forêt est configurée pour empêcher l’accès à tous les utilisateurs en dehors de la forêt.  
  
Si les utilisateurs dans une forêt d’organisation doivent accéder aux ressources des autres forêts (ou l’inverse), les relations d’approbation peuvent être établies entre une forêt d’organisation et les autres forêts. Cela permet aux administrateurs d’accorder l’accès aux ressources de l’autre forêt. L’illustration suivante montre le modèle de forêt d’organisation.  
  
![modèles de conception de forêt](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Chaque conception d’Active Directory comprend au moins une forêt d’organisation.  
  
## <a name="resource-forest-model"></a>Modèle de forêt de ressources  
Dans le modèle de forêt de ressources, une forêt distincte est utilisée pour gérer les ressources. Les forêts de ressource ne contiennent pas de comptes d’utilisateur autres que celles qui sont requises pour l’administration des services et celles qui sont requises pour fournir l’accès de substitution pour les ressources de cette forêt, si les comptes d’utilisateur dans la forêt d’organisation ne sont plus disponibles. Approbations de forêt sont établies afin que les utilisateurs d’autres forêts peuvent accéder aux ressources contenues dans la forêt de ressources. L’illustration suivante montre le modèle de forêt de ressources.  
  
![modèles de conception de forêt](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Les forêts de ressource fournissent l’isolation du service qui est utilisée pour protéger les zones du réseau que vous avez besoin pour maintenir un état de la haute disponibilité. Par exemple, si votre société comprend une fonctionnalité de fabrication qui doit continuer à fonctionner lorsqu’il existe des problèmes sur le reste du réseau, vous pouvez créer une forêt de ressources distinct pour le groupe de production.  
  
## <a name="restricted-access-forest-model"></a>Modèle à accès restreint  
Dans le modèle à accès restreint, une forêt distincte est créée pour contenir les comptes d’utilisateur et les données qui doivent être isolées du reste de l’organisation. Isolation des données dans les situations où les conséquences de compromettre les données de projet sont graves fournissent des forêts accès restreint. L’illustration suivante montre un modèle à accès restreint.  
  
![modèles de conception de forêt](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Les utilisateurs d’autres forêts ne peut pas accéder à des données confidentielles, car aucune relation d’approbation n’existe. Dans ce modèle, les utilisateurs ont un compte dans une forêt d’organisation pour accéder aux ressources de l’organisation générales et un autre compte d’utilisateur dans la forêt à accès restreint pour l’accès à des données confidentielles. Ces utilisateurs doivent disposer de deux postes distincts, une connectée à la forêt d’organisation et l’autre connectée à la forêt à accès restreint. Protège contre la possibilité qu’un administrateur de service à partir d’une seule forêt peut accéder à une station de travail dans la forêt à accès restreint.  
  
Dans les cas extrêmes, la forêt à accès restreint peut être conservée sur un autre réseau physique. Les organisations qui fonctionnent sur des projets gouvernementaux classifié parfois gardent forêts accès restreint sur des réseaux distincts pour répondre aux exigences de sécurité.  
  


