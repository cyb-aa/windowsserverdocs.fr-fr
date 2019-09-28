---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: Modèles de conception de forêt
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 436bbc7038e9797d194f4b4a6ea65d88c0b8279a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402569"
---
# <a name="forest-design-models"></a>Modèles de conception de forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez appliquer l’un des trois modèles de conception de forêt suivants dans votre environnement de Active Directory :  
  
-   Modèle de forêt d’organisation  
  
-   Modèle de forêt de ressources  
  
-   Modèle de forêt à accès restreint  
  
Il est probable que vous devrez utiliser une combinaison de ces modèles pour répondre aux besoins de tous les différents groupes de votre organisation.  
  
## <a name="organizational-forest-model"></a>Modèle de forêt d’organisation  
Dans le modèle de forêt d’organisation, les comptes d’utilisateurs et les ressources sont contenus dans la forêt et gérés indépendamment. La forêt d’organisation peut être utilisée pour assurer l’autonomie du service, l’isolation du service ou l’isolation des données, si la forêt est configurée pour empêcher l’accès à toute personne extérieure à la forêt.  
  
Si les utilisateurs d’une forêt d’organisation doivent accéder à des ressources dans d’autres forêts (ou l’inverse), des relations d’approbation peuvent être établies entre une forêt d’organisation et les autres forêts. Cela permet aux administrateurs d’accorder l’accès aux ressources de l’autre forêt. L’illustration suivante montre le modèle de forêt d’organisation.  
  
![modèles de conception de forêt](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
Chaque Active Directory conception comprend au moins une forêt d’organisation.  
  
## <a name="resource-forest-model"></a>Modèle de forêt de ressources  
Dans le modèle de forêt de ressources, une forêt distincte est utilisée pour gérer les ressources. Les forêts de ressources ne contiennent pas de comptes d’utilisateur autres que ceux requis pour l’administration de service et ceux qui sont requis pour fournir un autre accès aux ressources de cette forêt, si les comptes d’utilisateur de la forêt d’organisation ne sont plus disponibles. Les approbations de forêt sont établies afin que les utilisateurs d’autres forêts puissent accéder aux ressources contenues dans la forêt de ressources. L’illustration suivante montre le modèle de forêt de ressources.  
  
![modèles de conception de forêt](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
Les forêts de ressources fournissent un isolement de service qui est utilisé pour protéger les zones du réseau qui doivent maintenir un état de haute disponibilité. Par exemple, si votre entreprise inclut une fonction de fabrication qui doit continuer à fonctionner en cas de problème sur le reste du réseau, vous pouvez créer une forêt de ressources distincte pour le groupe de fabrication.  
  
## <a name="restricted-access-forest-model"></a>Modèle de forêt à accès restreint  
Dans le modèle de forêt à accès restreint, une forêt distincte est créée pour contenir les comptes d’utilisateur et les données qui doivent être isolés du reste de l’organisation. Les forêts avec accès restreint permettent d’isoler les données dans des situations où les conséquences de la compromission des données de projet sont graves. L’illustration suivante montre un modèle de forêt à accès restreint.  
  
![modèles de conception de forêt](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
Les utilisateurs d’autres forêts ne peuvent pas se voir accorder l’accès aux données confidentielles, car aucune approbation n’existe. Dans ce modèle, les utilisateurs disposent d’un compte dans une forêt d’organisation pour accéder aux ressources générales de l’organisation et à un compte d’utilisateur distinct dans la forêt à accès restreint pour l’accès aux données classées. Ces utilisateurs doivent disposer de deux stations de travail distinctes, une connectée à la forêt de l’organisation et l’autre connectée à la forêt à accès restreint. Cela permet de vous protéger contre la possibilité qu’un administrateur de service d’une forêt puisse accéder à une station de travail dans la forêt restreinte.  
  
Dans les cas extrêmes, la forêt à accès restreint peut être conservée sur un réseau physique distinct. Les organisations qui travaillent sur des projets gouvernementaux classés gèrent parfois des forêts à accès restreint sur des réseaux distincts pour répondre aux exigences de sécurité.  
  


