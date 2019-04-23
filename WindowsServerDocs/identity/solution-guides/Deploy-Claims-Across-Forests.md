---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Déployer des revendications dans les forêts
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890650"
---
# <a name="deploy-claims-across-forests"></a>Déployer des revendications dans les forêts

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans Windows Server 2012, un type de revendication est une assertion sur l’objet avec lequel elle est associée. Les types de revendications sont définis par forêt dans Active Directory. Il existe de nombreux scénarios où un principal de sécurité doit pouvoir franchir une limite d’approbation pour accéder aux ressources dans une forêt approuvée. Transformation des revendications d’inter-forêts dans Windows Server 2012 vous permet de transformer les revendications entrantes et sortantes qui franchissent les forêts afin que les revendications soient reconnues et acceptées dans les forêts approuvées et d’approbation. Voici certains scénarios réels de transformation des revendications :  
  
-   Les forêts de confiance peuvent utiliser la transformation de revendication comme protection contre l’élévation de privilèges en filtrant les revendications entrantes aux valeurs spécifiques.  
  
    Les forêts de confiance peuvent également émettre des revendications pour les principaux franchissant une limite d’approbation si la forêt approuvée ne prend pas de revendication ou n’en n’émet pas.  
  
-   Les forêts approuvées peuvent utiliser la transformation pour empêcher certains types de revendications et des revendications ayant certaines valeurs de sortir de la forêt de confiance.  
  
-   Vous pouvez également utiliser la transformation de revendication pour mapper différents types de revendications entre des forêts approuvées et d’approbation. La transformation permet de généraliser le type et/ou la valeur de la revendication. Sans cela, vous devez normaliser les données entre les forêts avant de pouvoir utiliser les revendications. La généralisation des revendications entre les forêts d’approbation et approuvées réduit les coûts informatiques.  
  
## <a name="claim-transformation-rules"></a>Règles de transformation de revendication  
La syntaxe du langage de règle de transformation divise une règle en deux parties principales : une série d’instructions de condition et l’énoncé. Chaque instruction de condition a deux sous-composants : l’identificateur de revendication et la condition. L’instruction d’émission contient les mots clés, les séparateurs et une expression d’émission. L’instruction de condition peut commencer par une variable d’identificateur de revendication, qui représente la revendication d’entrée correspondante. La condition vérifie l’expression. Si la revendication d’entrée ne correspond pas à la condition, le moteur de transformation ignore l’instruction d’émission et évalue la revendication d’entrée suivante en fonction de la règle de transformation. Si toutes les conditions correspondent à la revendication d’entrée, il traite l’instruction d’émission.  
  
Pour obtenir des informations détaillées sur le langage des règles de revendication, voir [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Associer des stratégies de transformation de revendication à des forêts  
Deux composants sont impliqués dans la définition des stratégies de transformation de revendication : les objets de stratégie de transformation de revendication et le lien de transformation. Les objets de stratégie se trouvent dans le contexte d’appellation de configuration d’une forêt et contiennent des informations de mappage pour les revendications. Le lien spécifie les forêts d’approbation et approuvées auxquelles le mappage s’applique.  
  
Il est important de comprendre s’il s’agit de la forêt d’approbation ou approuvée, car c’est sur cette dernière que repose l’association des objets de stratégie de transformation. Par exemple, la forêt approuvée est la forêt qui contient les comptes d’utilisateurs qui ont besoin d’un accès. La forêt de confiance est la forêt qui contient les ressources auxquelles vous voulez autoriser les utilisateurs à accéder. Les revendications se déplacent dans le même sens que le principal de sécurité qui requiert l’accès. Par exemple, s’il existe une approbation à sens unique à partir de la forêt contoso.com vers la forêt adatum.com, les revendications seront acheminées depuis adatum.com vers contoso.com, ce qui permet aux utilisateurs de adatum.com d’accéder aux ressources de contoso.com.  
  
Par défaut, une forêt approuvée autorise la transmission de toutes les revendications sortantes et une forêt de confiance rejette toutes les revendications entrantes qu’elle reçoit.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Vous trouverez ci-dessous les instructions disponibles pour ce scénario :  
  
-   [Déployer des revendications dans les forêts &#40;étapes de démonstration&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau qui suit décrit les rôles et les fonctionnalités inclus dans ce scénario et détaille la manière dont ils prennent en charge ce dernier.  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|-----------------|---------------------------------|  
|Services de domaine Active Directory|Dans ce scénario, vous devez configurer deux forêts Active Directory avec une approbation bidirectionnelle. Les deux forêts contiennent des revendications. Vous définissez également des stratégies d’accès central sur la forêt de confiance où se trouvent les ressources.|  
|Rôle des Services de fichiers et de stockage|Dans ce scénario, la classification des données est appliquée aux ressources sur les serveurs de fichiers. La stratégie d’accès central est appliquée au dossier auquel vous voulez autoriser l’utilisateur à accéder. Après la transformation, la revendication accorde à l’utilisateur l’accès aux ressources en fonction de la stratégie d’accès central qui s’applique au dossier sur le serveur de fichiers.|  
  


