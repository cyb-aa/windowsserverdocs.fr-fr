---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modèles de conception de forêt
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aca59a31f75628b311a92bd842d93ee91408dc4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819790"
---
# <a name="forest-design-models"></a>Modèles de conception de forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez appliquer un des modèles de conception de trois forêt suivant dans votre environnement Active Directory :  
  
-   Modèle de forêt d’organisation  
  
-   Modèle de forêt de ressources  
  
-   Modèle à accès restreint  
  
Il est probable que vous deviez utiliser une combinaison de ces modèles pour répondre aux besoins de tous les groupes différents de votre organisation.  
  
## <a name="organizational-forest-model"></a>Modèle de forêt d’organisation  
Dans le modèle de forêt d’organisation, les comptes d’utilisateur et les ressources sont contenues dans la forêt et gérés indépendamment. La forêt d’organisation peut être utilisée pour fournir l’autonomie des services, l’isolation de service ou l’isolation de données, si la forêt est configurée pour empêcher l’accès à l’extérieur de la forêt.  
  
Si les utilisateurs dans une forêt d’organisation doivent accéder aux ressources dans d’autres forêts (ou l’inverse), les relations d’approbation peuvent être établies entre une forêt d’organisation et les autres forêts. Cela permet aux administrateurs d’accorder l’accès aux ressources de l’autre forêt. L’illustration suivante montre le modèle de forêt d’organisation.  
  
![modèles de conception de forêt](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Chaque structure Active Directory inclut au moins une forêt d’organisation.  
  
## <a name="resource-forest-model"></a>Modèle de forêt de ressources  
Dans le modèle de forêt de ressources, une forêt distincte est utilisée pour gérer les ressources. Forêts de ressources ne contiennent pas de comptes d’utilisateurs autres que ceux requis pour l’administration des services et celles qui sont requises pour fournir un autre accès aux ressources de cette forêt, si les comptes d’utilisateur dans la forêt d’organisation deviennent indisponibles. Approbations de forêt sont établies afin que les utilisateurs d’autres forêts peuvent accéder aux ressources contenues dans la forêt de ressources. L’illustration suivante montre le modèle de forêt de ressources.  
  
![modèles de conception de forêt](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Forêts de ressources assurent l’isolation de service qui permet de protéger les zones de réseau qui doivent maintenir un état de la haute disponibilité. Par exemple, si votre entreprise inclut une fonctionnalité de fabrication qui doit continuer à fonctionner lorsqu’il existe des problèmes sur le reste du réseau, vous pouvez créer une forêt de ressources distinct pour le groupe de fabrication.  
  
## <a name="restricted-access-forest-model"></a>Modèle à accès restreint  
Dans le modèle de forêt d’un accès restreint, une forêt distincte est créée pour contenir les comptes d’utilisateur et les données qui doivent être isolées du reste de l’organisation. Forêts d’un accès restreint assurent l’isolation des données dans les situations où les conséquences de compromettre les données de projet sont graves. L’illustration suivante montre un modèle à accès restreint.  
  
![modèles de conception de forêt](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Les utilisateurs d’autres forêts ne peut pas avoir accès à des données confidentielles, car aucune relation d’approbation n’existe. Dans ce modèle, les utilisateurs ont un compte dans une forêt d’organisation pour l’accès aux ressources de l’organisation générales et un compte d’utilisateur distinct dans la forêt à accès restreint pour l’accès aux données classifiées. Ces utilisateurs doivent disposer de deux distinctes des stations de travail, sont connectés à la forêt d’organisation et l’autre connectée à la forêt d’un accès restreint. Cela protège contre le risque qu’un administrateur de service à partir d’une seule forêt peut accéder à une station de travail dans la forêt à accès restreint.  
  
Dans les cas extrêmes, la forêt à accès restreint peut être gérée sur un autre réseau physique. Les organisations qui fonctionnent sur des projets government classifiées parfois gardent forêts un accès restreint sur des réseaux distincts pour répondre aux exigences de sécurité.  
  


